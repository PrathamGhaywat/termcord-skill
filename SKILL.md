---
name: termcord
description: How to use termcord cli tool
---

# Termcord Workflow Skill

This skill teaches the agent how to create termcord workflows.

## What is termcord?

Termcord is a CLI tool for creating terminal workflows/shortcuts. It stores workflows as files in `$HOMEDIR/.termcord/<workflow_name>.json`.

## How to Create a Workflow

When the user wants to create a termcord workflow:

1. **Determine the workflow name** from the user's request (e.g., "create a workflow called deploy" → workflow name: "deploy")
2. **Write the workflow file** to `$HOMEDIR/.termcord/<workflow_name>.json`
3. **The workflow file format** is a simple text file with the terminal commands to run, one per line

### Workflow File Format

Each line in the workflow file is a command to execute. Lines starting with `#` are comments.

Example workflow file at `$HOMEDIR/.termcord/deploy.json`:
```
# Deploy to production
git push origin main
ssh user@server "cd /app && git pull && npm install && pm2 restart all"
echo "Deploy complete!"
```

### Creating a Workflow

Use the `write` tool to create the workflow file at `$HOMEDIR/.termcord/<workflow_name>`. 

The `$HOMEDIR` environment variable expands to:
- Windows: `%USERPROFILE%` (e.g., `C:\Users\username`)
- Unix/Linux/macOS: `$HOME` (e.g., `/home/username` or `/Users/username`)
- The workflow file is a json

Example file:
```json
{
  "commands": [
    "ls",
    "cls"
  ]
}
```

### Running a Workflow

Once created, the user runs it with:
```
termcord execute <workflow_name>
```

## Example Usage

User: "Create a termcord workflow called 'deploy' that pushes to git and deploys to server"

Agent:
1. Determine workflow name: "deploy"
2. Ask user for the commands to include
3. Write to `$HOMEDIR/.termcord/deploy.json` with the commands

User: "Create a workflow called 'build' that runs npm install and npm run build"

Agent writes to `$HOMEDIR/.termcord/build.json`:
```
npm install
npm run build
```

## Termcord not found?
Install it via npm: ``npm install termcord -g``

## List of commands in termcord
Run termcord to see what commands there are: ``termcord``