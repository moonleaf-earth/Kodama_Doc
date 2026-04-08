
Doc folder: /Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch
Code Folder: ~/Projects/Kodama/Ongoing_Projects/260328_patchwatch

[2026-04-07] Kodama: Archived previous conversation to Log.md. Applied the GitHub Actions + claude-code-action approach:

**Created:** `code-repo/.github/workflows/claude-test-fix.yml`

**What it does:**
1. Checks out both `moonleaf-earth/patchwatch` (code) and `moonleaf-earth/Kodama_Doc` (docs) side-by-side
2. Installs deps + Playwright
3. Runs Claude with `anthropic/claude-code-action@beta` — reads Comm_Tester.md, fixes bugs, runs tests, iterates
4. Pushes fixes to both repos

**Schedule:** daily 22:00 Stockholm (same as old trigger) + manual `workflow_dispatch`

**Secrets needed in GitHub (code repo → Settings → Secrets):**

| Secret                          | What                                           | Status                   |
| ------------------------------- | ---------------------------------------------- | ------------------------ |
| `ANTHROPIC_API_KEY`             | Anthropic API key                              | needs adding             |
| `CROSS_REPO_PAT`                | GitHub PAT with `contents:write` on both repos | you may already have one |
| `E2E_EMAIL`                     | `coworker_patchwatch@moonleafearth.com`        | needs adding             |
| `E2E_PASSWORD`                  | password for that account                      | needs adding             |
| `NEXT_PUBLIC_SUPABASE_URL`      | Supabase project URL                           | needs adding             |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon key                              | needs adding             |
| `STRIPE_SECRET_KEY`             | Stripe test key                                | needs adding             |
| `RESEND_API_KEY`                | Resend API key                                 | needs adding             |

**vs old trigger:** No PAT in plaintext, both repos on disk (not cloned mid-prompt), credentials as env vars not in prompt text, full audit trail in GitHub Actions logs.

**Where to add secrets:** Code repo only (`moonleaf-earth/patchwatch` → Settings → Secrets). The workflow runs there and checks out the doc repo via the PAT — doc repo needs no secrets.

**Next steps:**
1. Add all 8 secrets in the code repo on GitHub
2. Push the code repo (workflow commit is ready)
3. Test with manual dispatch from Actions tab

Z: 
1. I set those 8 secrets in the code repo, under environment  `Configure 0408_test`
    Maybe you need to change code repo?

[2026-04-07] Kodama: Added `environment: Configure 0408_test` to the workflow so it picks up those secrets. Ready to push the code repo and test.

[2026-04-08] Kodama: Workflow debugged and working. 6 fixes applied:

1. `npm ci` lock mismatch → ran `npm install` to sync package-lock.json
2. Missing `permissions: id-token: write` → added (required by claude-code-action OIDC)
3. `prompt` → `direct_prompt` (correct input name for agent mode)
4. `mode: agent` added (required for `workflow_dispatch` trigger)
5. `NEXT_PUBLIC_SUPABASE_ANON_KEY` → `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` (matches codebase)
6. Environment name: `0408_test` (not `Configure 0408_test`)

**Result:** Workflow runs successfully — checkout, install, Playwright all pass. Claude step reached but returned **"Credit balance is too low"** on the ANTHROPIC_API_KEY.

**Action needed:** Add credits at https://console.anthropic.com → then re-run from Actions tab.

