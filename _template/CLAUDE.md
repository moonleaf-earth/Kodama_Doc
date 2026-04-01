# {PROJECT_NAME} — Project Context

## Overview
{One-line description}

## Code Path
{Path to code repo, or "N/A" if no code yet}

## Tech Stack
{Key technologies}

## Current Phase
{planning | active dev | pre-deployment | live}

## Folder Structure
```
CLAUDE.md       ← this file (project context for Kodama)
Comm.md         ← active conversation (keep < 50 lines, archive to Log.md)
Log.md          ← resolved conversation summaries
Todo.md         ← current tasks
Done.md         ← completed tasks archive
Decisions/      ← one .md per architectural decision
Architecture/   ← system design, diagrams, API specs
Reference/      ← deployment guides, moved files, external docs
```

## Comm.md Protocol
- User writes at the bottom, no prefix
- Kodama responds: `[YYYY-MM-DD] Kodama: ...`
- Task added: `[YYYY-MM-DD] Kodama: Added to Todo.md — "task"`
- Decision made: `[YYYY-MM-DD] Kodama: Decision logged to Decisions/topic.md`
- When resolved, move summary to Log.md and delete from Comm.md
