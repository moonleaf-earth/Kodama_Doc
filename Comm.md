1. I want this folder become the source of truth for all docs of any projects. My vision is to put all communication with Claude, all decision made,  all system architectures,  all external service used in one places -- here.  So there is a folder for code, and all other information on projects are HERE.
2. A usage example can be, 
	1. Kodama :
		1. make the code under /Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch
		2. In that session, all communication, idea, PRD,  task todo, and decisions all update  here: `/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch`
	2. Thus I can just add my new thoughts in Obsidan mobil app
3. I add some folder and file to start, could you please revise and give recommendations how to make these datacollection/information sharing /async work more seamlessly and token effective?

---

[2026-04-01] Kodama:

**Love this idea.** Separating "the brain" (here, in Obsidian/iCloud) from "the hands" (code repos) is exactly the right pattern. You get mobile access, cross-project context, and async handoff — all things that break when docs live inside code repos.

Here's what I think works well, what could be sharper, and a proposed standard structure:

### What's Already Strong
- **Obsidian as the single source** — mobile access means you can capture thoughts anytime
- **Per-project folders** (`Projects/patchwatch/`) — scales naturally to more projects
- **Separation of Communications vs WorkLog vs Frameworks** — good conceptual split

### Recommendations

**1. Standardize the per-project template**

Every new project folder should look like this:

```
Projects/{project-name}/
├── CLAUDE.md          ← project-specific instructions for Kodama
├── Comm.md            ← active conversation (kept short)
├── Log.md             ← resolved conversations archive
├── Todo.md            ← current tasks (replaces WorkLog/)
├── Done.md            ← completed tasks archive
├── Decisions/         ← one .md per decision (not one big file)
│   └── 2026-03-28_stripe-vs-lemonsqueezy.md
├── Architecture/      ← system design docs, diagrams, API specs
└── Reference/         ← moved files, external docs, deployment guides
```

Why flatten vs. the current `Communications/` + `WorkLog/` + `Frameworks/` split: fewer clicks in Obsidian mobile, and Kodama reads fewer paths = fewer tokens per session start.

**2. Keep Comm.md short — archive aggressively**

The #1 token cost is Comm.md growing unbounded. Rule:
- Once a topic is **resolved**, move a 1-2 line summary to `Log.md` and **delete** it from `Comm.md`
- Comm.md should ideally be < 50 lines at any time

**3. Use a root-level INDEX.md**

At `Kodama/INDEX.md`, keep a one-liner per project:

```
| Project | Status | Code Path | Notes |
|---------|--------|-----------|-------|
| patchwatch | active | ~/Projects/.../260328_patchwatch | satellite monitoring SaaS |
| (next project) | planning | — | — |
```

This lets Kodama orient instantly without reading every subfolder.

**4. Tag conventions for Obsidian**

Use a simple tag system so you can filter in Obsidian mobile:
- `#kodama/question` — waiting for Kodama to answer
- `#kodama/done` — Kodama has responded
- `#decision` — architectural decision
- `#blocked` — needs user action

**5. CLAUDE.md: one at root, one per project**

- **Root `Kodama/CLAUDE.md`** — global instructions (identity, permissions, session protocol) — already done ✓
- **Per-project `CLAUDE.md`** — project-specific context (code path, tech stack, services used, current phase). This is what Kodama reads at session start to orient. Keep it under 30 lines.

**6. Token efficiency tips**

- Don't duplicate info that's in code (e.g., don't copy package.json contents here)
- Reference files by path instead of pasting content: "see `Reference/deployment-checklist.md`"
- Decisions/ folder with individual files > one big DECISIONS.md (Kodama only reads the relevant one)

### On the patchwatch Comm.md tasks
- Task 1 (move .md files): Ready to do on your go-ahead
- Task 2 ("Questions on Deployment.md"): That file is currently empty — add your questions there and I'll answer them next session

### Next step
Want me to restructure the current `Projects/patchwatch/` folder to match the proposed template above? I can do it now and move files accordingly.


--
User: Do it! Restructure and update CLAUDE.md when you see fit.  Create also a project template folder which can be easily reused when there are more projects.

btw, what's most token efficient way to notify you on terminal so you read my answer?

---

[2026-04-01] Kodama:

**Done.** Restructured patchwatch and created the template. Here's what changed:

**Patchwatch** (`Projects/patchwatch/`):
```
Before:                     After:
Communications/             CLAUDE.md (updated)
  CLAUDE.md                 Comm.md
  Comm.md                   Log.md
  Log.md                    Todo.md (was WorkLog/kodama-todo.md)
  Questions on Deployment   Done.md (was WorkLog/Kodama_Done.md)
Frameworks/  (empty)        Decisions/
WorkLog/                    Architecture/
  kodama-todo.md            Reference/
  Kodama_Done.md              deployment-checklist.md (was Questions on Deployment.md)
```

**New reusable template** at `_template/` — copy to `Projects/{name}/` for any new project.

**New INDEX.md** at root — one-line-per-project table for instant orientation.

**Updated root CLAUDE.md** — streamlined session protocol with token efficiency rules.

---

**Re: most token-efficient way to notify me:**

Just type exactly this in the terminal:

```
Read Comm.md and answer it.
```

That's it — 6 words, ~10 tokens. I'll read the file and pick up whatever you wrote. No need for longer prompts.

--
User: Now I'm satisfied, how do I make you know you can archive, or will you automatically do that?

[2026-04-01] Kodama: I'll do it automatically. The rule in CLAUDE.md says "keep Comm.md < 50 lines, archive aggressively." So each session when I read Comm.md, if resolved topics are piling up, I'll move summaries to Log.md and trim Comm.md on my own. You don't need to tell me — it's part of the protocol now.

If you ever want to force an archive mid-conversation, just write "archive" in Comm.md and I'll do it immediately.