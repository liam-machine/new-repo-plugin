# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Claude Code plugin that provides the `/repo` slash command for quickly creating, initializing, and publishing repositories to GitHub with optional virtual environment setup.

## Plugin Architecture

This is a Claude Code plugin following the standard plugin structure:

- **`.claude-plugin/plugin.json`** - Plugin manifest with name, version, description, and metadata
- **`.claude-plugin/marketplace.json`** - Marketplace configuration for plugin distribution
- **`commands/repo.md`** - The `/repo` slash command implementation (markdown-based prompt)
- **`scripts/ensure-gh-cli.sh`** - Helper script for GitHub CLI installation/authentication checks

## How the Plugin Works

The `/repo` command is defined in `commands/repo.md` as a markdown file with YAML frontmatter specifying:
- `description` - Command description
- `argument-hint` - Shows `<repo-name> [work]` in help
- `allowed-tools` - Restricts command to `Bash(*)`, `Read`, `Write`, `AskUserQuestion`

The command flow:
1. Reads config from `~/.claude/repo-creator-config.json`
2. If no config, prompts user for personal/work repo paths
3. Ensures GitHub CLI is installed and authenticated
4. Creates directory, initializes git, makes initial commit
5. Publishes to GitHub (personal account or work org)
6. Optionally sets up venv, conda, or npm environment

## Configuration

User config stored at `~/.claude/repo-creator-config.json`:
```json
{
  "personalRepoPath": "/path/to/personal/repos",
  "workRepoPath": "/path/to/work/repos"
}
```

## Dependencies

- GitHub CLI (`gh`) - required for publishing
- Git - must be installed and configured
- Homebrew (macOS) - for automatic gh installation
