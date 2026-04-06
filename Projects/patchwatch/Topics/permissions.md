# Reducing Permission Prompts in Claude Code

## Agreed scope
Kodama sessions starting from `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch` get full access to:
1. `/Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch/**`
2. `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch/**`
3. `/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Knowledges/**`

No global wildcard `Bash(cd * && ...)` — only these three paths.

## How to apply

This needs to be set in Claude Code's **project-level settings** file. Run this once in your terminal:

```bash
! /permissions
```

Then add these allow patterns when prompted:

```
Bash(cd /Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch* && *)
Bash(find /Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch*)
Bash(cd "/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch"* && *)
Bash(cd "/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Knowledges"* && *)
```

Alternatively, you can edit the project settings file directly at:
`/Users/freedom/.claude/projects/-Users-freedom-Library-Mobile-Documents-iCloud-md-obsidian-Documents-O1-Kodama/settings.json`

Add under `permissions.allow`:
```json
[
  "Bash(cd /Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch* && *)",
  "Bash(find /Users/freedom/Projects/Kodama/Ongoing_Projects/260328_patchwatch*)",
  "Bash(cd \"/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch\"* && *)",
  "Bash(cd \"/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Knowledges\"* && *)"
]
```

## Answers to your questions

**Can you remove settings.local.json?** Yes. It has a bug (`//Users/` double slash) and the stripe commands in it were one-off. Everything important is now in `settings.json`. Delete it.

**Which settings.json is correct?** The one at `.claude/settings.json` inside the patchwatch folder is the correct project-level settings. There's no `settings.json` at the patchwatch root — if you see one there, it's not read by Claude Code.

**Relative paths in settings.json?** No — Claude Code requires absolute paths in `permissions.allow`. Relative paths are not supported.

## Status
Applied. `settings.json` is authoritative.
