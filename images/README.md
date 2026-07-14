# Images Directory

This directory contains all images used on the Sarck Fusion website.

## Folder Structure

```
images/
├── logo/                 # Logo and branding images
│   ├── logo.jpg
│   ├── favicon.ico
│   └── brand-image.jpg
├── products/             # Product images
│   ├── product-1.jpg
│   ├── product-2.jpg
│   └── product-3.jpg
├── gallery/              # Gallery and showcase images
│   ├── gallery-1.jpg
│   ├── gallery-2.jpg
│   └── gallery-3.jpg
├── maps/                 # Map and location images
│   ├── location-map.png
│   └── directions.png
└── backgrounds/          # Background images
    ├── hero-bg.jpg
    └── section-bg.jpg
```

## How to Add Images

1. **Add image files** to the appropriate subfolder based on their use
2. **Update `../index.html`** to reference the new image paths
3. **Use relative paths** in your HTML:
   ```html
   <img src="images/logo/logo.jpg" alt="Sarck Fusion Logo">
   <img src="images/products/product-1.jpg" alt="Product 1">
   ```

## Image Naming Convention

- Use lowercase, hyphenated names (e.g., `hero-banner.jpg`, not `HeroBanner.jpg`)
- Be descriptive (e.g., `tomato-fresh-harvest.jpg` rather than `img1.jpg`)
- Include dimensions in name if needed (e.g., `hero-1920x600.jpg`)

## File Formats

- **JPG/JPEG**: For photographs and complex images with many colors
- **PNG**: For images with transparency or graphics
- **WebP**: For optimized web delivery (optional, but recommended)

## Current External Images to Replace

The following external images from `img1.wsimg.com` need to be downloaded and added to this directory:

1. **Logo/Branding**
   - SARCK FUSION.jpg → `images/logo/sarck-fusion.jpg`

2. **Product Images** (multiple variations)
   - Replace external product URLs → `images/products/product-*.jpg`

3. **Background Images**
   - Hero backgrounds → `images/backgrounds/hero-bg.jpg`
   - Section backgrounds → `images/backgrounds/section-bg.jpg`

4. **Map/Location Images**
   - Location maps → `images/maps/location-map.png`
   - Direction images → `images/maps/directions.png`

5. **Gallery Images**
   - Showcase images → `images/gallery/gallery-*.jpg`

## Steps to Complete Migration

1. **Download images** from your original website (or export from your web builder)
2. **Optimize images** (compress with tools like TinyPNG)
3. **Add files** to appropriate subfolders here
4. **Update `index.html`** with new local paths (see examples below)
5. **Test** the website to ensure all images display correctly

## Example HTML Updates

### Before (External):
```html
<img src="https://img1.wsimg.com/isteam/ip/4c6aeb25-0fb3-4a50-817a-3204201e5263/SARCK%20FUSION.jpg" alt="Sarck Fusion">
```

### After (Local):
```html
<img src="images/logo/sarck-fusion.jpg" alt="Sarck Fusion">
```

## Support

For GitHub Pages to serve images correctly:
- Ensure file extensions are correct (.jpg, .png, etc.)
- Use lowercase in file paths
- Avoid special characters in filenames
- Test locally before pushing to GitHub
