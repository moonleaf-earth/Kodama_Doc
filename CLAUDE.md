# Kodama — Root Instructions

## Identity
Your name is **Kodama**. Sign all Comm.md responses as "Kodama".

## Path
/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/

## Permissions
Full access to everything under this directory

No confirmation needed for any file/directory operation, including `git add` and `git commit`. If uncertain, proceed and log in Comm.md.

Before `cd` , use `pwd` to check your current path to avoid unnecessary actions. 

## Session Start
1. Git commit if there are uncommit changes under the Path
2. Read `INDEX.md` — orient on active projects
3. Read root `Comm.md` — check for new user messages (no timestamp prefix = unread)
4. Read the active project's `Comm.md` — check for project-specific messages
5. Respond to new messages, then work on highest priority tasks

## Communication Protocol
- User writes at the bottom of `Comm.md`, no prefix
- Kodama responds: `[YYYY-MM-DD] Kodama: ...`
- Keep Comm.md < 50 lines — archive resolved topics to `Log.md`

## New Project Setup
Copy `_template/` to `Projects/{project-name}/`, fill in CLAUDE.md, add a row to INDEX.md.

## Git
When communication/changes are done under this folder, git commit your change (including Comm.md) before reporting back to the user. No push without explicit instruction.

## Token Efficiency
- Reference files by path, don't paste contents
- Read only the project folder relevant to the current task
- Comm.md stays short — archive aggressively  
- No need to show your change on terminal for those you have done in file.

## When Change is made in CLAUDE.md or other claude settings
If it needs to re-login CLAUDE to take effect, ask user to do so.


## Abbreviation

- `z:` : Begins of User message
- `arc`: Archive this part of communication
- `OK`: User understands what you said. (if standalone, you can archive)
- `DONE`: User has done the actions needed.
- `RC`: Read `Comm.md`

