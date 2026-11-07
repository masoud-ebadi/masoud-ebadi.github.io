# Urban GIS Lab

Educational site for **Urban Spatial Analysis & Modeling with ArcGIS Pro**, taught by Masoud Ebadi.

Tagline: Practical GIS for Understanding and Planning Cities.

The site is a static [Jekyll](https://jekyllrb.com/) project for [GitHub Pages](https://pages.github.com/). Content is Markdown. There is no database, no paid service, and no frontend framework.

It lives in the `blog` folder of this workspace so it stays separate from the Remotion video project. To publish, use this folder as the root of its own GitHub repository, or point GitHub Pages at it as described below.

## Pages

| Page | URL |
| --- | --- |
| Course landing page | `/` |
| Population density tutorial | `/population-density-analysis-arcgis-pro/` |
| Urban accessibility tutorial | `/urban-accessibility-analysis-arcgis-pro/` |

The course button on every page uses this URL, including the referral code:

`https://www.udemy.com/course/urban-spatial-analysis-with-arcgis-pro/?referralCode=64855D6258A9CB243BD7`

Change it in one place: `course.url` in `_config.yml`.

## Local preview

Install [Ruby](https://rubyinstaller.org/) (3.3 is the version GitHub Pages uses) and Bundler, then from this folder:

```bash
bundle install
bundle exec jekyll serve
```

Open `http://127.0.0.1:4000`.

`bundle exec jekyll build` writes the finished site to `_site/`. Do not commit `_site/`.

## 1. Create the GitHub repository

1. Create a new empty repository on GitHub. Do not add a README, license, or `.gitignore` there if you are about to push this folder.
2. For a user or organization site, name the repository `username.github.io`.
3. For a project site, any repository name is fine. You will set `baseurl` to `/repository-name`.

## 2. Upload or push the project

From this `blog` folder:

```bash
git init
git add .
git commit -m "Publish the Urban GIS Lab site."
git branch -M main
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git push -u origin main
```

Replace `USERNAME` and `REPOSITORY`. If this folder is already inside a larger git repository, publish it as its own repository or follow the project-site note in the next section.

## 3. Enable GitHub Pages

1. Open the repository on GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
4. Set the branch to `main` and the folder to `/ (root)`.
5. Save.

GitHub builds the site with Jekyll. The first build can take a minute. The live address appears at the top of the Pages settings.

Use this branch method **or** the Actions workflow, not both.

## 4. Deployment source

**Branch (recommended).** GitHub runs Jekyll on the repository root. Plugins must be on the [GitHub Pages allow list](https://pages.github.com/versions/). This project uses only `jekyll-seo-tag` and `jekyll-sitemap`, which are allowed. Versions are pinned through the `github-pages` gem in the `Gemfile`.

**GitHub Actions (optional).** `.github/workflows/pages.yml` builds and deploys when you run it by hand.

1. In **Settings → Pages**, set **Source** to **GitHub Actions**.
2. Open the **Actions** tab, choose **Deploy Jekyll site to GitHub Pages**, and run the workflow.

If the site stays inside a larger repository, copy that workflow to the repository-root `.github/workflows/` folder and set `source` to `./blog`.

## 5. Connect a custom domain later

1. In `_config.yml`, set `url` to `https://your-domain.com` and set `baseurl` to `""`.
2. In **Settings → Pages → Custom domain**, enter the domain.
3. Add the DNS records GitHub shows (an `A`/`AAAA` record or a `CNAME`).
4. After DNS resolves, enable **Enforce HTTPS**.
5. Commit the `CNAME` file GitHub adds, then rebuild so canonical URLs and the sitemap use the new origin.

No custom domain is configured in this project.

## 6. Edit and publish a new article

1. Add a file in `_posts/` named `YYYY-MM-DD-slug.md`. The URL becomes `/slug/`. There is no date in the URL.
2. Start the file with front matter:

```yaml
---
layout: post
title: "Article title"
description: "Unique meta description, about 150 characters, with the topic stated plainly."
card_text: "One or two sentences for the homepage card."
card_image: /assets/images/your-diagram.svg
card_alt: "What the diagram shows."
image:
  path: /assets/images/og-your-article.png
  width: 1200
  height: 630
  alt: "Short description of the share image."
---
```

3. Write the article in Markdown. Link the other tutorials with descriptive anchor text, and link the course with `site.course.url`.
4. Add the article to the Tutorials menu in `_includes/header.html`. The homepage card list updates from `site.posts` on its own.
5. Commit and push. GitHub Pages rebuilds.

Keep the editorial rule used for the first two articles: explain the method, name limits of the data, and do not invent ratings, enrollment, prices, certificates, testimonials, or software results.

## 7. Update SEO metadata

Each page needs its own `title` and `description` in front matter. The [Jekyll SEO Tag](https://github.com/jekyll/jekyll-seo-tag) plugin writes the title tag, meta description, canonical URL, Open Graph tags, Twitter card, and baseline JSON-LD.

Shared settings are in `_config.yml`:

- `title`, `description`, `url`, `baseurl`
- `logo` and the default social image
- `twitter.card`

Before the first public build, replace `url`:

```yaml
# User or organization site
url: "https://username.github.io"
baseurl: ""

# Project site
url: "https://username.github.io"
baseurl: "/repository-name"
```

Canonical URLs, Open Graph image URLs, the sitemap, and `robots.txt` all use `url` and `baseurl`. Internal links use the `relative_url` filter so a project site and a later custom domain both work.

Extra structured data is in `_includes/structured-data.html`:

- `Organization` for Urban GIS Lab
- `Course` on the homepage, using the public course facts only
- `BreadcrumbList` on articles

`sitemap.xml` is generated by `jekyll-sitemap` at build time. It is not a file you edit by hand. `robots.txt` allows crawlers and points at that sitemap.

Social images are PNG (1200×630) because sharing platforms expect a raster image. Diagrams on the pages are original SVG files.

## 8. Google Analytics and Search Console

### Analytics

`google_analytics` in `_config.yml` is set to `GOOGLE_ANALYTICS_ID`. The site does not load Google Analytics while that placeholder is in place.

1. Create a GA4 property and copy the measurement ID (`G-XXXXXXXX`).
2. Replace the placeholder in `_config.yml`.
3. Rebuild and deploy.

Course buttons then send a `course_cta_click` event with the link text. No event is sent when analytics is not configured.

### Search Console

1. Open [Google Search Console](https://search.google.com/search-console) and add a property for the live origin (`https://username.github.io` or your domain).
2. Choose the HTML-tag verification method. Copy only the token, not a made-up code.
3. Paste it into `google_site_verification` in `_config.yml`, commit, and wait for the Pages rebuild.
4. In Search Console, open **Sitemaps** and submit `sitemap.xml`.
5. Use **Pages** (or URL Inspection) to see which URLs are indexed. Request indexing for the homepage and the two articles after the sitemap is read.
6. After the site has been live for a few days, use **Performance** to see queries, clicks, and the pages that earn them.

The verification meta tag is omitted until a real token is present.

## Manual checklist before launch

- [ ] Set `url` and, for a project site, `baseurl`.
- [ ] Confirm every course button still uses the referral URL.
- [ ] Add the GA4 measurement ID, or leave the placeholder so analytics stays off.
- [ ] Add the Search Console verification token after Google issues it.
- [ ] Submit `sitemap.xml` and check indexing.
- [ ] Read both articles in a browser on a phone-width window and a desktop window.
- [ ] Do not add ratings, student counts, prices, certificates, or testimonials unless they come from a source you can cite.
