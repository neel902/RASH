# RASH

**Rash Sandboxing Interface (RSI)** is a small, terminal-based sandbox environment written in Python. It provides a custom command-line interface, a JSON-backed virtual filesystem, an integrated text editor, a Python console, simple RAM simulation, and a handful of Unix-like command aliases.

> **Status:** Experimental / hobby project  
> **Note:** This project is vibe coded and may contain bugs or unfinished features.

## Features

- Custom terminal interface with RASH branding
- JSON-based virtual filesystem that may be compatible with PyBit's VFS
- Integrated terminal text editor using `prompt_toolkit`
- Interactive Python console
- Simulated 8192-cell RAM (does nothing but simulated anyway)
- Session permission elevation with `sudo`
- `rashfetch` system-information display
- Mathematical expression evaluation
- Command aliases such as `cat`, `pwd`, `dir`, `calc`, and `python3`
- Drive formatting and drive information commands
- File creation, reading, editing, and deletion

## Project Structure

The project is split into components similar to:

```text
RASH/
├── RASHFileAPI.py       # Virtual filesystem API
├── RASHFileEditor.py    # Terminal text editor
├── <main RASH file>     # Terminal / command implementation
└── filesystem           # JSON-based VFS
```

The exact filenames may differ depending on how the project is organized.

## Requirements

RASH uses Python and the following third-party packages:

- `prompt_toolkit`
- `pygments`

Install them with:

```bash
pip install prompt_toolkit pygments
```
(if on debian, replace pip with the location of pip in a virtual environment. for more information, look up debian pip usage)

## Running RASH

Run the main Python program:

```bash
python <main_file>.py
```

You should be greeted by the RASH terminal interface.

The terminal starts with a simulated **8192-cell RAM (disclaimer: usefulness to be added)** and a virtual current directory of `/`.

## Commands

### File and directory commands

| Command | Description |
|---|---|
| `ls [directory]` | Lists files in a directory |
| `cd <directory>` | Changes the current directory |
| `pwd` | Displays the current directory |
| `create <path>` | Creates a new file |
| `read <path>` | Reads a file |
| `cat <path>` | Alias for `read` |
| `ed <path>` | Opens the integrated file editor |
| `edit <path>` | Alias for `ed` |
| `rm <path>` | Deletes a file |
| `del <path>` | Alias for `rm` |
| `drive_details` | Displays virtual-drive information |
| `drive` | Alias for `drive_details` |
| `format drive` | Resets the virtual drive |

### Console commands

| Command | Description |
|---|---|
| `help [command]` | Displays command help |
| `echo <message>` | Prints a message |
| `type <command>` | Shows whether a command is recognized |
| `aliases [command]` | Displays command aliases |
| `clearconsole` | Clears the terminal |
| `cls` | Alias for `clearconsole` |
| `testconsole` | Tests console output |
| `math <expression>` | Evaluates a mathematical expression |
| `rashfetch` | Displays RASH system information |
| `exit` | Exits RASH |

### Python

Run:

```text
python
```

to enter the interactive Python console.

You can also execute Python code stored in a virtual file:

```text
python <path>
```

Inside the RASH Python console, the project exposes helpers through `rash` and `rashFS`.

#### `rash`

- `rash.merge(address, value)` — writes a value to simulated RAM
- `rash.get(address)` — reads simulated RAM
- `rash.rollback()` — restores RAM to its state when the Python console was opened

#### `rashFS`

- `rashFS.read(path)` — reads a virtual file
- `rashFS.write(path, value)` — writes a virtual file
- `rashFS.delete(path)` — deletes a virtual file

The normal Python `open()` function is intentionally overridden to discourage direct filesystem access and redirect users toward `rashFS`.

## Permissions

Some commands require elevated permissions:

```text
python
format
rm
ed
```

Use:

```text
sudo <password>
```

to elevate the current session.

Permissions can be revoked with:

```text
sudo off
```

The implementation also supports password-management operations after permissions have already been elevated.

> **Security note:** The password is rash2026 by default. Idk how to do encryption so you get a plaintext password congrats. Also password changes by sudo are currently per session, and not saved.

## File System

RASH does not use a conventional directory tree for its virtual drive. Instead, `FileAPI` stores paths and their contents in a JSON file named `filesystem`.

The API provides operations for:

- Formatting the drive
- Reading files
- Creating/updating files
- Deleting files
- Listing files
- Listing files within a directory
- Inspecting drive details

A new drive contains a root entry with a welcome message.

## Integrated Editor

The `ed` / `edit` command launches a full-screen terminal editor built with `prompt_toolkit`.

The editor supports normal text editing with keyboard navigation and displays:

- Current filename
- Cursor line
- Cursor column

Press:

```text
Ctrl+Q
```

to save the editor contents and return to RASH.

Syntax highlighting is selected using the filename extension through Pygments where supported.

## Command Aliases

RASH provides several aliases for convenience. Do `alias <command>` to see them

## Disclaimer

RASH is an experimental project and is a learning/hobby implementation rather than a production operating system or secure sandbox. I don't imagine anyone would actually use this for anything important, but I still have to have this (I think).

In particular:

- The virtual filesystem is JSON-based.
- Permission handling is implemented in application code.
- The Python console executes Python code through `exec`.
- The password is embedded in the source.
- Some functionality is intentionally incomplete or deprecated.
- The project has not been presented as a security boundary.

**Do not use RASH as a real security sandbox for untrusted code.**

<details>
<summary>License</summary>

## License

MIT License

Copyright (c) 2026 @neel902

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF THE CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

</details>

## Credits

Built as a Python terminal experiment using:

- `prompt_toolkit`
- `Pygments`
