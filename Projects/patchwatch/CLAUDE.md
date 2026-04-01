# PatchWatch — Project Context

## Overview
Satellite land-change monitoring SaaS.

## Code Path
`/Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch`

## Tech Stack
Next.js, Supabase (Postgres + Auth), Stripe, Resend, Copernicus API

## Current Phase
Local dev working. Pre-deployment (see `Reference/deployment-checklist.md`).

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
Topics/         ← deep-dive discussions on a single topic (linked from Comm.md)
```

## Comm.md Protocol
- User writes at the bottom, no prefix
- Kodama responds: `[YYYY-MM-DD] Kodama: ...`
- Task added: `[YYYY-MM-DD] Kodama: Added to Todo.md — "task"`
- Decision made: `[YYYY-MM-DD] Kodama: Decision logged to Decisions/topic.md`
- When resolved, move summary to Log.md and delete from Comm.md
