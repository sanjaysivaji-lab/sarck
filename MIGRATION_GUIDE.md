# Migration Guide: Moving Images to Local Repository

This guide walks you through the steps to download your images from sarckfusion.com and add them to the repository locally.

## Step 1: Download Images from Your Website

### For GoDaddy Website Builder Users:

1. **Open your website** at https://sarckfusion.com in a web browser
2. **Right-click on each image** you want to save
3. Select **"Save image as..."** (Chrome/Firefox) or **"Save image..."** (Safari)
4. Save to a folder on your computer (e.g., `Downloads/sarck-images/`)

### Alternative: Bulk Download All Images

If you want to download many images at once:

1. **Install a browser extension:**
   - Chrome: [Image Downloader](https://chrome.google.com/webstore/detail/image-downloader/iieboppjgbnfglkkcbjgeildglgjbjel)
   - Firefox: DownThemAll!

2. **Run the extension** on your website
3. **Select all images** and download them to a folder

---

## Step 2: Organize Downloaded Images

Once downloaded, organize your images into these folders:

```
downloaded-images/
├── logo/
│   ├── sarck-fusion-logo.jpg
│   └── brand-header.jpg
├── products/
│   ├── product-tomato.jpg
│   ├── product-lettuce.jpg
│   └── product-carrots.jpg
├── gallery/
│   ├── farm-field-1.jpg
│   ├── harvest-2.jpg
│   └── produce-display-3.jpg
├── backgrounds/
│   ├── hero-background.jpg
│   └── section-background.jpg
└── maps/
    └── location-map.png
```

### File Naming Rules:
- Use **lowercase** only: `my-image.jpg` (not `My-Image.jpg`)
- Use **hyphens** for spaces: `product-photo.jpg` (not `product photo.jpg`)
- Be **descriptive**: `tomato-fresh-organic.jpg` (not `img123.jpg`)
- **Include extensions**: `.jpg`, `.png`, `.webp`

---

## Step 3: Upload Images to GitHub Repository

### Method A: Upload via GitHub Web Interface (Easiest)

1. Go to your repository: https://github.com/sanjaysivaji-lab/sarck
2. Click **"Add file"** → **"Upload files"**
3. Drag and drop images, or click to select them
4. **Create folders as needed:**
   - Upload to `images/logo/` first (create folder if needed)
   - Then `images/products/`, etc.
5. Add a commit message: `Add product and gallery images`
6. Click **"Commit changes"**

### Method B: Upload via Git Command Line (Advanced)

```bash
# Clone your repository
git clone https://github.com/sanjaysivaji-lab/sarck.git
cd sarck

# Copy your images into the images/ folder
# Example:
cp ~/Downloads/sarck-images/logo/*.jpg images/logo/
cp ~/Downloads/sarck-images/products/*.jpg images/products/

# Add all images to git
git add images/

# Commit with a message
git commit -m "Add images for products, gallery, and backgrounds"

# Push to GitHub
git push origin main
```

---

## Step 4: Update HTML References

After uploading images, update your `index.html` file to use local image paths.

### Find and Replace Pattern:

**BEFORE (External URL):**
```html
<img src="https://img1.wsimg.com/isteam/ip/4c6aeb25-0fb3-4a50-817a-3204201e5263/SARCK%20FUSION.jpg" alt="Logo">
```

**AFTER (Local Path):**
```html
<img src="images/logo/sarck-fusion-logo.jpg" alt="Logo">
```

### Quick Replacement Guide:

| Image Type | Original URL Contains | Replace With |
|------------|----------------------|---------------|
| Logo | `SARCK%20FUSION.jpg` | `images/logo/sarck-fusion-logo.jpg` |
| Product images | `cr=w_600,h_300` | `images/products/product-*.jpg` |
| Background images | `rs=w_700,cg_true` | `images/backgrounds/hero-bg.jpg` |
| Gallery images | `/isteam/ip/` + UUID | `images/gallery/gallery-*.jpg` |
| Location map | Generic image URL | `images/maps/location-map.png` |

### Using Find & Replace in Code Editor:

1. Open `index.html` in your text editor (VS Code, Sublime Text, etc.)
2. **Ctrl+H** (or **Cmd+H** on Mac) to open Find & Replace
3. Find: `https://img1.wsimg.com/isteam/`
4. Replace with: `images/`
5. Click **"Replace All"**

---

## Step 5: Test Your Changes

### Test Locally:

1. **Open `index.html` in a browser** (drag and drop the file into browser)
2. **Check if all images display correctly**
3. Look for broken image icons (broken links)

### Test on GitHub Pages:

1. Push your updated `index.html` to GitHub
2. Visit your live site: https://sarckfusion.com
3. Verify all images are visible
4. Open browser **Developer Tools (F12)** → **Console** tab
5. Look for any "404" or "Failed to load" errors

---

## Step 6: Fix Maps & Direction Images

If your site has embedded maps (Google Maps, etc.):

### For Google Maps Embedding:

1. Locate the Google Maps embed code in `index.html`
2. Ensure the **API key** is valid:
   ```html
   <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY"></script>
   ```
3. Verify the key has **Maps JavaScript API** enabled in Google Cloud Console
4. Add your domain (sarckfusion.com) to authorized domains

### For Static Map Images:

1. Save the map screenshot/image to `images/maps/`
2. Reference it as: `<img src="images/maps/location-map.png" alt="Location Map">`

---

## Step 7: Image Optimization (Optional)

For faster loading, optimize your images:

### Online Tools:
- [TinyPNG](https://tinypng.com/) - Compress PNG & JPEG
- [ImageOptim](https://imageoptim.com/) - Mac application
- [FileZilla](https://filezilla-project.org/) - Batch compression

### Save File Sizes:
- JPG: 40-200 KB per image
- PNG: 30-150 KB per image
- WebP: 20-100 KB per image (better compression)

---

## Common Issues & Solutions

### Issue: Images don't display after upload

**Solution:**
- Check file path capitalization (must match exactly)
- Verify file extensions (.jpg, .png)
- Check for typos in image file names
- Wait 5-10 minutes for GitHub Pages cache to update

### Issue: Images show as broken links

**Solution:**
- Verify image files actually exist in the repository
- Check browser console for 404 errors
- Ensure paths start with `images/` not `/images/`

### Issue: Map doesn't show

**Solution:**
- If using Google Maps, check API key validity
- Verify domain is whitelisted in Google Cloud Console
- Try alternative: Use a static image instead

### Issue: Images load slowly

**Solution:**
- Compress images further
- Consider using WebP format
- Move very large images (>1MB) to separate CDN

---

## Troubleshooting Commands

If using Git command line:

```bash
# Check if images were added
git status

# View image files in repository
ls -la images/

# Check for broken image references
grep -r "img1.wsimg.com" index.html

# Test locally with Python HTTP server
python3 -m http.server 8000
# Then visit: http://localhost:8000
```

---

## Support

- **GitHub Pages Documentation:** https://docs.github.com/en/pages
- **GitHub Help:** https://docs.github.com/en
- **Contact Repository Owner:** sanjaysivaji-lab

Once complete, your images will be:
✅ Hosted locally on GitHub
✅ Faster loading (no external dependencies)
✅ More reliable (no external service failures)
✅ Easier to maintain and update

---

**Last Updated:** 2026-07-14
**Repository:** https://github.com/sanjaysivaji-lab/sarck
