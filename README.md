# mikactech.com

Static capability-statement site for Mikac Technologies LLC. Four pages, one
stylesheet, no build step, no JavaScript.

## Local preview

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

## Deploying to GitHub Pages

Current DNS state for `mikactech.com`:

- Registrar / DNS: Network Solutions (`ns1.systemdns.com`)
- Email: Zoho Mail (three `MX` records plus an SPF `TXT` record)
- `www`: currently a `CNAME` to `zhs.zohosites.com` (a Zoho Sites placeholder)

Nameservers stay at Network Solutions. **Do not touch the `MX` records or the
`v=spf1 include:zohomail.com ~all` TXT record** — those are what make
`erik@mikactech.com` work.

### 1. Push the repo

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin git@github.com:<username>/mikactech-site.git
git push -u origin main
```

The repo must be public for GitHub Pages to serve it on the free tier.

### 2. Enable Pages

Repository → Settings → Pages. Source: "Deploy from a branch", branch `main`,
folder `/ (root)`. The `CNAME` file in this directory sets the custom domain to
`mikactech.com` automatically. Once DNS resolves, tick **Enforce HTTPS**.

### 3. DNS records to change at Network Solutions

Replace the apex `A` record (currently the parked `64.99.64.37`) with GitHub's
four:

| Type | Host | Value |
| --- | --- | --- |
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

Repoint `www` from Zoho Sites to GitHub:

| Type | Host | Value |
| --- | --- | --- |
| CNAME | www | `<username>.github.io.` |

With the apex set as the custom domain, GitHub redirects `www` to
`mikactech.com` on its own. Leave everything else in the zone alone.

Certificate issuance can take up to an hour after DNS propagates. Enforce HTTPS
before sharing the URL — federal users on managed networks are often blocked
from plain HTTP.

## Content that needs updating later

These values are deliberately marked as pending. Update them everywhere they
appear when the underlying status changes:

| Item | Current value | Appears in |
| --- | --- | --- |
| CAGE code | "In process" | `index.html`, `contact.html` |
| SAM.gov registration | "In process" | `contact.html` |
| SDVOSB certification | "In progress" | all four pages, including footers |

The SDVOSB language appears in the footer of every page and in the registration
status section of `contact.html`. Do not change it to a certified claim until
SBA VetCert has actually been issued — misrepresenting socioeconomic status
carries real penalties, and the current wording is accurate as written.

## Accessibility notes

The site is built to be straightforwardly Section 508 conformant: semantic
landmarks, a skip link, a logical heading order, visible focus indicators, text
contrast above 4.5:1, and a layout that reflows to 320px without horizontal
scrolling. If you add pages, keep those properties — accessibility is a real
evaluation factor in federal work and the site is itself a work sample.

## Print

`styles.css` includes a print stylesheet that drops the navigation and footer,
so any page can be printed or saved to PDF as a leave-behind.
