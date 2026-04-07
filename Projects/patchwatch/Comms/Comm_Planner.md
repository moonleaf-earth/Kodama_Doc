
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

| Secret | What | Status |
|--------|------|--------|
| `ANTHROPIC_API_KEY` | Anthropic API key | needs adding |
| `CROSS_REPO_PAT` | GitHub PAT with `contents:write` on both repos | you may already have one |
| `E2E_EMAIL` | `coworker_patchwatch@moonleafearth.com` | needs adding |
| `E2E_PASSWORD` | password for that account | needs adding |
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL | needs adding |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon key | needs adding |
| `STRIPE_SECRET_KEY` | Stripe test key | needs adding |
| `RESEND_API_KEY` | Resend API key | needs adding |

**vs old trigger:** No PAT in plaintext, both repos on disk (not cloned mid-prompt), credentials as env vars not in prompt text, full audit trail in GitHub Actions logs.

**Next steps:**
1. Add the secrets above in GitHub
2. Push the workflow file (commit is ready locally)
3. Test with manual dispatch from Actions tab
4. Optionally disable the old Claude trigger `trig_01XrcuBtFcuCm86FMypM8omv`

