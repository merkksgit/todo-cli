# Todo CLI

[![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)](#)
[![Bash](https://img.shields.io/badge/Bash-4EAA25?logo=gnubash&logoColor=fff)](#)
[![Obsidian](https://img.shields.io/badge/Obsidian-%23483699.svg?&logo=obsidian&logoColor=white)](#)

A simple command-line tool for managing your todos in markdown format. Seamlessly integrates with Obsidian and other markdown editors.

<!-- vim-markdown-toc GFM -->

* [Features](#features)
* [Installation](#installation)
* [Usage](#usage)
* [Examples](#examples)
* [Aliases I use](#aliases-i-use)
* [Todo Format](#todo-format)
* [Use with Obsidian for Best Experience](#use-with-obsidian-for-best-experience)
* [Configuration](#configuration)
* [Server Sync Setup (Optional)](#server-sync-setup-optional)
    * [Server Setup](#server-setup)
    * [Adding Additional Computers](#adding-additional-computers)
    * [Manual Sync Control](#manual-sync-control)
    * [How Sync Works](#how-sync-works)
    * [Sync Commands](#sync-commands)
    * [Workflow Examples](#workflow-examples)
* [Requirements](#requirements)
* [License](#license)

<!-- vim-markdown-toc -->

## Features

- Manage your todos directly from the command line
- Store todos in a standard markdown file with checkbox syntax `- [ ]`
- Mark todos as done/undone with simple commands
- Edit, remove, and clear completed todos
- Set priority levels (high, medium, low) for tasks
- Add due dates to tasks
- Organize with categories/tags
- Search for specific todos
- View statistics about your tasks
- Configure where your todo list is stored
- **Multi-device sync** with smart conflict resolution via your own server
- **Obsidian integration** - edits sync seamlessly between CLI and GUI
- Color-coded terminal output for better readability
- Lightweight, fast, and works on any Unix-like system

## Installation

```bash
# Clone the repository
git clone https://github.com/merkksgit/todo-cli.git

# Move to a directory in your PATH
cp todo-cli/todo ~/.local/bin/

# Make it executable
chmod +x ~/.local/bin/todo
```

Make sure `~/.local/bin` is in your PATH.

## Usage

```
USAGE:
  todo "<task description>"       - Add a new todo item (use quotes for commands)
  todo <task without commands>    - Add a new todo item

TASK OPTIONS:
  -p, --priority <level>          - Set priority (high, medium, low)
  -d, --due <date>                - Set due date (DD-MM-YYYY, today, tomorrow)
  -c, --category <category>       - Add a category tag

COMMANDS:
  todo list                       - List all todo items
  todo done <number>              - Mark a todo as completed
  todo undo <number>              - Mark a completed todo as not done
  todo remove <number>            - Remove a todo item
  todo clear                      - Remove all completed tasks
  todo edit <number>              - Edit an existing todo item
  todo search <term>              - Search for todos containing term
  todo stats                      - Show todo statistics
  todo config <path>              - Set the location of the todo file
  todo sync-setup                 - Configure server sync settings
  todo sync                       - Sync with server (bi-directional)
  todo sync-up                    - Upload todos to server
  todo sync-down                  - Download todos from server
  todo offline                    - Switch to offline mode (disable sync)
  todo online                     - Switch to online mode (enable sync)
  todo status                     - Show current sync mode and configuration
  todo help                       - Show this help message
```

## Examples

```bash
# Add a new simple todo
todo Buy groceries

# Add todo with priority
todo "Complete report" -p high

# Add todo with due date
todo "Call doctor" -d tomorrow

# Add todo with category
todo "Update website" -c work

# Add todo with multiple attributes
todo "Team meeting" -p medium -d "05-05-2025" -c meetings

# List all todos
todo list

# Search todos
todo search groceries

# See todo statistics
todo stats

# Mark the first todo as done
todo done 1

# Edit a todo
todo edit 2

# Remove a todo
todo remove 3

# Clear all completed todos
todo clear
```

## Aliases I use

```bash
alias todol="todo list"
alias todod="todo done"
alias todou="todo undo"
alias todor="todo remove"
alias todos="todo search"
alias todoe="todo edit"
alias todoc="todo clear"
alias todoh="todo help"
```

## Todo Format

Todos are stored in markdown format:

```markdown
# Todo List

- [ ] Simple task
- [ ] Task with priority (high)
- [ ] Task with due date (due: 05-05-2025)
- [ ] Task with category #work
- [x] Completed task with priority (medium) #project
```

## Use with Obsidian for Best Experience

Todo CLI really shines when used in conjunction with markdown editors like Obsidian:

1. Configure your todo file to be stored within your Obsidian vault:

   ```bash
   todo config /path/to/your/Obsidian/vault/todos.md
   ```

2. Now you can:
   - Add todos from the command line while working
   - See and interact with them in Obsidian with proper markdown formatting
   - Use Obsidian's checkbox features to mark todos as complete visually
   - Benefit from Obsidian's search and linking capabilities
   - Organize tasks with categories that work as Obsidian tags

This creates a seamless workflow where you can quickly capture tasks from the terminal and organize/review them in your knowledge management system.

## Configuration

By default, todos are stored in `~/todos.md`. You can change this:

```bash
# Set a custom location
todo config ~/Documents/my-todos.md

# Point to your Obsidian vault
todo config ~/Obsidian/Work/todos.md
```

The configuration is stored in ~/.todo_config, which is a simple text file containing the full path to your todo file. This allows your todo file location to persist between sessions.

## Server Sync Setup (Optional)

Todo CLI supports automatic synchronization across multiple devices using your own server (e.g., Proxmox, VPS, Raspberry Pi).

### Server Setup

**1. Create a dedicated user on your server:**

```bash
# On your server
sudo useradd -m -s /bin/bash todouser
sudo passwd todouser

# Create sync directory
sudo mkdir -p /home/todouser/todos
sudo chown todouser:todouser /home/todouser/todos
```

**2. Set up SSH key authentication:**

```bash
# On your client machine(s)
ssh-keygen -t ed25519 -f ~/.ssh/todouser_key -C "todo-sync"
# Press Enter when asked for passphrase (no passphrase needed for automation)

# Copy public key to server
ssh-copy-id -i ~/.ssh/todouser_key todouser@your-server-ip

# Test connection (should work without password)
ssh -i ~/.ssh/todouser_key todouser@your-server-ip
```

**3. Configure sync on your client:**

```bash
./todo sync-setup
# Server hostname or IP: your-server-ip
# Username: todouser
# Remote path: /home/todouser/todos/todos.md
# SSH key path: ~/.ssh/todouser_key
```

### Adding Additional Computers

**To set up sync on your second/third/etc. computer:**

```bash
# 1. Install todo CLI on the new computer
git clone https://github.com/your-username/todo-cli.git
cp todo-cli/todo ~/.local/bin/ && chmod +x ~/.local/bin/todo

# 2. Generate a new SSH key (each computer gets its own key)
ssh-keygen -t ed25519 -f ~/.ssh/todouser_key -C "todo-sync-computer2"
# Press Enter for no passphrase

# 3. Add this new key to your server (adds to existing keys)
ssh-copy-id -i ~/.ssh/todouser_key todouser@your-server-ip

# 4. Test the connection
ssh -i ~/.ssh/todouser_key todouser@your-server-ip
# Should work without password

# 5. Configure sync (use same server details as first computer)
./todo sync-setup
# Server hostname: your-server-ip
# Username: todouser
# Remote path: /home/todouser/todos/todos.md
# SSH key path: ~/.ssh/todouser_key

# 6. Get your existing todos
./todo list
# Downloads all your existing todos from server
```

**Note:** Each computer uses its own SSH key for security. The server accepts multiple keys, so all your devices can sync to the same todo file.

### Manual Sync Control

Todo CLI gives you full control over when sync happens for optimal performance. When your server is offline or unreachable, SSH connection attempts cause significant delays (2-3 seconds per command). The offline mode prevents these SSH checks entirely, ensuring instant performance even when your server is down.

**Offline Mode (Default for new users):**

```bash
todo offline              # Disable sync for maximum speed
todo list                 # Instant response (~0.01s)
todo "new task"           # Add tasks instantly
```

**Online Mode (When you want to sync):**

```bash
todo online               # Enable sync
todo list                 # Syncs with server then lists todos
todo "sync this task"     # Adds task and syncs to server
```

**Mixed Workflow (Recommended):**

```bash
# Daily fast usage
todo offline
todo "task 1"
todo "task 2"
todo list                 # All instant

# When ready to sync
todo online
todo list                 # Syncs all changes with server
todo offline              # Back to fast mode
```

Your mode preference is saved and remembered between sessions.

### How Sync Works

- **Manual Control**: Choose when sync happens vs when you want speed
- **Smart Conflict Resolution**: Only syncs when needed based on file timestamps
- **Automatic Sync**: All todo operations automatically sync to server
- **Obsidian Integration**: Edit in Obsidian, changes sync seamlessly
- **Multi-Device**: All devices stay in sync automatically

### Sync Commands

```bash
todo sync-setup    # One-time setup
todo online        # Enable sync mode
todo sync          # Manual bi-directional sync
todo sync-up       # Upload to server only
todo sync-down     # Download from server only
todo offline       # Disable sync for fast local usage
```

### Workflow Examples

```bash
# Device A: Fast daily usage
./todo offline                    # Switch to offline mode
./todo "Buy groceries" -p high    # Add tasks instantly
./todo "Call dentist"             # No sync delays
./todo list                       # Instant listing

# When ready to sync changes
./todo online                     # Enable sync
./todo list                       # Syncs all changes to server

# Device B: Get updates
./todo online                     # Enable sync
./todo list                       # Downloads Device A's changes

# Edit in Obsidian on any device
# → Next './todo online && ./todo list' syncs changes

# Device C: Mixed workflow
./todo online                     # Enable sync for session
./todo done 1                     # Mark done, syncs to server
./todo offline                    # Back to fast mode
./todo "urgent task"              # Add without sync delay

# Periodic sync across all devices
./todo online && ./todo list      # Everyone gets latest changes
```

## Requirements

- Bash
- Core Unix utilities (grep, sed, etc.)
- **For sync**: rsync, ssh (usually pre-installed)

## License

MIT
