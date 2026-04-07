# PatchWatch — Agent Entry Point

Read this file first. It tells you who you are, what to read, and where things live.

---

## Roles & Comm Files

| Role | Comm file | Responsibility |
|------|-----------|----------------|
| Planner | `Comms/Comm_Planner.md` | Architecture, structure, roadmap |
| Coder | `Comms/Comm_Coder.md` | Implementation, bug fixes |
| Tester | `Comms/Comm_Tester.md` | Run tests, report bugs, retest |
| Business | `Comms/Comm_Business.md` | Marketing, growth, copy |

**On session start:** check `AGENT_ROLE` env var → read the matching Comm file, not Comm.md.

---

## Key Files

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Full project context, permissions, folder structure |
| `Todo.md` | Active tasks — pick up from here |
| `PRD.md` | Product requirements, user stories, technical architecture |
| `Architecture/services.md` | All external services and credentials map |
| `Architecture/auto-test-spec.md` | Auto-test spec (reference for Tester/Coder) |
| `Test/test-plan.md` | Manual test plan |
| `Decisions/` | One file per architectural decision |
| `Reference/deployment-checklist.md` | Deployment steps and status |

---

## Paths

- **Doc folder (this repo):** `/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch/`
- **Code folder:** `~/Projects/Kodama/Ongoing_Projects/260328_patchwatch`

---

## Tester Loop (no human needed between iterations)

1. Read `Comms/Comm_Tester.md` for current task
2. Read `Test/test-plan.md` and `Architecture/auto-test-spec.md`
3. Run tests in code folder: `cd ~/Projects/Kodama/Ongoing_Projects/260328_patchwatch && npx vitest run`
4. Fix failures → retest → repeat until green
5. Report results in `Comms/Comm_Tester.md`
