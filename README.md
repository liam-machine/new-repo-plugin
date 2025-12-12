# Repo Creator Plugin for Claude Code

A Claude Code plugin that provides a `/repo` slash command to quickly create, initialize, and publish repositories to GitHub with optional virtual environment setup.

## Features

- Create personal or work repositories with a single command
- Automatic GitHub publishing
- Virtual environment setup (Python venv, Conda, or npm)
- Persistent configuration for your repo paths
- GitHub CLI installation check/prompt

## Installation

### From GitHub

```bash
# In Claude Code, add this repository as a marketplace
/plugin marketplace add liam-machine/new-repo-plugin

# Install the plugin
/plugin install repo-creator
```

### Manual Installation

Clone this repository and add it as a local marketplace:

```bash
git clone https://github.com/liam-machine/new-repo-plugin.git
# In Claude Code:
/plugin marketplace add /path/to/new-repo-plugin
/plugin install repo-creator
```

## Usage

### Basic Usage (Personal Repository)

```
/repo my-awesome-project
```

This will:
1. Create `my-awesome-project` in your personal repo directory
2. Initialize git and create initial commit
3. Publish to your GitHub account
4. Ask if you want a virtual environment

### Work Repository

```
/repo my-work-project work
```

This will:
1. Create `my-work-project` in your work repo directory
2. Initialize git and create initial commit
3. Ask which GitHub organization to publish to
4. Ask if you want a virtual environment

### First Run Configuration

The first time you use `/repo`, you'll be prompted to configure:

- **Personal repo path**: Where your personal projects are stored (e.g., `/Users/you/GIT/personal`)
- **Work repo path**: Where your work projects are stored (e.g., `/Users/you/GIT/work`)

This configuration is saved to `~/.claude/repo-creator-config.json` and persists across sessions.

## Virtual Environment Options

When prompted, you can choose:

| Option | Description |
|--------|-------------|
| **venv** | Python virtual environment (`python3 -m venv .venv`) |
| **conda** | Conda environment in the project directory |
| **npm** | Node.js project with `package.json` |
| **Skip** | No virtual environment |

## Requirements

- **GitHub CLI (`gh`)**: Required for publishing to GitHub. The plugin will attempt to install it via Homebrew on macOS if not found.
- **Git**: Must be installed and configured
- **Homebrew** (macOS): For automatic GitHub CLI installation

## Configuration

Configuration is stored at `~/.claude/repo-creator-config.json`:

```json
{
  "personalRepoPath": "/Users/username/GIT/personal",
  "workRepoPath": "/Users/username/GIT/work"
}
```

To reset configuration, delete this file and run `/repo` again.

## Plugin Structure

```
new-repo-plugin/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── commands/
│   └── repo.md              # /repo slash command
├── scripts/
│   └── ensure-gh-cli.sh     # GitHub CLI helper script
└── README.md
```

## License

MIT License - see LICENSE file for details.

## Contributing

Contributions are welcome! Please open an issue or pull request on GitHub.

## Author

[liam-machine](https://github.com/liam-machine)
