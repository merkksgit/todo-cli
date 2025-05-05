# Todo CLI

[![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)](#)
[![Bash](https://img.shields.io/badge/Bash-4EAA25?logo=gnubash&logoColor=fff)](#)
[![Obsidian](https://img.shields.io/badge/Obsidian-%23483699.svg?&logo=obsidian&logoColor=white)](#)

A simple command-line tool for managing your todos in markdown format. Seamlessly integrates with Obsidian and other markdown editors.

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

## These are aliases I use

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

## Requirements

- Bash
- Core Unix utilities (grep, sed, etc.)

## License

MIT
