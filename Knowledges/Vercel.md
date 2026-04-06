# Vercel — Quick Reference

## Blocking Payments on a Live Site (Test Stripe)

Two easy options to prevent accidental purchases while Stripe is still in test mode:

### Option 1: Environment Variable Gate
Set `NEXT_PUBLIC_PAYMENTS_ENABLED=false` in Vercel dashboard env vars. Wrap upgrade buttons:
```tsx
{process.env.NEXT_PUBLIC_PAYMENTS_ENABLED === 'true' && (
  <UpgradeButton />
)}
```
Quick to toggle on/off. Zero downtime.

### Option 2: Vercel Password Protection
Vercel Dashboard → Project Settings → General → Password Protection.
Blocks the entire site behind a password. No code changes needed.

**Note:** While Stripe is in test mode (`pk_test_` keys), real credit cards are rejected automatically — so there's no risk of accidental real charges. These options are for hiding the incomplete payment UI from visitors.

## Environment Variable Scoping (Stripe + Supabase)

Vercel env vars can be scoped per environment. In the Dashboard or CLI, scope:

| Variable | Production | Preview | Development |
|----------|-----------|---------|-------------|
| `STRIPE_SECRET_KEY` | `sk_live_...` | `sk_test_...` | `sk_test_...` |
| `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` | `pk_live_...` | `pk_test_...` | `pk_test_...` |
| `STRIPE_WEBHOOK_SECRET` | live webhook secret | test webhook secret | test webhook secret |
| `NEXT_PUBLIC_SUPABASE_URL` | prod project URL | dev project URL | dev project URL |
| `SUPABASE_SERVICE_ROLE_KEY` | prod key | dev key | dev key |

**To re-scope an existing var:** delete it in the Dashboard → re-add with correct scope checkboxes ticked. There is no "edit scope" button.

**Supabase keys** (`sb_publishable_`, `sb_secret_`, `SUPABASE_SERVICE_ROLE_KEY`) are Supabase, not Stripe. If you have separate Supabase projects for prod vs dev, scope them the same way.
