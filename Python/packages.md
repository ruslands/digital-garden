# Python Packages and Development Tools

## Table of Contents
1. [Introduction](#introduction)
2. [FastAPI Developer Tools](#fastapi-developer-tools)
3. [Essential Python Packages](#essential-python-packages)
4. [Development Tools](#development-tools)
5. [Environment Configuration](#environment-configuration)
6. [VS Code Configuration](#vs-code-configuration)

---

## Introduction

This guide covers essential Python packages, FastAPI development tools, and development environment configuration. These tools help improve productivity, code quality, and development workflow.

---

## FastAPI Developer Tools

FastAPI has a rich ecosystem of developer tools that enhance productivity and code quality. Here are some of the best tools available:

### FastAPI Code Generator

**Stars:** 612 ⭐

**Description:**

Create a FastAPI app from an OpenAPI file, enabling schema-driven development.

**Created by:** [Koudai Aono](https://www.linkedin.com/in/ACoAAAzV_hgBRcsUjAyed_Yj9RfvE0iag1wn1wc)

**Contributors:** 
- [Roberto Polli](https://www.linkedin.com/in/ACoAAACZ_i4Bx8eKdrHIFrnV8TKQrs2XAFkEcNU)
- [Roman Inflianskas](https://www.linkedin.com/in/ACoAABe_htMBhBhA2rsgskGBQ2OA_ldxslzyeD8)

**Link:** [FastAPI Code Generator](https://lnkd.in/eNCQehuz)

**Use Case:** Generate FastAPI application code from OpenAPI specifications, ensuring consistency between API documentation and implementation.

---

### FastAPI Client Generator

**Stars:** 270 ⭐

**Description:**

Generate a mypy- and IDE-friendly API client from an OpenAPI spec.

**Created by:** [David M.](https://www.linkedin.com/in/ACoAABjy0PQBcv2B3STeDjylIpfB-PDx3DmxDOU)

**Link:** [FastAPI Client Generator](https://lnkd.in/eAS3ynm9)

**Use Case:** Automatically generate type-safe Python clients for your FastAPI applications, improving developer experience and reducing errors.

---

### FastAPI MVC

**Stars:** 250 ⭐

**Description:**

Developer productivity tool for making high-quality FastAPI production-ready APIs.

**Created by:** [Radosław Szamszur](https://www.linkedin.com/in/ACoAAB20lrYBBFFG8nsYchH2Q7FuQSoXTI7_A1Q)

**Link:** [FastAPI MVC](https://lnkd.in/eMXUPCv5)

**Use Case:** Scaffold FastAPI projects with best practices, following MVC (Model-View-Controller) patterns.

---

### FastAPI Profiler

**Stars:** 94 ⭐

**Description:**

A FastAPI Middleware of joerick/pyinstrument to check your service performance.

**Link:** [FastAPI Profiler](https://lnkd.in/eq299fKY)

**Use Case:** Profile FastAPI applications to identify performance bottlenecks and optimize slow endpoints.

---

### FastAPI Versioning

**Stars:** 459 ⭐

**Description:**

API versioning support for FastAPI applications.

**Created by:** [Dean Way](https://www.linkedin.com/in/ACoAABjNbLMBuHnoau4WQFQ8s0088t3ANAneoMU)

**Contributors:** 
- [Simon C.](https://www.linkedin.com/in/ACoAAARDXk0B727uBK0Jejj33jkw9MLiOjGl6S4)
- Rafael Araujo Lehmkuhl

**Link:** [FastAPI Versioning](https://lnkd.in/eaBadeHs)

**Use Case:** Implement API versioning strategies to maintain backward compatibility while evolving your API.

---

### Jupyter Notebook REST API

**Description:**

Run your Jupyter notebooks as RESTful API endpoints.

**Created by:** [Justin Mitchel](https://www.linkedin.com/in/ACoAAAC3efIBKP1EwXh4DtLky2-WBW3wn1xtoD8)

**Link:** [Jupyter Notebook REST API](https://lnkd.in/e5FsYEe9)

**Use Case:** Expose Jupyter notebooks as API endpoints, enabling programmatic access to notebook functionality.

---

### Manage FastAPI

**Stars:** 1k ⭐

**Description:**

CLI tool for generating and managing FastAPI projects.

**Created by:** [Yagiz Degirmenci](https://www.linkedin.com/in/ACoAADA7qosBS89O-8ZToouo2gfottG3WMWIl4E)

**Contributors:** [Marcelo Trylesinski](https://www.linkedin.com/in/ACoAAAxCSfUBa-94DHQQBCRmT8JN8neKmYv9sec)

**Link:** [Manage FastAPI](https://lnkd.in/ebn8nKs2)

**Use Case:** Command-line tool for scaffolding, managing, and maintaining FastAPI projects with best practices.

---

### msgpack-asgi

**Stars:** 127 ⭐

**Description:**

Automatic MessagePack content negotiation for ASGI applications.

**Created by:** [Florimond Manca](https://www.linkedin.com/in/ACoAABsdm2oBdfbPbiapYPA9--DwUPSDKXLTKY)

**Link:** Available on GitHub/PyPI

**Use Case:** Enable MessagePack serialization for FastAPI applications, providing more efficient data transfer than JSON.

---

## Essential Python Packages

### ODMantic

**Description:**

ODM (Object Document Mapper) for MongoDB with Pydantic integration.

**Use Case:** Type-safe MongoDB interactions with Pydantic models, similar to SQLAlchemy for MongoDB.

---

### OAuthLib

**Description:**

Comprehensive, tested, and abstract implementation of the OAuth request-signing logic.

**Documentation:** [OAuthLib Installation](https://oauthlib.readthedocs.io/en/latest/installation.html)

**Use Case:** Implement OAuth 1.0 and OAuth 2.0 authentication in Python applications.

---

### watchgod

**Description:**

File watching and code reload in Python.

**Use Case:** 
- Auto-reload development servers when code changes
- Watch file systems for changes
- Implement hot-reloading functionality

---

### Other Useful Packages

- **wave**: Read and write WAV audio files
- **Jose**: JavaScript Object Signing and Encryption (JWT) library
- **arq.jobs**: Job queue library for asyncio
- **Aiogram**: Asynchronous framework for Telegram Bot API
- **pyms (tracing)**: Python microservices tracing
  - [Tutorial](https://python-microservices.github.io/tutorials/tutorial_propagate_traces/)
- **Anyio**: High-level asynchronous compatibility layer

---

## Development Tools

### Alembic

**Description:**

Database migration tool for SQLAlchemy.

**Documentation:** [Alembic Tutorial](https://alembic.sqlalchemy.org/en/latest/tutorial.html#)

**Common Commands:**

```bash
# Initialize Alembic in your project
alembic init alembic

# Check current database revision
alembic current

# Create a new migration
alembic revision -m "new"

# Apply migrations to head
alembic upgrade head

# View migration history
alembic history

# Rollback to a specific revision
alembic downgrade <revision_id>
```

**Important Note:**

Alembic won't generate complex changes automatically like table renaming or column type changes. You need to create manual revisions for such cases.

**Use Case:** Version control for database schema changes, enabling safe and reversible database migrations.

---

### Copier

**Description:**

Project templating tool for creating projects from templates.

**Documentation:** [Copier Creating Templates](https://copier.readthedocs.io/en/stable/creating/)

**Key Concepts:**

1. **Template Files:**
   - Files ending with `.jinja` are rendered as templates
   - Other files are copied as-is

2. **Configuration File:**
   - `copier.yml` or `copier.yaml` in the root prompts users for values
   - Has two kinds of configurations:
     - **Settings**: For Copier itself (start with underscore, e.g., `_min_copier_version`)
     - **Answers**: User-provided values stored as variables for template rendering

**Use Case:** Create project templates for consistent project structure, reducing boilerplate and ensuring best practices.

---

### pytest

**Description:**

Python testing framework with powerful features.

**Key Feature: conftest.py**

The `conftest.py` file provides fixtures for an entire directory. Fixtures defined in a `conftest.py` can be used by any test in that package without needing to import them.

**Example:**

```python
# conftest.py
import pytest

@pytest.fixture
def sample_data():
    return {"key": "value"}

# test_example.py (in same directory)
def test_something(sample_data):  # Fixture automatically available
    assert sample_data["key"] == "value"
```

**Use Case:** Write and run tests with fixtures, parametrization, and plugins for comprehensive test coverage.

---

## Environment Configuration

### PYTHONPATH

The `PYTHONPATH` environment variable tells Python where to look for modules and packages.

**Setting PYTHONPATH:**

```bash
# Add to PYTHONPATH
export PYTHONPATH="${PYTHONPATH}:/path/to/your/project/"

# Set PYTHONPATH (replaces existing)
export PYTHONPATH="/app/application:$PYTHONPATH"

# Add current directory
export PYTHONPATH="${PYTHONPATH}:$PWD"
```

**Use Cases:**
- Running scripts from different directories
- Testing modules during development
- Deploying applications with custom module paths

---

## VS Code Configuration

### Launch Configuration

VS Code launch configuration for Python debugging:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "env": {
                "PYTHONPATH": "${workspaceFolder}${pathSeparator}${env:PYTHONPATH}"
            }
        }
    ]
}
```

**What This Does:**
- Launches the currently open Python file
- Uses integrated terminal for output
- Sets PYTHONPATH to include workspace folder
- Preserves existing PYTHONPATH environment variable

### Advanced Launch Configuration

Example with custom environment file and PYTHONPATH:

```json
{
    "name": "Python: Custom Project",
    "type": "python",
    "request": "launch",
    "program": "/path/to/project/app/main.py",
    "console": "integratedTerminal",
    "justMyCode": true,
    "envFile": "/path/to/project/.env.development",
    "env": {
        "PYTHONPATH": "/path/to/venv/lib/python3.11/site-packages:/path/to/project"
    }
}
```

**Configuration Options:**
- `justMyCode`: Only debug your code, skip library code
- `envFile`: Load environment variables from a file
- `env`: Set specific environment variables

---

## Additional Resources

### Curses Module

```python
from curses import intrflush
```

**Use Case:** Terminal UI development, creating interactive command-line applications.

---

## Summary

This guide covered:

1. **FastAPI Tools**: Code generators, clients, profilers, and management tools
2. **Essential Packages**: ODMantic, OAuthLib, watchgod, and others
3. **Development Tools**: Alembic, Copier, pytest
4. **Environment Setup**: PYTHONPATH configuration
5. **VS Code Configuration**: Debugging and launch configurations

**Key Takeaways:**
- FastAPI has a rich ecosystem of developer tools
- Proper environment configuration improves development workflow
- Testing and migration tools are essential for production code
- VS Code configurations can streamline debugging

These tools and configurations help create a productive Python development environment.
