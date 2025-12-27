# Kodi Add-on Packaging Guide

## Overview

This document explains how MediaFusion is packaged for distribution as a Kodi add-on.

## Repository Structure

The Kodi add-on consists of two main components:

1. **Plugin Add-on** (`plugin.video.mediafusion/`) - The actual MediaFusion video add-on
2. **Repository Add-on** (`repository.mediafusion/`) - A repository that hosts the plugin

## How It Works

### For End Users

1. In Kodi, users add the source: `https://beo-clmm.github.io/MediaFusion/`
2. Kodi downloads the repository zip file from that URL
3. Users install the repository add-on
4. The repository provides access to the MediaFusion plugin add-on
5. Users can then install and receive automatic updates for MediaFusion

### Build Process

The build process is automated through `make` and creates the following structure:

```
kodi/dist/
├── addons.xml                                          # XML index of all add-ons
├── addons.xml.md5                                      # MD5 checksum of addons.xml
├── index.html                                          # Landing page with download links
├── plugin.video.mediafusion/
│   ├── addon.xml                                       # Plugin metadata
│   └── plugin.video.mediafusion-X.X.X.zip             # Plugin zip file
├── repository.mediafusion/
│   ├── addon.xml                                       # Repository metadata
│   └── repository.mediafusion-X.X.X.zip               # Repository zip file
└── repository.mediafusion-X.X.X.zip                   # Repository zip (root level)
```

### Building Locally

To build the add-on and repository locally:

```bash
cd kodi
make clean
make all
```

This will:
1. Clean previous build artifacts
2. Create the `build/` directory
3. Install Python dependencies into `lib/`
4. Create the plugin zip file
5. Create the repository zip file
6. Generate the `dist/` directory structure
7. Create `addons.xml` and `addons.xml.md5`
8. Create `index.html`

### Development Build

For development with additional dependencies:

```bash
cd kodi
make dev
```

This creates a development zip with extra debugging tools.

## CI/CD Workflow

When a release is created on GitHub:

1. The workflow checks out the code
2. Builds the add-on using `make -C kodi`
3. Deploys the `kodi/dist/` directory to GitHub Pages (gh-pages branch)
4. Uploads the plugin zip to the release assets

The GitHub Pages deployment makes the repository available at:
`https://beo-clmm.github.io/MediaFusion/`

## Key Files

### addon.xml (Plugin)

Located at `plugin.video.mediafusion/addon.xml`, this defines:
- Add-on ID: `plugin.video.mediafusion`
- Version number
- Dependencies (Python version, other add-ons)
- Provider information
- Metadata (description, icon, fanart)

### addon.xml (Repository)

Located at `repository.mediafusion/addon.xml`, this defines:
- Repository ID: `repository.mediafusion`
- Version number
- URLs for `addons.xml`, `addons.xml.md5`, and zip files
- Repository metadata

**Important**: The URLs in this file must point to the correct GitHub Pages URL:
```xml
<info compressed="false">https://beo-clmm.github.io/MediaFusion/addons.xml</info>
<checksum>https://beo-clmm.github.io/MediaFusion/addons.xml.md5</checksum>
<datadir zip="true">https://beo-clmm.github.io/MediaFusion/</datadir>
```

### generate_repository.py

This script:
1. Reads all `addon.xml` files from `dist/` subdirectories
2. Generates `addons.xml` containing all add-on metadata
3. Creates MD5 checksum for `addons.xml`
4. Duplicates repository entries (Kodi requirement)

### Makefile

Orchestrates the entire build process with these targets:
- `all` - Default target, builds everything
- `clean` - Removes build artifacts
- `dev` - Creates development build
- `prepare_build` - Sets up build directory
- `install_dependencies` - Installs Python packages
- `repository` - Creates repository structure

## Requirements

- Python 3.8+
- `pip` for installing dependencies
- `zip` command-line tool
- Dependencies listed in `requirements.txt`

## GitHub Pages Configuration

The repository must have GitHub Pages enabled with:
- Source: Deploy from a branch
- Branch: `gh-pages`
- Folder: `/ (root)`

The workflow automatically creates and updates the `gh-pages` branch.

## Version Management

The version number is defined in:
1. `plugin.video.mediafusion/addon.xml` - Plugin version
2. `repository.mediafusion/addon.xml` - Repository version

Both should be kept in sync and updated before creating a release.

### How to Update Versions

1. Edit both `addon.xml` files to update the version attribute:
   ```xml
   <addon id="plugin.video.mediafusion" version="X.X.X" ...>
   ```
2. Commit the changes
3. Create a git tag matching the version: `git tag X.X.X`
4. Create a GitHub release using that tag
5. The CI workflow will build and deploy with the new version

### What Happens if Versions Get Out of Sync

- If repository version doesn't match plugin version, users may see inconsistent version numbers
- Kodi will still function, but updates may not be detected properly
- The CI/CD workflow uses the version from `addon.xml` to name the zip files
- Keep versions synchronized to avoid confusion and ensure proper update detection

## Troubleshooting

### Build fails with missing dependencies
- Ensure Python 3.8+ is installed
- Check that `pip` is available
- Verify `requirements.txt` is present

### URLs point to wrong location
- Check `repository.mediafusion/addon.xml`
- Ensure URLs point to `https://beo-clmm.github.io/MediaFusion/`
- Rebuild after changing URLs

### Add-on not appearing in Kodi
- Verify GitHub Pages is enabled
- Check that `addons.xml` is accessible at the URL
- Ensure repository version matches the built version
- Try force-refreshing the repository in Kodi

## References

- [Kodi Add-on Development](https://kodi.wiki/view/Add-on_development)
- [Kodi Repository Structure](https://kodi.wiki/view/Add-on_repositories)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
