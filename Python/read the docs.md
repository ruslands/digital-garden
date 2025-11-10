# Read the Docs: Documentation Deployment Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Setting Up Read the Docs](#setting-up-read-the-docs)
3. [Project Configuration](#project-configuration)
4. [Building Documentation](#building-documentation)
5. [Publishing to PyPI](#publishing-to-pypi)

---

## Introduction

Read the Docs is a popular platform for hosting documentation automatically built from your source code. This guide covers the process of setting up documentation deployment on Read the Docs and publishing Python packages to PyPI.

---

## Setting Up Read the Docs

### Prerequisites

- A GitHub account
- A project repository with documentation source files
- Documentation built using Sphinx, MkDocs, or similar tools

### Initial Setup Steps

After merging your branch into master, follow these steps:

1. **Log in to Read the Docs**
   - Visit [Read the Docs](https://readthedocs.org/)
   - Log in using your GitHub account
   - This allows Read the Docs to access your repositories

2. **Import Your Project**
   - Click "Import Project"
   - Select "Chronobiology" (or your project name)
   - Read the Docs will scan your GitHub repositories

---

## Project Configuration

### Admin Panel Settings

Navigate to your project's "Admin" menu and configure the following:

#### Basic Information

- **Project Name**: "Chronobiology"
  - Note: If you change this in the future, update the link in your README file
- **Description**: "A python package to calculate and plot circadian cycles data."
- **Language**: English
- **Programming Language**: Python
- **Homepage**: `https://github.com/ruslands/Chronobiology`

#### Tags

Add relevant tags for discoverability:
- `actogram`
- `analysis`
- `chronobiology`
- `periodogram`
- `sleep`

### Advanced Settings

1. Navigate to the "Advanced Settings" tab
2. **Disable PDF Builds** (if not needed):
   - Uncheck "Enable PDF build"
   - This speeds up builds and reduces resource usage
3. Save your changes

---

## Building Documentation

### Triggering a Build

1. Go to the "Overview" page of your project
2. Click "Build Version"
3. This will trigger the documentation build process
4. Wait for the build to complete (you can monitor progress)

### Verifying the Build

1. Click "View" to preview your documentation
2. Verify that all content rendered correctly
3. Check that all links work properly
4. Ensure code examples display correctly

### Automatic Builds

Read the Docs automatically rebuilds your documentation when you:
- Push commits to your default branch
- Create new tags/releases
- Manually trigger a build

---

## Publishing to PyPI

PyPI (Python Package Index) is the official repository for Python packages. Follow these steps to publish your package.

### Prerequisites

Ensure you have:
- A PyPI account ([create one here](https://pypi.org/account/register/))
- Your package properly configured with `setup.py` or `pyproject.toml`
- All dependencies listed correctly

### Step 1: Install Build Tools

Install or upgrade the required tools:

```bash
# Install/upgrade build tool
python3 -m pip install --upgrade build

# Install/upgrade twine (for uploading)
python3 -m pip install --upgrade twine
```

**What these tools do:**
- **build**: Creates distribution packages (wheel and source distributions)
- **twine**: Securely uploads packages to PyPI

### Step 2: Build Your Package

Create distribution files:

```bash
python3 -m build
```

This command will:
- Create a `dist/` directory
- Generate both wheel (`.whl`) and source (`.tar.gz`) distributions
- Include all necessary files from your package

**Output:**
```
dist/
  ├── your_package-0.1.0-py3-none-any.whl
  └── your_package-0.1.0.tar.gz
```

### Step 3: Upload to PyPI

Upload your package using twine:

```bash
python3 -m twine upload dist/*
```

**What happens:**
- Twine will prompt for your PyPI credentials
- Uploads both wheel and source distributions
- Validates your package before uploading

**Alternative: Upload to Test PyPI First**

Test your upload process on Test PyPI before publishing to production:

```bash
# Upload to test repository
python3 -m twine upload --repository testpypi dist/*

# If successful, upload to production
python3 -m twine upload dist/*
```

### Complete Workflow Example

```bash
# 1. Ensure you're in your project directory
cd /path/to/your/package

# 2. Clean previous builds (optional)
rm -rf dist/ build/ *.egg-info

# 3. Install/upgrade tools
python3 -m pip install --upgrade build twine

# 4. Build the package
python3 -m build

# 5. Check the distribution (optional)
python3 -m twine check dist/*

# 6. Upload to PyPI
python3 -m twine upload dist/*
```

### Post-Upload

After successful upload:

1. **Verify on PyPI**: Visit `https://pypi.org/project/your-package-name/`
2. **Test Installation**: Try installing your package:
   ```bash
   pip install your-package-name
   ```
3. **Update Documentation**: Ensure your README and documentation reflect the published version

---

## Best Practices

### Version Management

- Use semantic versioning (e.g., `1.2.3`)
- Update version in `setup.py` or `pyproject.toml` before each release
- Tag releases in Git for better tracking

### Security

- Never commit credentials to your repository
- Use API tokens instead of passwords when possible
- Consider using `keyring` for secure credential storage:
  ```bash
  pip install keyring
  twine upload dist/*  # Will use keyring for credentials
  ```

### Documentation

- Keep your README up to date
- Include installation instructions
- Provide usage examples
- Link to your Read the Docs documentation

### Testing

- Test your package installation before uploading:
  ```bash
  pip install dist/your_package-*.whl
  ```
- Run your test suite
- Verify all dependencies install correctly

---

## Troubleshooting

### Common Issues

**Build Fails:**
- Check your `setup.py` or `pyproject.toml` syntax
- Ensure all required files are included
- Verify MANIFEST.in if using source distributions

**Upload Fails:**
- Verify your PyPI credentials
- Check if package name is already taken
- Ensure version number is unique (can't re-upload same version)

**Documentation Not Updating:**
- Check build logs in Read the Docs
- Verify webhook is configured correctly
- Ensure `.readthedocs.yml` is correct (if using)

---

## Summary

This guide covered:

1. **Read the Docs Setup**: Import project, configure settings, and build documentation
2. **PyPI Publishing**: Install tools, build distributions, and upload to PyPI

Following these steps ensures your Python package is properly documented and easily installable by users worldwide.

**Key Takeaways:**
- Read the Docs automates documentation hosting and updates
- PyPI makes your package installable via `pip install`
- Always test before publishing to production
- Keep documentation and package versions synchronized
