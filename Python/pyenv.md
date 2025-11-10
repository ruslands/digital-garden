# Pyenv: Python Version Management Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Basic Usage](#basic-usage)
4. [Integration with Poetry](#integration-with-poetry)
5. [VS Code Integration](#vs-code-integration)
6. [Troubleshooting](#troubleshooting)
7. [PATH Configuration](#path-configuration)

---

## Introduction

Pyenv is a simple, powerful Python version management tool. It allows you to easily switch between multiple versions of Python, ensuring you can work with different Python versions for different projects without conflicts.

**Key Benefits:**
- Install and manage multiple Python versions
- Set Python version per project or globally
- Automatically switch versions based on project
- Works seamlessly with virtual environment tools like Poetry

---

## Installation

### Step 1: Install Pyenv

**Using the official installer:**

```bash
curl https://pyenv.run | bash
```

This script installs pyenv and sets up the necessary environment variables.

### Step 2: Configure Shell

**For Zsh (macOS default):**

Add to `~/.zshrc`:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
source ~/.zshrc
```

**For Bash:**

Add to `~/.bashrc`:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
source ~/.bashrc
```

**What These Commands Do:**
- Set `PYENV_ROOT` to pyenv's installation directory
- Add pyenv to your PATH
- Initialize pyenv in your shell session

### Step 3: Install Build Dependencies

**For Linux/Debian-based systems:**

Before installing Python versions, install necessary build packages:

```bash
apt-get update &&
apt-get install libffi-dev libreadline-dev libsqlite3-dev liblzma-dev libssl-dev libbz2-dev libncurses5-dev
```

**Why These Are Needed:**
These packages provide libraries and headers required to compile Python from source.

---

## Basic Usage

### Check Available Versions

**List all installable Python versions:**

```bash
pyenv install --list
```

**Check current version:**

```bash
pyenv version
```

**List all installed versions:**

```bash
pyenv versions
```

**Find which Python executable is being used:**

```bash
pyenv which python
```

### Install Python Versions

**Install a specific Python version:**

```bash
pyenv install 3.11.0
```

**Note:** Installation compiles Python from source, which may take several minutes.

### Set Python Version

Pyenv provides three levels of version specification:

#### 1. Local (Project-Specific)

Sets Python version for the current directory and subdirectories:

```bash
pyenv local 3.11.0
```

This creates a `.python-version` file in the current directory.

**Use Case:** Different projects requiring different Python versions.

#### 2. Global (System-Wide)

Sets the default Python version for your user:

```bash
pyenv global 3.11.0
```

**Alternative syntax:**

```bash
pyenv global /root/.pyenv/versions/3.11.0/bin/python
```

**Use Case:** Set your preferred default Python version.

#### 3. Shell (Session-Specific)

Sets Python version for the current shell session only:

```bash
export PYENV_VERSION=3.11.0
# or
pyenv shell 3.11.0
```

**Use Case:** Temporary version switching for testing.

**Priority Order:**
1. `PYENV_VERSION` (shell)
2. `.python-version` (local)
3. `~/.pyenv/version` (global)

---

## Integration with Poetry

Poetry is a modern dependency management tool for Python. Here's how to integrate it with pyenv.

### Configure Poetry to Use Active Python

**Set Poetry to prefer the active Python version:**

```bash
poetry config virtualenvs.prefer-active-python=true
```

**What This Does:**
Poetry will use the Python version specified by pyenv instead of searching for its own.

### Set Poetry Environment

**Use a specific Python version with Poetry:**

```bash
# Using version number
poetry env use 3.11.0

# Using full path
poetry env use /full/path/to/python

# Using pyenv path
poetry env use /root/.pyenv/versions/3.11.0/bin/python
```

### Check Environment Information

**Display Poetry environment details:**

```bash
poetry env info
```

**List all Poetry environments:**

```bash
poetry env list
```

**Additional Commands:** [Poetry Environment Management](https://python-poetry.org/docs/managing-environments/)

### Common Workflow

**Complete setup process:**

```bash
# 1. Install Python version with pyenv
pyenv install 3.11.5

# 2. Set local version for project
pyenv local 3.11.5

# 3. Configure Poetry
poetry config virtualenvs.prefer-active-python=true

# 4. Create/use Poetry environment
poetry env use 3.11.5
```

---

## VS Code Integration

### Checking Environment Info

**Verify your environment setup:**

```bash
poetry env info
```

**Output shows:**
- Python version
- Virtual environment path
- System information

### Setting Poetry Environment Manually

If Poetry doesn't detect the correct Python version:

```bash
poetry env use /Users/konovalov/.pyenv/versions/3.11.5/bin/python
```

---

## Troubleshooting

### Error: Version Not Installed

**Problem:**
```
pyenv: version `3.11.5' not installed
```

**Solution:**

Manually specify the actual installed version:

```bash
pyenv shell 3.11.0  # Use an installed version
```

Or install the required version:

```bash
pyenv install 3.11.5
```

### Poetry Environment Issues

**If Poetry environment creation fails:**

1. **Delete Poetry cache:**

```bash
rm -rf /root/.cache/pypoetry/virtualenvs
```

2. **Manually specify Python path:**

```bash
poetry env use /root/.pyenv/versions/3.11.0/bin/python
```

3. **Create virtual environment manually:**

```bash
python3 -m venv /root/.cache/pypoetry/virtualenvs/-HkBvPgAz-py3.11
$VENV_PATH/bin/pip install -U pip setuptools
$VENV_PATH/bin/pip install poetry
```

---

## PATH Configuration

### Understanding PATH

When you call `python` or `pip`, the system looks for the interpreter in the `PATH` variable. The first matching path is used.

**To use a specific virtual environment:**

Ensure the desired interpreter path is first in PATH:

```bash
export PATH=/Users/konovalov/1.code/seller/backend/venv/bin:$PATH
```

This ensures packages are read from the correct virtual environment.

### Example PATH Configuration

A typical PATH might look like:

```
/Users/konovalov/.pyenv/versions/3.11.5/bin:
/Users/konovalov/.nvm/versions/node/v16.19.0/bin:
/usr/local/opt/gradle/bin:
/usr/local/opt/maven/bin:
/usr/local/opt/ant/bin:
/usr/local/bin/sdkmanager:
/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/:
/Users/konovalov/.local/bin:
/Users/konovalov/.pyenv/shims:
/Users/konovalov/.pyenv/bin:
/Users/konovalov/.zplug/bin:
/Users/konovalov/.pyenv/versions/3.11.5/bin:
/Library/Frameworks/Python.framework/Versions/3.11/bin:
/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

**Key Points:**
- Pyenv shims (`~/.pyenv/shims`) intercept Python commands
- Shims redirect to the correct Python version based on pyenv settings
- Virtual environment paths should come before system Python paths

### Pyenv Shims

Pyenv uses "shims" - lightweight executables that intercept Python commands and route them to the correct version.

**How It Works:**
1. `pyenv init` adds shims to PATH
2. When you run `python`, the shim intercepts it
3. Shim checks `.python-version` or global settings
4. Shim executes the correct Python version

---

## Best Practices

### Version Management

1. **Use `.python-version` files:** Commit these to your repository so team members use the same version
2. **Set global to a stable version:** Use a recent stable version as your global default
3. **Install only what you need:** Don't install every Python version - only those you actually use

### Poetry Integration

1. **Always set `prefer-active-python`:** Ensures Poetry respects pyenv versions
2. **Check environment info:** Use `poetry env info` to verify correct Python version
3. **Clear cache when needed:** If environments get corrupted, clear Poetry's cache

### Project Setup

**Recommended workflow for new projects:**

```bash
# 1. Navigate to project directory
cd /path/to/project

# 2. Install required Python version (if not already installed)
pyenv install 3.11.5

# 3. Set local version
pyenv local 3.11.5

# 4. Configure Poetry
poetry config virtualenvs.prefer-active-python=true

# 5. Initialize Poetry (if new project)
poetry init

# 6. Create environment
poetry env use 3.11.5

# 7. Install dependencies
poetry install
```

---

## Additional Resources

- [Pyenv GitHub Repository](https://github.com/pyenv/pyenv)
- [Pyenv Commands Documentation](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-latest)
- [Downgrading Python Version (Stack Overflow)](https://stackoverflow.com/questions/62898911/how-to-downgrade-python-version-from-3-8-to-3-7-mac)

---

## Summary

Pyenv provides powerful Python version management:

- **Installation**: Simple curl-based installer with shell configuration
- **Version Management**: Local, global, and shell-level version control
- **Poetry Integration**: Seamless integration with modern dependency management
- **PATH Handling**: Automatic shim-based routing to correct Python versions

**Key Commands:**
- `pyenv install <version>` - Install Python version
- `pyenv local <version>` - Set project-specific version
- `pyenv global <version>` - Set system-wide version
- `pyenv versions` - List installed versions
- `poetry env use <version>` - Use version with Poetry

Mastering pyenv enables flexible Python development across multiple projects and versions.
