# Urban GIS Lab

Static HTML / CSS / JS site for **Urban Spatial Analysis & Modeling with ArcGIS Pro**.

Live site: https://masoud-ebadi.github.io

## Technical SEO files (already on the site)

| File | Role |
| --- | --- |
| `/robots.txt` | Allows crawlers; points to sitemap |
| `/sitemap.xml` | Homepage + 2 tutorials |
| `/llms.txt` | Plain-language summary for AI / LLM systems |
| Page `<head>` | Unique title, description, canonical, Open Graph, Twitter, JSON-LD |

## Edit content

- Homepage: `index.html`
- Articles: `*/index.html`
- Styles: `assets/css/style.css`
- Course URL (referral): search for `referralCode=64855D6258A9CB243BD7`

## Deploy

Push to `main` on `urbangispro/masoud-ebadi.github.io`.  
Pages source: Deploy from a branch → `main` / root.  
`.nojekyll` keeps GitHub Pages from running Jekyll.

---

## Manual checklist (you need to do these)

### 1. Google Search Console

1. Open https://search.google.com/search-console
2. Add property: **URL prefix** → `https://masoud-ebadi.github.io`
3. Verify with **HTML tag** (copy the token only)
4. Paste the token into each page `<head>` as:
   `<meta name="google-site-verification" content="YOUR_TOKEN">`
   Or ask the assistant to add it after you paste the token here.
5. Submit sitemap: `https://masoud-ebadi.github.io/sitemap.xml`
6. Use **URL Inspection** on:
   - `https://masoud-ebadi.github.io/`
   - `https://masoud-ebadi.github.io/population-density-analysis-arcgis-pro/`
   - `https://masoud-ebadi.github.io/urban-accessibility-analysis-arcgis-pro/`
7. Request indexing for each.

### 2. Bing Webmaster Tools (recommended)

1. https://www.bing.com/webmasters
2. Add the same site and sitemap.

### 3. Google Analytics 4 (optional)

1. Create a GA4 property → Measurement ID `G-XXXXXXXX`
2. Send the ID to the assistant, or add this before `</head>` on every page:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXX', { anonymize_ip: true });
</script>
```

### 4. Validate structured data

1. https://search.google.com/test/rich-results  
2. Test homepage + both articles.  
3. Fix only real errors (warnings about missing optional Course fields are OK; do **not** invent ratings or prices).

### 5. Share / social preview

1. https://www.opengraph.xyz or Facebook Sharing Debugger  
2. Paste each URL and confirm title, description, image.

### 6. Ongoing content (SEO growth)

1. Publish more English tutorials with one clear primary keyword each.  
2. Keep internal links between related articles and the homepage.  
3. Always use the exact Udemy referral URL for CTAs.  
4. Update `sitemap.xml` `lastmod` when you change a page.  
5. Keep `llms.txt` accurate when course facts change on Udemy.

### 7. Do not invent

Do not add fake ratings, student counts, prices, discounts, or testimonials.
