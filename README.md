# Urban GIS Lab

Static HTML / CSS / JS site for **Urban Spatial Analysis & Modeling with ArcGIS Pro**.

- GitHub user: `masoud-ebadi`
- Live site (after correct repo name): https://masoud-ebadi.github.io

## Fix the repository name (required)

Your username is `masoud-ebadi`, so the Pages repo must be named exactly:

**`masoud-ebadi.github.io`**

Right now it is `masoudebadi.github.io` (no hyphen). That breaks the root site.

1. Open https://github.com/masoud-ebadi/masoudebadi.github.io/settings
2. Under **Repository name**, change to: `masoud-ebadi.github.io`
3. Click **Rename**
4. Settings → Pages → Source: **Deploy from a branch** → `main` / `(root)` → Save
5. Wait 1–2 minutes, then open https://masoud-ebadi.github.io

After renaming, run:

```powershell
cd D:\00\video\blog
git remote set-url origin https://github.com/masoud-ebadi/masoud-ebadi.github.io.git
git push -u origin main
```

## SEO files already in the project

| File | Role |
| --- | --- |
| `/robots.txt` | Allows crawlers; sitemap link |
| `/sitemap.xml` | Home + 2 tutorials |
| `/llms.txt` | Summary for AI / LLM systems |
| Page `<head>` | Title, description, canonical, Open Graph, Twitter, JSON-LD |

## Manual SEO steps after the site is live

1. Google Search Console → add `https://masoud-ebadi.github.io`
2. Verify ownership (HTML tag) — send the token if you want it inserted
3. Submit sitemap: `https://masoud-ebadi.github.io/sitemap.xml`
4. Request indexing for home + both tutorials
5. Optional: GA4 measurement ID (`G-XXXXXXXX`)
6. Optional: Bing Webmaster Tools

Do not invent ratings, prices, or student counts.
