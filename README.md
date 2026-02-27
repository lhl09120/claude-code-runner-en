# Claude Code Runner

A wrapper for running Claude Code in non-interactive environments using PTY (pseudo-terminal).

## Features

- ✅ **PTY Execution**: Run in non-TTY environments (containers, CI/CD, background processes)
- ✅ **Auto-Response**: Automatically answer confirmation prompts ("Do you want to...")
- ✅ **User Switching**: Run as specified non-root user
- ✅ **File Synchronization**: Automatically sync changes back to original directory
- ✅ **Timeout Handling**: Configurable timeout with proper cleanup
- ✅ **Output Capture**: Capture and return full stdout/stderr

## Installation

```bash
git clone https://github.com/lhl09120/claude-code-runner-en.git
cd claude-code-runner-en
chmod +x scripts/run_claude.py
```

## Quick Start

### Python API

```python
from scripts.run_claude import run_claude_code

result = run_claude_code(
    workdir='/path/to/project',
    prompt='Refactor the authentication module',
    user='lighthouse',
    timeout=300
)
```

### Command Line

```bash
python3 scripts/run_claude.py /path/to/project "Your task description"
```

## Usage Examples

### Code Review

```python
run_claude_code(
    workdir='/root/repo/project',
    prompt='Review this codebase and identify potential bugs'
)
```

### Add Features

```python
run_claude_code(
    workdir='/root/repo/api',
    prompt='''
    Add a new REST endpoint:
    - GET /api/users/{id}/profile
    - Include validation and tests
    '''
)
```

### Bug Fix

```python
run_claude_code(
    workdir='/root/repo/app',
    prompt='Fix the memory leak in WebSocket handler'
)
```

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `workdir` | str | - | Project working directory |
| `prompt` | str | - | Natural language task description |
| `user` | str | 'lighthouse' | User to run as |
| `timeout` | int | 300 | Timeout in seconds |

## Dependencies

- Python 3.8+
- Claude Code (installed and in PATH)
- Unix-like environment (Linux/macOS)
- root or sudo privileges (for user switching)

## How It Works

1. **Copy Project** → to temporary directory
2. **Change Permissions** → switch to target user
3. **PTY Execute** → run Claude Code
4. **Auto-Response** → handle confirmation prompts
5. **Sync Changes** → write changes back to original directory
6. **Cleanup** → delete temporary files

## License

GNU Affero General Public License v3.0 (AGPL-3.0)

This program is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

See LICENSE file for details or visit https://www.gnu.org/licenses/agpl-3.0.html
