---
name: termcord-workflow
description: Create termcord workflows by writing workflow files at $HOMEDIR/.termcord/<workflow_name>
---

# Termcord Workflow Skill

This skill teaches the agent how to create termcord workflows.

## What is termcord?

Termcord is a CLI tool for creating terminal workflows/shortcuts. It stores workflows as files in `$HOMEDIR/.termcord/<workflow_name>`.

## How to Create a Workflow

When the user wants to create a termcord workflow:

1. **Determine the workflow name** from the user's request (e.g., "create a workflow called deploy" → workflow name: "deploy")
2. **Write the workflow file** to `$HOMEDIR/.termcord/<workflow_name>`
3. **The workflow file format** is a simple text file with the terminal commands to run, one per line

### Workflow File Format

Each line in the workflow file is a command to execute. Lines starting with `#` are comments.

Example workflow file at `$HOMEDIR/.termcord/deploy`:
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

Example using write tool:
```
write({
  filePath: "C:\\Users\\username\\.termcord\\deploy",
  content: "git push origin main\nssh user@server \"cd /app && git pull && npm install && pm2 restart all\"\necho \"Deploy complete!\""
})
```

Or on Unix-like systems:
```
write({
  filePath: "/home/username/.termcord/deploy",
  content: "git push origin main\nssh user@server \"cd /app && git pull && npm install && pm2 restart all\"\necho \"Deploy complete!\""
})
```

### Running a Workflow

Once created, the user runs it with:
```
termcord run <workflow_name>
```

Or if installed globally via npm:
```
termcord <workflow_name>
```

## Example Usage

User: "Create a termcord workflow called 'deploy' that pushes to git and deploys to server"

Agent:
1. Determine workflow name: "deploy"
2. Ask user for the commands to include
3. Write to `$HOMEDIR/.termcord/deploy` with the commands

User: "Create a workflow called 'build' that runs npm install and npm run build"

Agent writes to `$HOMEDIR/.termcord/build`:
```
npm install
npm run build
```