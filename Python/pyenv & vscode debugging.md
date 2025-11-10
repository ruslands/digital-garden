### 1. Steps to Create an Environment and Start the Debugger

#### Create and Configure the Virtual Environment
1. **Set Up `pyenv` Python**:
   - Ensure Python 3.11.5 is installed via `pyenv`:
     ```bash
     pyenv install 3.11.5
     pyenv shell 3.11.5
     ```
   - Verify: `python --version` should show `Python 3.11.5`.

2. **Initialize Poetry Virtualenv**:
   - In your project directory (`/Users/konovalov/Documents/code/seller/backend`):
     ```bash
     poetry init  # If not already set up
     poetry env use /Users/konovalov/.pyenv/versions/3.11.5/bin/python3.11
     ```
   - Ensure virtualenv is in the project folder (optional):
     ```bash
     poetry config virtualenvs.in-project true
     ```
   - Virtualenv path: `/Users/konovalov/Documents/code/seller/backend/.venv`.

3. **Install Dependencies**:
   - Run:
     ```bash
     poetry install --no-root
     ```
   - Add specific packages to `pyproject.toml`:
     ```bash
     poetry add <package-name>
     ```
   - **Case: Install a package without modifying `pyproject.toml`**:
     - Command:
       ```bash
       poetry run pip install <package-name>
       ```
     - Use: Temporarily install a package into the virtualenv without tracking it in Poetry’s dependency list (e.g., for testing).

4. **Verify Environment**:
   - Check: `poetry env info` should show:
     - `Executable`: `/Users/konovalov/Documents/code/seller/backend/.venv/bin/python`
     - `Python`: `3.11.5`
   - **Case: Inspect Poetry configuration**:
     - Command:
       ```bash
       poetry config --list
       ```
     - Use: Displays all Poetry settings (e.g., `virtualenvs.create`, `virtualenvs.path`) to confirm or debug environment behavior.

#### Configure and Start the Debugger in VS Code
1. **Update `launch.json`**:
   - Use this configuration in `.vscode/launch.json`:
     ```json
     {
         "name": "Seller Backend",
         "type": "python",
         "request": "launch",
         "module": "uvicorn",
         "args": [
             "seller.backend.app.main:app",
             "--port", "8000",
             "--host", "0.0.0.0",
             "--reload"
         ],
         "jinja": true,
         "justMyCode": false,
         "env": {
             "PYTHONPATH": "/Users/konovalov/Documents/code/seller/backend"
         },
         "envFile": "/Users/konovalov/Documents/code/seller/backend/.env",
         "python": "/Users/konovalov/Documents/code/seller/backend/.venv/bin/python"
     }
     ```

2. **Select Interpreter in VS Code**:
   - Open Command Palette (`Cmd + Shift + P`).
   - Run `Python: Select Interpreter`.
   - Choose `/Users/konovalov/Documents/code/seller/backend/.venv/bin/python`.

3. **Start Debugging**:
   - Set a breakpoint in your code (e.g., `main.py`).
   - Press `F5` or use the “Run and Debug” button.
   - Verify: Add `print(sys.executable)` in your code to confirm it’s using `/Users/konovalov/Documents/code/seller/backend/.venv/bin/python`.

---

### 2. Troubleshooting

#### Environment Creation Issues
- **Poetry Uses Wrong Python**:
  - Symptom: `poetry env info` shows `/Library/Frameworks/Python.framework/Versions/3.11` instead of `pyenv`.
  - Fix:
    ```bash
    poetry config virtualenvs.create true
    poetry env use /Users/konovalov/.pyenv/versions/3.11.5/bin/python3.11
    ```
  - **Case: Remove conflicting virtualenv**:
    - Command:
      ```bash
      poetry env remove /Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11
      ```
    - Use: Deletes an unwanted virtualenv (e.g., system Python) that Poetry keeps defaulting to, forcing it to use your specified `pyenv` Python.

- **Dependency Installation Fails**:
  - Symptom: `poetry install --no-root` errors out.
  - Fix:
    - Check `python = "~3.11"` in `pyproject.toml` matches 3.11.5.
    - Update lock file: `poetry lock --no-update`.
    - Retry: `poetry install --no-root`.
  - **Case: Check Poetry settings**:
    - Command: `poetry config --list`
    - Use: Identify if settings like `virtualenvs.create = false` or `virtualenvs.path` are causing Poetry to skip virtualenv creation or use the wrong path.

- **Permission Errors**:
  - Fix: `sudo chown -R konovalov:staff /Users/konovalov/Documents/code/seller/backend/.venv`.

#### Debugger Issues
- **Debugger Uses Wrong Python**:
  - Symptom: Debug output shows a different Python (e.g., `/Users/konovalov/.pyenv/versions/3.11.5/bin/python3.11`).
  - Fix:
    - Add `"python": "/Users/konovalov/Documents/code/seller/backend/.venv/bin/python"` to `launch.json`.
    - Reselect interpreter in VS Code.
    - Check `settings.json` for overrides like `"python.pythonPath"`.

- **Uvicorn Fails to Start**:
  - Symptom: Debugger starts but Uvicorn crashes.
  - Fix:
    - Ensure `uvicorn` is installed: `poetry add uvicorn`.
    - Or use: `poetry run pip install uvicorn` for a quick install without tracking.
    - Verify `PYTHONPATH` in `launch.json` only includes source dirs, not binaries.

- **Missing Packages**:
  - Symptom: “Module not found” errors in debugger.
  - Fix:
    - Install with Poetry: `poetry add <package-name>`.
    - Or manually: `poetry run pip install <package-name>` for a temporary install without modifying `pyproject.toml`.

#### General Checks
- **Verify Active Python**:
  - `which python` should match your intent.
  - Use `poetry shell` or `source .venv/bin/activate` to test manually.
- **Debug Output**:
  - Check VS Code terminal for Python path or errors.
  - Run `/Users/konovalov/Documents/code/seller/backend/.venv/bin/python --version` to confirm.
