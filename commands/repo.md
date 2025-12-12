---
description: Create a new repository, publish to GitHub, and optionally set up a virtual environment
argument-hint: <repo-name> [work]
allowed-tools: Bash(*), Read, Write, AskUserQuestion
---

# Create Repository Command

You are helping the user create a new repository. Follow these steps carefully:

## Arguments
- `$1` = Repository name (required)
- `$2` = "work" flag (optional) - if present, create in work directory

**Repo name provided:** $ARGUMENTS

## Step 1: Check Configuration

First, check if the configuration file exists:

```bash
cat ~/.claude/repo-creator-config.json 2>/dev/null
```

**If the config file does NOT exist or is empty:**

Use the AskUserQuestion tool to ask the user TWO questions:
1. "What is the absolute path to your PERSONAL projects directory?" (e.g., /Users/username/GIT/personal)
2. "What is the absolute path to your WORK projects directory?" (e.g., /Users/username/GIT/work)

Then create the config file:
```bash
mkdir -p ~/.claude
cat > ~/.claude/repo-creator-config.json << 'EOF'
{
  "personalRepoPath": "<USER_PROVIDED_PERSONAL_PATH>",
  "workRepoPath": "<USER_PROVIDED_WORK_PATH>"
}
EOF
```

## Step 2: Determine Repository Location

Read the config file and determine where to create the repo:
- If `$2` is "work" or "Work" or "WORK", use `workRepoPath`
- Otherwise (default), use `personalRepoPath`

## Step 3: Check GitHub CLI

Check if GitHub CLI is installed:
```bash
which gh
```

If `gh` is not found, install it:
```bash
brew install gh
```

If brew is not available, inform the user they need to install GitHub CLI manually from https://cli.github.com/

Verify gh is authenticated:
```bash
gh auth status
```

If not authenticated, run `gh auth login` and guide the user through authentication.

## Step 4: Create the Repository

Create the repository directory and initialize git:

```bash
# Create directory
mkdir -p <REPO_PATH>/<REPO_NAME>
cd <REPO_PATH>/<REPO_NAME>

# Initialize git
git init

# Create README
echo "# <REPO_NAME>" > README.md

# Initial commit
git add README.md
git commit -m "Initial commit"
```

## Step 5: Publish to GitHub

**For PERSONAL repos (default):**
Get the user's GitHub username and create the repo:
```bash
gh auth status 2>&1 | grep "Logged in to github.com account"
gh repo create <GITHUB_USERNAME>/<REPO_NAME> --public --source=. --remote=origin --push
```

**For WORK repos:**
Use AskUserQuestion to ask: "What GitHub organization should this work repo be published to?"
Then create with that org:
```bash
gh repo create <ORG_NAME>/<REPO_NAME> --public --source=. --remote=origin --push
```

## Step 6: Virtual Environment Setup

Use AskUserQuestion to ask: "Would you like to set up a virtual environment?"

Provide these options:
1. **venv** - Python virtual environment (python3 -m venv .venv)
2. **conda** - Conda environment
3. **npm** - Node.js project (npm init)
4. **Skip** - No virtual environment

Based on the user's choice:

**For venv:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```
Tell the user to activate with: `source .venv/bin/activate`

**For conda:**
Ask for Python version preference, then:
```bash
conda create -p ./conda-env python=<VERSION> -y
conda activate ./conda-env
```
Tell the user to activate with: `conda activate ./conda-env`

**For npm:**
```bash
npm init -y
```
This creates a package.json for Node.js projects.

**For Skip:**
No action needed.

## Step 7: Set Working Directory

After everything is complete, change to the new repository directory:
```bash
cd <REPO_PATH>/<REPO_NAME>
```

Confirm to the user:
- Repository created at: `<REPO_PATH>/<REPO_NAME>`
- GitHub URL: `https://github.com/<USERNAME_OR_ORG>/<REPO_NAME>`
- Virtual environment: <status>
- Current working directory is now the new repository

## Error Handling

- If repo name is not provided, ask the user for it
- If directory already exists, ask if they want to use a different name
- If GitHub publishing fails, show the error and offer to retry or skip
- If venv creation fails, show the error and continue without it
