# Poetry: Python Dependency Management Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Project Setup](#project-setup)
4. [Dependency Management](#dependency-management)
5. [Virtual Environment Management](#virtual-environment-management)
6. [Running Commands](#running-commands)
7. [Exporting Dependencies](#exporting-dependencies)
8. [VS Code Configuration](#vs-code-configuration)

---

## Introduction

Poetry is a modern dependency management and packaging tool for Python. It simplifies project setup, dependency resolution, and virtual environment management.

**Key Features:**
- Dependency resolution with conflict detection
- Virtual environment management
- Project scaffolding
- Lock file for reproducible builds
- Integration with PyPI for publishing

**Benefits:**
- Solves dependency conflicts automatically
- Creates reproducible environments
- Simplifies project setup
- Better than `pip` + `requirements.txt` workflow

---

## Installation

### Install Poetry

**Using the official installer:**

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

**Add to PATH:**

```bash
export PATH="/root/.local/bin:$PATH"
```

**Verify installation:**

```bash
poetry --version
```

### Uninstall Poetry

```bash
curl -sSL https://install.python-poetry.org | python3 - --uninstall
```

### Update Poetry

```bash
poetry self update
```

---

## Project Setup

### Create New Project

**Initialize a new Poetry project:**

```bash
poetry new poetry-demo
```

This creates a project structure:
```
poetry-demo/
├── pyproject.toml
├── README.md
├── poetry_demo/
│   └── __init__.py
└── tests/
    └── __init__.py
```

### Initialize Existing Project

**Add Poetry to an existing project:**

```bash
poetry init
```

This interactively creates a `pyproject.toml` file.

**Key File: pyproject.toml**

The `pyproject.toml` file is the most important file. It orchestrates your project and its dependencies, replacing `setup.py` and `requirements.txt`.

---

## Dependency Management

### Adding Dependencies

**Add a package:**

```bash
poetry add requests
```

**Add with version constraint:**

```bash
poetry add fastapi@latest
poetry add jinja2==3.0.0
```

**Add development dependencies:**

```bash
poetry add --dev pytest-order
```

**Add from requirements.txt:**

```bash
cat requirements.txt | xargs poetry add
```

### Removing Dependencies

**Remove a package:**

```bash
poetry remove --dev pytest-order
```

### Viewing Dependencies

**Show installed packages:**

```bash
poetry show
```

**Show dependency tree:**

```bash
poetry show tree
```

**Show latest available versions:**

```bash
poetry show --latest
```

### Lock File Management

**Update lock file without upgrading:**

```bash
poetry lock --no-update
```

**Dependency Constraints:**

Reference: [Poetry Dependency Specification](https://python-poetry.org/docs/dependency-specification/)

Poetry supports various version constraints:
- `^1.2.3` - Compatible with 1.2.3 (>=1.2.3, <2.0.0)
- `~1.2.3` - Compatible with 1.2.3 (>=1.2.3, <1.3.0)
- `==1.2.3` - Exact version
- `>=1.2.3` - Greater than or equal
- `*` - Any version

---

## Virtual Environment Management

### Environment Configuration

**Set Poetry to use active Python:**

```bash
poetry config virtualenvs.prefer-active-python=true
```

**Use specific Python version:**

```bash
# Using version number
poetry env use python3.9

# Using full path
poetry env use /full/path/to/python

# Using pyenv path
poetry env use /Users/konovalov/.pyenv/versions/3.11.5/bin/python
```

### Environment Information

**Display environment details:**

```bash
poetry env info
```

**List all environments:**

```bash
poetry env list
```

### Activating Environment

**Open shell in virtual environment:**

```bash
poetry shell
```

**Note:** This replaces the old approach of `source env/bin/activate`.

**Run commands without activating:**

```bash
poetry run uvicorn app.main:app
```

**Benefits:**
- No need to manually activate/deactivate
- Ensures correct environment is used
- Works consistently across platforms

### Workflow with Pyenv

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

# 5. Install dependencies
poetry install
```

---

## Running Commands

### Install Dependencies

**Install from poetry.lock:**

```bash
poetry install
```

This installs all dependencies specified in `pyproject.toml` and locked in `poetry.lock`.

### Run Scripts

**Run commands in Poetry environment:**

```bash
poetry run <command>
```

**Examples:**

```bash
poetry run uvicorn app.main:app --reload
poetry run pytest
poetry run python script.py
```

**Custom Scripts:**

You can add custom scripts to `pyproject.toml`:

```toml
[tool.poetry.scripts]
my-script = "my_package.module:function"
```

Then run:

```bash
poetry run my-script
```

---

## Exporting Dependencies

### Export to requirements.txt

**Export all dependencies:**

```bash
poetry export --without-hashes --format=requirements.txt > requirements.txt
```

**Export only production dependencies:**

```bash
poetry export --without-hashes --format=requirements.txt --only main > requirements.txt
```

**Export only development dependencies:**

```bash
poetry export --without-hashes --format=requirements.txt --only dev > requirements-dev.txt
```

### Install from Poetry Export

**Install via pip from Poetry export:**

```bash
# Install all dependencies
pip3 install --requirement <(poetry export --format requirements.txt)

# Install only lint dependencies
pip3 install --user --requirement <(poetry export --only lint --format requirements.txt)

# Install only development dependencies
pip3 install --user --requirement <(poetry export --only development --format requirements.txt)
```

**Use Case:** When working in environments that don't have Poetry installed, or for Docker builds.

---

## VS Code Configuration

### Launch Configuration

**Python debugging configuration:**

```json
{
    "name": "Python: Seller",
    "type": "python",
    "request": "launch",
    "program": "/Users/konovalov/1.code/seller/backend/app/main.py",
    "console": "integratedTerminal",
    "justMyCode": true,
    "envFile": "/Users/konovalov/1.code/seller/backend/.env.development",
    "env": {
        "PYTHONPATH": "/Users/konovalov/1.code/seller/backend/venv/lib/python3.11/site-packages:/Users/konovalov/1.code/seller/backend"
    }
}
```

**Configuration Options:**

- `justMyCode`: Only debug your code, skip library code
- `envFile`: Load environment variables from a file
- `env`: Set specific environment variables
- `PYTHONPATH`: Add paths for module resolution

### Poetry Environment Detection

VS Code should automatically detect Poetry environments. If not:

1. Open Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`)
2. Select "Python: Select Interpreter"
3. Choose the Poetry virtual environment

---

## Best Practices

### Project Structure

1. **Commit `pyproject.toml`**: Always commit this file
2. **Commit `poetry.lock`**: Ensures reproducible builds
3. **Use version constraints**: Specify compatible versions, not exact versions
4. **Separate dev dependencies**: Use `--dev` flag for development tools

### Dependency Management

1. **Regular updates**: Run `poetry update` periodically
2. **Check outdated packages**: Use `poetry show --latest`
3. **Review lock file changes**: Check `poetry.lock` in pull requests
4. **Use groups**: Organize dependencies by purpose (dev, test, docs)

### Virtual Environments

1. **One environment per project**: Let Poetry manage environments
2. **Use `poetry run`**: Prefer this over manual activation
3. **Check environment info**: Use `poetry env info` to verify setup
4. **Clean up old environments**: Remove unused environments periodically

### Workflow Integration

1. **CI/CD**: Use `poetry install` in build pipelines
2. **Docker**: Export requirements.txt if Poetry isn't available
3. **Documentation**: Include Poetry setup in README
4. **Team alignment**: Ensure all team members use Poetry

---

## Troubleshooting

### Environment Issues

**Problem: Poetry can't find Python**

```bash
# Check Poetry environment info
poetry env info

# Manually set Python path
poetry env use /full/path/to/python
```

**Problem: Wrong Python version**

```bash
# List available environments
poetry env list

# Remove and recreate
poetry env remove python3.9
poetry env use python3.11
poetry install
```

### Dependency Conflicts

**Problem: Dependency resolution fails**

```bash
# Update lock file
poetry lock --no-update

# Or update all dependencies
poetry update
```

**Problem: Package not found**

- Check PyPI for correct package name
- Verify internet connection
- Check if package requires additional repositories

### Lock File Issues

**Problem: Lock file out of sync**

```bash
# Regenerate lock file
poetry lock

# Install with updated lock file
poetry install
```

---

## Common Workflows

### Starting a New Project

```bash
# 1. Create project
poetry new my-project
cd my-project

# 2. Add dependencies
poetry add fastapi uvicorn

# 3. Add dev dependencies
poetry add --dev pytest black

# 4. Install everything
poetry install

# 5. Start development
poetry shell
# or
poetry run python main.py
```

### Adding to Existing Project

```bash
# 1. Initialize Poetry
poetry init

# 2. Add existing dependencies
poetry add package1 package2

# 3. Install
poetry install

# 4. Export for compatibility
poetry export --format=requirements.txt > requirements.txt
```

### Updating Dependencies

```bash
# Check for updates
poetry show --latest

# Update specific package
poetry update package-name

# Update all packages
poetry update

# Update lock file only
poetry lock
```

---

## Summary

Poetry provides a modern, efficient workflow for Python dependency management:

- **Installation**: Simple curl-based installer
- **Project Setup**: `poetry new` or `poetry init` for existing projects
- **Dependencies**: Easy add/remove with automatic conflict resolution
- **Environments**: Automatic virtual environment management
- **Lock Files**: Reproducible builds with `poetry.lock`
- **Export**: Generate `requirements.txt` for compatibility

**Key Commands:**
- `poetry new <name>` - Create new project
- `poetry add <package>` - Add dependency
- `poetry install` - Install all dependencies
- `poetry run <command>` - Run command in environment
- `poetry shell` - Activate virtual environment
- `poetry export` - Export to requirements.txt

Poetry simplifies Python project management and makes dependency handling more reliable and reproducible.
