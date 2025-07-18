# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a bash-based CLI todo manager that stores todos in markdown format with checkbox syntax. It's designed to integrate seamlessly with Obsidian and other markdown editors.

## Development Commands

**No build system required** - this is a single bash script:
- Run: `./todo` or `todo` (if in PATH)
- Test manually: `./todo list`, `./todo "test task"`, etc.
- Install: `cp todo ~/.local/bin/ && chmod +x ~/.local/bin/todo`

## Architecture

**Single File Design:**
- Main executable: `todo` (866 lines of bash)
- Configuration: `~/.todo_config` (stores todo file path)
- Data storage: Markdown file with GitHub-style checkboxes (default: `~/todos.md`)

**Key Functions:**
- `parse_date()` / `format_date_display()` - Handle due dates with natural language
- `format_todo_display()` - Color-coded terminal output
- `show_stats()` - Calculate task statistics
- Command parsing for: list, done, undo, remove, clear, edit, search, stats, config

**Data Format:**
```markdown
- [ ] Task description (priority) #category (due: YYYY-MM-DD)
- [x] Completed task
```

**Features:**
- Priorities: `(high)`, `(medium)`, `(low)`
- Due dates: Natural language ("today", "tomorrow") or DD-MM-YYYY format
- Categories: `#tagname` format
- Cross-platform date handling (GNU/BSD date compatibility)

## Key Technical Details

**Date Handling:** Uses both `date` command variants for cross-platform compatibility (GNU/BSD)
**Special Characters:** Recent fixes for handling special characters in todo text using awk instead of sed
**Color Output:** ANSI escape codes for terminal formatting
**File Integration:** Native markdown format works with Obsidian, VSCode, and other editors

## Common Workflows

- Adding tasks: `todo "Task description" -p high -d tomorrow -c work`
- Batch operations: Use numbered references from `todo list`
- Obsidian integration: Configure with `todo config /path/to/vault/todos.md`