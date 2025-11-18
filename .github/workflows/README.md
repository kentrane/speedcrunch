# GitHub Actions Workflows

This directory contains automated build and release workflows for SpeedCrunch.

## Workflows

### Build (`build.yml`)

**Triggers:**
- Push to `main`, `master`, or any `claude/**` branch
- Pull requests to `main` or `master`
- Manual workflow dispatch
- Release creation

**What it does:**
- Builds SpeedCrunch on Windows, macOS, and Linux
- Runs all tests via CTest
- Creates platform-specific packages:
  - **Linux**: AppImage (portable, runs on most distributions)
  - **Windows**: ZIP archive with all dependencies
  - **macOS**: DMG disk image
- Uploads build artifacts (available for 30 days)
- On releases: Automatically attaches binaries to the release

**Build matrix:**
- Ubuntu Latest (Linux x86_64)
- Windows Latest (Windows x64)
- macOS Latest (macOS Universal)

All builds use Qt 5.15.2 for consistency and stability.

### Release (`release.yml`)

**Triggers:**
- Push of version tags (e.g., `v1.0.0`, `release-1.0`)

**What it does:**
- Builds release-ready binaries for all platforms
- Creates packages with proper versioning
- Automatically uploads to GitHub Releases

## Creating a Release

To create a new release with automated binaries:

1. Tag your commit with a version:
   ```bash
   git tag -a v1.0.0 -m "Release version 1.0.0"
   git push origin v1.0.0
   ```

2. The `release.yml` workflow will automatically:
   - Build for all platforms
   - Create packages
   - Attach them to the GitHub release

3. Edit the release on GitHub to add release notes

## Testing Locally

To test builds locally before pushing:

```bash
# Linux/macOS
mkdir build && cd build
cmake ../src -DCMAKE_BUILD_TYPE=Release
make
ctest --output-on-failure

# Windows (with Visual Studio)
mkdir build && cd build
cmake ../src -DCMAKE_BUILD_TYPE=Release -G "Visual Studio 17 2022"
cmake --build . --config Release
ctest -C Release --output-on-failure
```

## Workflow Features

- ✅ Multi-platform builds (Linux, Windows, macOS)
- ✅ Automated testing
- ✅ Qt installation via cache
- ✅ Platform-specific packaging
- ✅ Artifact uploads
- ✅ Automatic release binary attachment
- ✅ Parallel builds with fail-fast disabled
- ✅ Build status badges in README

## Requirements

The workflows automatically install all required dependencies:
- Qt 5.15.2
- CMake (provided by runner)
- Platform-specific compilers
- Packaging tools (linuxdeploy, windeployqt, macdeployqt)

No manual setup required!
