# Deploy SuperDaddy public legal pages

App Store Connect needs **public HTTPS** URLs. This folder is a static site:

| Page | File | ASC field |
|------|------|-----------|
| Privacy Policy | `privacy.html` | Privacy Policy URL |
| Terms | `terms.html` | (optional / support) |
| Support | `support.html` | Support URL |

Canonical URLs (after you connect the domain or host):

- `https://www.superdaddy.app/legal/privacy`
- `https://www.superdaddy.app/legal/terms`
- `https://www.superdaddy.app/support`

The iOS app already points at those URLs via `LegalDocuments.swift`.

## Fastest options

### A) Cloudflare Pages / Netlify / Vercel (recommended)

1. Create a free account.
2. Deploy this `website/` folder as a static site.
3. Either:
   - use the provider subdomain temporarily and **update** `LegalDocuments` URLs to match, or
   - attach custom domain `www.superdaddy.app` and map:
     - `/legal/privacy` → `privacy.html`
     - `/legal/terms` → `terms.html`
     - `/support` → `support.html`

Simple path rewrite example (Netlify `_redirects`):

```
/legal/privacy  /privacy.html  200
/legal/terms    /terms.html    200
/support        /support.html  200
/legal          /index.html    200
```

### B) GitHub Pages

1. Push this repo (or only `website/`) to a public GitHub repository.
2. Settings → Pages → Deploy from branch → `/website` or `/ (root)`.
3. If using `username.github.io/repo/privacy.html`, temporarily set app URLs to that host, **or** add custom domain `www.superdaddy.app`.

### C) Local preview

```bash
cd website
python3 -m http.server 8088
# open http://127.0.0.1:8088/privacy.html
```

## Verify before submit

```bash
curl -I https://www.superdaddy.app/legal/privacy
curl -I https://www.superdaddy.app/support
```

Expect `HTTP/2 200` (or 200). Apple’s crawler must fetch without login.
