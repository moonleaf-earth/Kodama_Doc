# PatchWatch — Project Context

## Overview
Satellite land-change monitoring SaaS.

## Self Path
`/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch/`

## Code Path
`/Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch`

## Tech Stack
Next.js, Supabase (Postgres + Auth), Stripe, Resend, Copernicus API

## Current Phase
Local dev working. Pre-deployment (see `Reference/deployment-checklist.md`).


## When session starts

### Check if there are staged changes
```
if ! git diff --cached --quiet; then
    git add . && git commit -m "Commit at starting session"
else
    echo "Nothing to commit, skipping."
fi
```

## Find correct Comm_X for you

If env var AGENT_ROLE is set:
  Skip Comm.md
  Read comms/${AGENT_ROLE}.md 
  If file does not exist: print a warning, continue.
Else:
  ASK which AGENT_ROLE and NAME are you.
`git -c user.name="$AGENT_ROLE_$NAME" -c user.email="$AGENT_ROLE_$NAME@kodama"`


## Folder Structure
```
CLAUDE.md       ← this file (project context for Kodama)
Comm.md         ← active conversation (keep < 50 lines, archive to Log.md)
Log.md          ← resolved conversation summaries
Todo.md         ← current tasks
Done.md         ← completed tasks archive
Comms/          ← all comm_<roles>.md
Decisions/      ← one .md per architectural decision
Architecture/   ← system design, diagrams, API specs
Reference/      ← deployment guides, moved files, external docs
Topics/         ← deep-dive discussions on a single topic (linked from Comm.md)
```

## Comm.md Protocol
- User writes at the bottom, no prefix
- Kodama responds in comm.md: `[YYYY-MM-DD] Kodama: ...`
	- Task added: `[YYYY-MM-DD] Kodama: Added to Todo.md — "task"`
	- Decision made: `[YYYY-MM-DD] Kodama: Decision logged to Decisions/topic.md`
- When resolved, move summary to Log.md and delete from Comm.md

## Permissions
Before starting a long session of actions, If there will be any extra permission needed to be granted, ask all at once before starting action, so the user can be away.  If you encounter any questions can not decide, write in comm.md so user can fix later. 

