# git-it

One-stop save button for git workflows. A Claude Code skill that simplifies version control.

## What it does

- Auto-detects repository state (initializes git if needed)
- Suggests smart `.gitignore` patterns based on your files
- Generates Conventional Commit messages from your changes
- Determines semantic version bumps (patch/minor/major)
- Creates version tags automatically
- Asks for confirmation before any changes

## Usage

Just say:
- **"save my work"** - Save to current branch
- **"/git-it"** - Same as above
- **"save my work somewhere else"** - Create a new branch first

## Requirements

- Git 2.0+
- Claude Code CLI

## License

MIT
