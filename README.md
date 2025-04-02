# Todo CLI

[![Bash](https://img.shields.io/badge/Bash-4EAA25?logo=gnubash&logoColor=fff)](#)
[![Obsidian](https://img.shields.io/badge/Obsidian-%23483699.svg?&logo=obsidian&logoColor=white)](#)

A simple, powerful command-line tool for managing your todos in markdown format. Seamlessly integrates with Obsidian and other markdown editors.

## Features

- Manage your todos directly from the command line
- Store todos in a standard markdown file with checkbox syntax (`- [ ]`)
- Mark todos as done/undone with simple commands
- Edit, remove, and clear completed todos
- Configure where your todo list is stored
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
todo "<task description>"    - Add a new todo item (use quotes for commands)
todo <task without commands> - Add a new todo item
todo list                    - List all todo items
todo done <number>           - Mark a todo as completed
todo undo <number>           - Mark a completed todo as not done
todo remove <number>         - Remove a todo item
todo clear                   - Remove all completed tasks
todo edit <number>           - Edit an existing todo item
todo config <path>           - Set the location of the todo file
todo help                    - Show this help message
```

## Examples

```bash
# Add a new todo
todo "Buy groceries"

# List all todos
todo list

# Mark the first todo as done
todo done 1

# Edit a todo
todo edit 2

# Remove a todo
todo remove 3

# Clear all completed todos
todo clear
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

## Requirements

- Bash
- Core Unix utilities (grep, sed, etc.)

## License

MIT
