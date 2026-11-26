# Notes

Claude.md
Used the default structure as learned containing the headers of Claude.md
# Claude.md
Describing the guidance to Claude Code (claude.ai/code) when working with code in this repository.
## Project status
Note which describes the goal and content of the project
## Commands
Put the exact commands Claude needs to build, test, run, and verify the project here. 
Write them down ready to copy and paste, including any non-obvious flags.
## Conventions
This is where you describe the rules a linter doesn't enforce: 
naming conventions, folder structure, error handling, commit and branch names, documentation language, 
and things you want to avoid.
## Architecture
Here, you provide a map of the system: the core components, how they communicate with each other, where specific logic lives, 
and why certain choices were made. Keep it concise. It's a navigation aid, not full documentation.
## Maintenance & health checks
How to keep the project healthy and verify it still works

.claude/settings.json
The file .claude/settings.json is filled accordingly the example in the course:
Shared file — .claude/settings.json Committed to Git, so everyone gets the same rule:
{
  "permissions": {
    "allow": ["Bash(npm test)"],
    "deny": ["Bash(rm -rf *)"]
  }
}
