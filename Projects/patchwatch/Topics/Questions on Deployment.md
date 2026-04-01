I want to create a `patchwatch.moonleafearth.com`  and link Vercel to it.
I can see the product page on https://patchwatch.moonleafearth.com/. but Vercel setting still look off

### Configuration in CloudFlare

DNS Records: 
* CNAME (type), vercel (name), patchwatch-2604.vercel.app(targed), DNS only, Auto(TTL)
	* This one is added sucessfully
* CNAME (type), vercel (name), cname.vercel-dns-017.com(target), Proxied. Auto(TTL)
	*  "An A, AAAA, or CNAME record with that host already exists."
Q: do I need both?

**[2026-04-01] Kodama:**

No — you only need **one** CNAME record. Here's the fix:

1. **Delete** the first CNAME (`vercel` → `patchwatch-2604.vercel.app`). Pointing to `*.vercel.app` is the old/quick way and doesn't let Vercel manage SSL properly for custom domains.

2. **Keep/add** the CNAME pointing to `cname.vercel-dns.com` (or `cname.vercel-dns-017.com` as Vercel specified). This is the correct target — it lets Vercel handle SSL certificates and routing.

3. **Critical: Set to "DNS only"** (gray cloud), **not** "Proxied" (orange cloud). Cloudflare's proxy intercepts SSL and breaks Vercel's certificate provisioning. This is likely why your Vercel settings "look off" — Vercel can't verify the domain or issue SSL when Cloudflare proxies the traffic.

**Summary of what your Cloudflare DNS should look like:**

| Type | Name | Target | Proxy status |
|------|------|--------|-------------|
| CNAME | patchwatch | cname.vercel-dns.com | DNS only (gray cloud) |

> Note: The "name" field should match your subdomain (`patchwatch` for `patchwatch.moonleafearth.com`). Your current records show `vercel` as the name — double-check this matches the subdomain you actually want.

After updating, go to Vercel → Project Settings → Domains and click **Verify**. The SSL cert should provision within a few minutes.

---

### Configuration in Vercel

Domain -> DNS records
![[error_vercel.png]]











## Reference of instructions:
- [ ] Add CNAME for `patchwatch.moonleaflab.com` pointing to Vercel
- [ ] In Vercel: add `patchwatch.moonleaflab.com` as a custom domain for the project
### Step 6: Connect Domain to Vercel
1. In **Vercel > Project Settings > Domains**, add your domain:
   - If standalone domain: `patchwatch.earth` (or whatever you bought)
   - If subdomain: `patchwatch.moonleaflab.com`
2. Vercel will show DNS records to add. Go to **Cloudflare DNS**:
   - For apex domain (`patchwatch.earth`): Add **A** record → `76.76.21.21` (Vercel's IP)
   - For subdomain (`patchwatch.moonleaflab.com`): Add **CNAME** → `cname.vercel-dns.com`
   - **Important:** In Cloudflare, set the proxy status to **DNS only** (gray cloud, not orange) for Vercel domains
1. Back in Vercel, click **Verify** — should confirm within minutes


