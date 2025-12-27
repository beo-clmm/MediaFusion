# Quick Start: Enabling Kodi Add-on Distribution

## What You Need to Do

This repository is already configured to build and distribute MediaFusion as a Kodi add-on. To make it available to users, you need to enable GitHub Pages.

## Step 1: Enable GitHub Pages

1. Go to your repository on GitHub: `https://github.com/beo-clmm/MediaFusion`
2. Click on **Settings** (⚙️)
3. In the left sidebar, click **Pages**
4. Under **Source**, select:
   - **Source**: Deploy from a branch
   - **Branch**: `gh-pages`
   - **Folder**: `/ (root)`
5. Click **Save**

GitHub will automatically deploy the Kodi add-on repository to:
`https://beo-clmm.github.io/MediaFusion/`

## Step 2: Create a Release

The GitHub Actions workflow automatically builds and deploys the Kodi add-on when you create a release:

1. Go to **Releases** in your repository
2. Click **Create a new release**
3. Choose or create a tag (e.g., `4.3.35`)
4. Fill in release details
5. Click **Publish release**

The workflow will:
- Build the Kodi add-on
- Create zip files
- Deploy to GitHub Pages
- Attach the plugin zip to the release

## Step 3: Install in Kodi

Once GitHub Pages is enabled and a release is created, users can install MediaFusion in Kodi:

### Method 1: Via Repository (Recommended)

1. Open Kodi
2. Go to Settings → File manager
3. Click "Add source"
4. Enter: `https://beo-clmm.github.io/MediaFusion`
5. Name it "MediaFusion"
6. Go to Add-ons → Install from zip file
7. Select MediaFusion → Install `repository.mediafusion-X.X.X.zip`
8. Go to Install from repository → MediaFusion Repository → Video add-ons
9. Install MediaFusion

### Method 2: Manual Installation

1. Download `plugin.video.mediafusion-X.X.X.zip` from the releases page
2. In Kodi: Add-ons → Install from zip file
3. Select the downloaded zip file

## Verifying It Works

After enabling GitHub Pages, verify it's working:

1. Visit: `https://beo-clmm.github.io/MediaFusion/`
   - You should see a page with download links
2. Check: `https://beo-clmm.github.io/MediaFusion/addons.xml`
   - You should see XML content with repository and plugin info
3. Check: `https://beo-clmm.github.io/MediaFusion/repository.mediafusion-X.X.X.zip`
   - Should download the repository zip file (replace X.X.X with actual version)

## Troubleshooting

### GitHub Pages not showing content
- Wait a few minutes after enabling (initial deployment takes time)
- Check the **Actions** tab for workflow status
- Ensure the `gh-pages` branch exists

### URLs point to wrong location
- The URLs have been updated to `beo-clmm.github.io`
- If you need different URLs, edit `kodi/repository.mediafusion/addon.xml`

### No release artifacts
- Ensure you've created a release (not just a tag)
- Check the **Actions** tab for workflow status
- The workflow only runs on release creation

## Technical Details

For more information about the packaging process, see [PACKAGING.md](./PACKAGING.md)

## Support

If you encounter issues:
1. Check the **Actions** tab for workflow errors
2. Verify GitHub Pages is enabled and set to `gh-pages` branch
3. Ensure you've created at least one release
4. Review the [PACKAGING.md](./PACKAGING.md) documentation
