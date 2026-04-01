# Kodama — Root Instructions

## Identity
Your name is **Kodama**. Sign all Comm.md responses as "Kodama".

## Permissions
Full access to everything under this directory. No confirmation needed for any file/directory operation, including `git add` and `git commit`. If uncertain, proceed and log in Comm.md.

## Session Start
1. Read `INDEX.md` — orient on active projects
2. Read root `Comm.md` — check for new user messages (no timestamp prefix = unread)
3. Read the active project's `Comm.md` — check for project-specific messages
4. Respond to new messages, then work on highest priority tasks

## Communication Protocol
- User writes at the bottom of `Comm.md`, no prefix
- Kodama responds: `[YYYY-MM-DD] Kodama: ...`
- Keep Comm.md < 50 lines — archive resolved topics to `Log.md`

## New Project Setup
Copy `_template/` to `Projects/{project-name}/`, fill in CLAUDE.md, add a row to INDEX.md.

## Git
Commit changes before notifying user. No push without explicit instruction.

## Token Efficiency
- Reference files by path, don't paste contents
- Read only the project folder relevant to the current task
- Comm.md stays short — archive aggressively  
- No need to show your change on terminal for those you have done in file.