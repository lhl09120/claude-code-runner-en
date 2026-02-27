---
name: claude-code-runner
description: "Execute programming tasks via Claude Code using PTY-based invocation. Handles non-TTY environments, auto-responds to prompts, and manages file synchronization."
---

# Claude Code Runner

## Overview

A wrapper skill for running Claude Code programmatically in non-interactive environments. Uses PTY (pseudo-terminal) to handle TTY-required operations and automatically responds to confirmation prompts.

## Features

- **PTY-based execution**: Works in non-TTY environments (containers, CI/CD, background processes)
- **Auto-respond to prompts**: Automatically answers "Do you want to..." confirmations
- **User switching**: Runs as specified non-root user
- **File synchronization**: Copies project to temp directory, executes, syncs changes back
- **Timeout handling**: Configurable timeout with proper cleanup
- **Output capture**: Captures and returns full stdout/stderr

## Installation

```bash
# Clone the skill
git clone https://github.com/lhl09120/claude-code-runner-skill.git

# Make script executable
chmod +x claude-code-runner/scripts/run_claude.py
```

## Usage

### Basic Usage

```python
from claude_code_runner import run_claude_code

result = run_claude_code(
    workdir='/path/to/project',
    prompt='Refactor the authentication module to use JWT tokens',
    user='lighthouse',
    timeout=300
)

print(result)
```

### Via Command Line

```bash
python3 scripts/run_claude.py /path/to/project "Your task description here"
```

### Advanced Options

```python
result = run_claude_code(
    workdir='/root/repo/my-project',
    prompt='''
    1. Review the codebase
    2. Identify security vulnerabilities
    3. Fix any issues found
    4. Add appropriate tests
    ''',
    user='developer',
    timeout=600  # 10 minutes
)
```

## API Reference

### `run_claude_code(workdir, prompt, user='lighthouse', timeout=300)`

Execute a Claude Code task in a PTY environment.

**Parameters:**
- `workdir` (str): Working directory containing the project
- `prompt` (str): Natural language task description
- `user` (str): User to run as (default: 'lighthouse')
- `timeout` (int): Timeout in seconds (default: 300)

**Returns:**
- `str`: Combined stdout and stderr output

**Behavior:**
1. Copies project to temporary directory
2. Changes ownership to specified user
3. Executes Claude Code via PTY
4. Auto-responds to confirmation prompts
5. Syncs changes back to original directory
6. Cleans up temporary files

## Use Cases

### 1. Automated Code Review

```python
result = run_claude_code(
    workdir='/root/repo/project',
    prompt='Review this codebase and identify potential bugs or improvements'
)
```

### 2. Refactoring Tasks

```python
result = run_claude_code(
    workdir='/root/repo/legacy-app',
    prompt='Refactor the database layer to use SQLAlchemy ORM instead of raw SQL'
)
```

### 3. Adding Features

```python
result = run_claude_code(
    workdir='/root/repo/api-service',
    prompt='''
    Add a new REST endpoint for user profile management:
    - GET /api/users/{id}/profile
    - PUT /api/users/{id}/profile
    - Include validation and error handling
    - Add unit tests
    '''
)
```

### 4. Bug Fixes

```python
result = run_claude_code(
    workdir='/root/repo/web-app',
    prompt='Fix the memory leak in the WebSocket connection handler'
)
```

## Requirements

- Python 3.8+
- Claude Code installed and in PATH
- Unix-like environment (Linux/macOS)
- Root or sudo access (for user switching)

## Configuration

### Environment Variables

- `CLAUDE_CODE_USER`: Default user to run as (default: 'lighthouse')
- `CLAUDE_CODE_TIMEOUT`: Default timeout in seconds (default: 300)

### Customization

Edit `scripts/run_claude.py` to customize:
- Auto-response keywords
- Temp directory location
- Sync behavior
- Output formatting

## Troubleshooting

### "Permission denied" errors

Ensure the script is run with sufficient privileges to:
- Create temporary directories
- Change file ownership
- Switch to target user

### Claude Code not found

Make sure Claude Code is installed and in the system PATH:
```bash
which claude
```

### Task timeout

Increase the timeout for long-running tasks:
```python
run_claude_code(workdir, prompt, timeout=600)  # 10 minutes
```

### Interactive prompts not auto-responded

Add new prompt patterns to the auto-respond logic:
```python
if b'new prompt text' in output:
    os.write(master_fd, b'y\n')
```

## Limitations

- Requires Unix-like environment (uses PTY)
- Requires root/sudo for user switching
- Claude Code must be installed separately
- May not handle all edge cases of interactive prompts

## License

GNU Affero General Public License v3.0 (AGPL-3.0)

This program is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

See LICENSE file for full details or visit https://www.gnu.org/licenses/agpl-3.0.html

## Changelog

### v1.0.0 (2026-02-27)
- Initial release
- PTY-based Claude Code execution
- Auto-response to confirmation prompts
- File synchronization
- User switching support
