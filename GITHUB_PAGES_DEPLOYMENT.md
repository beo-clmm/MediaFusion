# GitHub Pages Deployment for Kodi Addon

## Problem Summary

The release 4.3.35 was created, but the GitHub Actions workflow didn't deploy to GitHub Pages because:
1. The workflow only triggers on **new** release creation events
2. Since the release was already created before the workflow was fully configured, it didn't run
3. No `gh-pages` branch was created, so GitHub Pages showed the README.md from the main branch

## Solution

A new workflow file has been added: `.github/workflows/deploy-gh-pages.yml`

This workflow can be **manually triggered** to deploy the Kodi addon repository to GitHub Pages.

## How to Deploy Now

### Step 1: Trigger the Manual Deployment

1. Go to your repository: https://github.com/beo-clmm/MediaFusion
2. Click on **Actions** tab
3. In the left sidebar, click **Deploy to GitHub Pages**
4. Click **Run workflow** button (on the right)
5. Click the green **Run workflow** button in the dropdown
6. Wait for the workflow to complete (usually takes 1-2 minutes)

### Step 2: Configure GitHub Pages (if not already done)

1. Go to **Settings** → **Pages**
2. Under **Source**, select:
   - **Source**: Deploy from a branch
   - **Branch**: `gh-pages`
   - **Folder**: `/ (root)`
3. Click **Save**

### Step 3: Verify Deployment

After a few minutes, visit:
- https://beo-clmm.github.io/MediaFusion/

You should see:
- A page with download links for the repository zip files
- Files accessible at:
  - `https://beo-clmm.github.io/MediaFusion/addons.xml`
  - `https://beo-clmm.github.io/MediaFusion/addons.xml.md5`
  - `https://beo-clmm.github.io/MediaFusion/repository.mediafusion-4.3.35.zip`

## For Future Releases

For future releases, the automatic workflow will work as expected:

1. Create a new release (e.g., 4.3.36)
2. The main CI/CD workflow will automatically:
   - Build the addon
   - Deploy to GitHub Pages
   - Upload artifacts to the release

You won't need to manually trigger the deployment workflow again unless:
- You want to redeploy without creating a new release
- The automatic workflow fails for some reason

## Installing in Kodi

Once deployed, users can install MediaFusion in Kodi:

### Method 1: Via Repository (Recommended)

1. Open Kodi
2. Settings → File manager → Add source
3. Enter: `https://beo-clmm.github.io/MediaFusion`
4. Name it "MediaFusion"
5. Add-ons → Install from zip file → MediaFusion
6. Install `repository.mediafusion-4.3.35.zip`
7. Install from repository → MediaFusion Repository → Video add-ons
8. Install MediaFusion

### Method 2: Direct Installation

Download the zip from the release page and install directly in Kodi.

## Troubleshooting

### Workflow Fails

Check the Actions tab for error messages. Common issues:
- Missing dependencies (should be installed automatically)
- Invalid addon.xml files

### GitHub Pages Shows README

- Ensure gh-pages branch exists (created by the workflow)
- Check Settings → Pages is configured correctly
- Wait 2-3 minutes after workflow completes

### Can't See Download Links

- Verify the workflow completed successfully
- Check that all files are in the gh-pages branch
- Clear browser cache and try again
