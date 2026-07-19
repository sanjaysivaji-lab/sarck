```mermaid
graph TD
    %% Define System States Styles
    classDef stateStyle fill:#f9f,stroke:#333,stroke-width:2px;
    classDef processStyle fill:#bbf,stroke:#333,stroke-width:1px;
    classDef riskStyle fill:#ffb3b3,stroke:#333,stroke-width:1px;
    
    %% Engine Initialization
    Start([System Start]) --> Init[Initialize Connections & Inputs<br>Instrument, Lots, Initial Direction]
    Init --> LoopStart{New Market Tick Received?<br>Fetch LTP via Option A Engine}
    
    %% Circuit Breakers Block
    LoopStart -- Yes --> CalcPNL[Calculate Unrealized P&L<br>Unified Math: LTP - Entry Price]
    CalcPNL --> MasterCheck{Master P&L Goal Met?<br>Realized + Open >= 5000 or <= -5000}
    
    MasterCheck -- Yes --> MasterShutdown[Execute Market Liquidation<br>Log Terminal Shutdown Row to Excel]
    MasterShutdown --> End([System Terminated])
    
    %% State Machine Routing
    MasterCheck -- No --> StateRoute{Current State?}
    
    %% State: IDLE
    StateRoute -- IDLE --> IdleState[Record Entry Reference Prices<br>Reset Strike Counter = 0]
    IdleState --> OrderEntry[Execute BUY Order]
    OrderEntry --> LogEntry[Log ENTRY to Excel]
    LogEntry --> SetActive[Set State = ACTIVE_POSITION]
    SetActive --> Hold[Wait 1.5s for Next Tick]
    Hold --> LoopStart
    
    %% State: ACTIVE_POSITION
    StateRoute -- ACTIVE_POSITION --> TPCheck{Unrealized P&L >= 2000?}
    
    %% Take Profit Pathway
    TPCheck -- Yes --> TPExt[Execute SELL Order]
    TPExt --> LogTP[Log TAKE_PROFIT_CLOSE to Excel]
    LogTP --> RolloverFetch[Fetch Fresh Tick Price]
    RolloverFetch --> RolloverOrder[Execute BUY Order Continuation]
    RolloverOrder --> LogRoll[Log ROLLOVER_ENTRY to Excel]
    LogRoll --> ResetActive[Reset Entry References & Strikes = 0]
    ResetActive --> Hold
    
    %% Stop Loss Pathway
    TPCheck -- No --> SLCheck{Unrealized P&L <= -1500?}
    SLCheck -- Yes --> SLExt[Execute SELL Order]
    SLExt --> LogSL[Log EMERGENCY_STOP_LOSS to Excel]
    LogSL --> SetReversal[Set State = REVERSAL_WAIT]
    SetReversal --> Hold
    
    %% 3-Strike Risk Processing
    SLCheck -- No --> HighWater{Return >= 60% AND<br>LTP > Previous Base Ref?}
    HighWater -- Yes --> LockFloor[Lock New Entry Base Price Ref = LTP<br>Reset Strikes = 0]
    LockFloor --> Hold
    
    HighWater -- No --> Recovery{High-Water % > -3%<br>AND Strikes > 0?}
    Recovery -- Yes --> DecayStrike[Decay Counter: Strikes = Strikes - 1]
    DecayStrike --> Hold
    
    Recovery -- No --> StrikeMatrix{Adverse Drawdown Check}
    StrikeMatrix -- Drawdown <= -9% --> Strike3[Set Strikes = 3]
    StrikeMatrix -- Drawdown <= -6% --> Strike2[Set Strikes = 2]
    StrikeMatrix -- Drawdown <= -3% --> Strike1[Set Strikes = 1]
    
    Strike3 & Strike2 & Strike1 --> FlipCheck{Strikes >= 3?}
    
    FlipCheck -- No --> Hold
    
    %% 3-Strike Flip Execution
    FlipCheck -- Yes --> StrikeExt[Execute SELL Order]
    StrikeExt --> LogStrike[Log STRIKE_OUT_EXIT to Excel]
    LogStrike --> SetReversal
    
    %% State: REVERSAL_WAIT
    StateRoute -- REVERSAL_WAIT --> FlipDirection[Invert Vector Label<br>CALL -> PUT / PUT -> CALL]
    FlipDirection --> ResetIdle[Set State = IDLE]
    ResetIdle --> Hold

    class IDLE,ACTIVE_POSITION,REVERSAL_WAIT stateStyle;
