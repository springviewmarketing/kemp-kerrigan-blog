# Kemp & Kerrigan blog: rules for Claude

Blog for Kemp & Kerrigan, a single-practice independent opticians at 8 Brooklands Avenue, Fulwood, Sheffield S10 4GA, run single-handedly by optometrist Adam Kerrigan. The marketer is Tom (Spring View Marketing). Tom is not a developer: make technical decisions yourself, never ask him to run commands, and explain anything he must do in plain English.

The practice's main website, https://kempkerrigan.co.uk, is a one-page Carrd site that is **not** in this repository. The blog must feel like part of the same practice but be better designed.

## Hosting and build

- Jekyll, hosted free on GitHub Pages from `main`, root folder, using **GitHub Pages' built-in Jekyll build only**. No GitHub Actions workflow. No `.nojekyll` file.
- Plugins: only `jekyll-seo-tag`, `jekyll-sitemap`, `jekyll-feed`. Do not add others (GitHub Pages will not run them). The `Gemfile` uses the `github-pages` gem so a local build matches production.
- `theme: null` in `_config.yml` is deliberate: without it GitHub Pages injects its Primer theme.
- Local build: `bundle config set --local path vendor/bundle && bundle install`, then `LC_ALL=C.UTF-8 bundle exec jekyll build`. Always build after changes and fix every error before committing.
- Chromium in the cloud container cannot reach Google Fonts directly. For screenshots, download the font CSS and files with `curl` and serve them locally via Playwright `page.route`.

### Preview vs live

- Preview address: https://springviewmarketing.github.io/kemp-kerrigan-blog/ (`url: "https://springviewmarketing.github.io"`, `baseurl: "/kemp-kerrigan-blog"`, `preview_mode: true`).
- While `preview_mode` is true every page outputs `<meta name="robots" content="noindex, nofollow">` and `robots.txt` disallows everything. (A project site's robots.txt is not at the domain root, so the meta tag is what actually protects the preview.)
- **Going live** on https://blog.kempkerrigan.co.uk is a config change only: `url: "https://blog.kempkerrigan.co.uk"`, `baseurl: ""`, `preview_mode: false`, add a `CNAME` file containing `blog.kempkerrigan.co.uk`, then Tom sets the custom domain and DNS (CNAME record `blog` → `springviewmarketing.github.io`) and ticks "Enforce HTTPS". Delete the sample post first.
- Therefore every internal link, image, stylesheet and script in templates must use `relative_url` or `absolute_url`. Never hard-code `/kemp-kerrigan-blog`.
- In post Markdown and FAQ answers, write internal links as root-relative paths (`/post-slug/`). `_includes/fix-links.html` adds the baseurl automatically. Figure `src` values go through `relative_url` inside the include.

## Files

```
_config.yml              site settings; practice details live under `practice:`
_layouts/default.html    page shell (head, skip link, header, main, footer)
_layouts/post.html       post page + build checks
_includes/head.html      meta, SEO (plugin JSON-LD stripped), fonts, CSS, JS
_includes/header.html    charcoal header and navigation
_includes/footer.html    charcoal footer
_includes/post-card.html one card; used by the blog home and "More from the blog"
_includes/more-posts.html
_includes/faqs.html      <details> FAQs
_includes/cta.html       call-to-action box
_includes/figure.html    the only way to put an image in a post body
_includes/fix-links.html adds baseurl to root-relative links
_includes/schema-practice.html  Optician JSON-LD, every page
_includes/schema-post.html      BlogPosting + BreadcrumbList (+ FAQPage) JSON-LD
_posts/                  posts, named YYYY-MM-DD-slug.md
assets/css/main.css      the only stylesheet
assets/js/nav.js         the only script (mobile menu toggle)
assets/brand/            kk-logo.png (original), kk-logo-header.png (cropped to the artwork, 440px)
assets/images/           WebP photos, max 1600px wide, lowercase-hyphenated names
index.html               blog home
404.html                 not-found page
robots.txt               depends on preview_mode
```

## Brand

No brand guide exists; these values come from the live main site. Do not introduce any other colours beyond neutral tints of these.

| Token | Value | Use |
|---|---|---|
| `--charcoal` | #262020 | Header and footer bands, primary buttons, CTA box, headings |
| `--tan` | #CD9D6D | Only on charcoal: button text and edge, active nav item, thin rules, CTA heading (6.6:1). Fails on white (2.43:1): never use it as text on light backgrounds |
| `--tan-dark` | #8A5A2E | Links, list markers, focus ring on light backgrounds (5.86:1 on white, 5.01:1 on #EDEDED) |
| `--tan-darker` | #6E4724 | Link hover on light (8.11:1) |
| `--page` | #EDEDED | Page background |
| `--surface` | #FFFFFF | Article surface, cards |
| `--text` | #2B2424 | Body text, a charcoal tint (15.2:1) |
| `--muted` | #5C5252 | Meta lines, captions (7.5:1 on white, 6.4:1 on #EDEDED) |
| `--rule` | #D9D4D4 | Hairlines on light |
| `--on-dark` / `--on-dark-muted` | #FFFFFF / #D9D4D4 | Text on charcoal (16:1 / 10.9:1) |

- All text must pass WCAG AA. Check the ratio for any new pairing before using it.
- Font: Source Sans 3 (Google Fonts) at 400/600/700 with a system sans-serif fallback. Stay sans-serif. No italics are loaded; don't rely on them.
- Logo: `kk-logo.png` is tan on transparent, designed for charcoal. Only place it on charcoal. It links to https://kempkerrigan.co.uk/. The artwork includes the strapline "EYES & EARS"; that is the practice's own mark, so leave it as is, but never add hearing or ear content anywhere else.
- One primary button style everywhere: charcoal fill, tan text, 2px tan edge; hover fills tan with charcoal text. It works on white and on charcoal.

## Design system

- **Spacing scale only**: 4, 8, 12, 16, 24, 32, 48, 64, 96px (`--s4` … `--s96`). No other spacing values.
- **Measure**: article text column `--measure: 35.5rem` (about 68 characters a line at 19px in Source Sans 3; `68ch` measured ~74 so it is not used). Body 18px on phones, 19px from 768px, line-height 1.7.
- **Rhythm** (`.prose`): blocks get margin-top only. Paragraphs, lists 24px apart. H2: 48px above (64px desktop), 12px below, so question headings sit with their answers. H3: 32px (48px) above, 12px below. Figures and quotes: 32px (48px) above and below. List items 8px apart.
- **Headings**: one H1 per page. H1 32px → 44px, H2 24px → 28px, H3 20px → 22px, all 700, charcoal. Post H2s are usually questions.
- **Images**: in-column (default) or deliberately wider with `class="wide"` (full 1040px content width). Never any other width. Hero and card images are always 3:2 with `object-fit: cover`. One radius everywhere: `--radius: 6px`. Captions directly below in 15px muted text. Every `<img>` has width and height attributes. `loading="lazy"` on everything except the post hero (`fetchpriority="high"`) and the first card on the blog home (which is the above-the-fold image there).
- **Layout**: mobile first, 20px gutters on phones (32px from 768px). Content max width 1040px. Tap targets at least 44px. Navigation collapses behind a "Menu" button below 960px, but only when JS runs (`.js` class), so it still works without JavaScript.
- **Card grid**: 1 column on phones, 2 from 640px, 3 from 1024px (implemented as a 6-track grid). Classes `card-grid--m2-N` and `card-grid--m3-N` (post count mod 2 / mod 3) fill every row with no gaps: a leftover single post becomes a wide horizontal featured card (the newest); on desktop, two leftover posts share the first row half and half.
- **No frameworks, no jQuery.** The only JS is `assets/js/nav.js`, plus the one-line `js` class setter in the head.
- **Accessibility**: semantic HTML, skip link, visible `:focus-visible` rings (tan-dark on light, tan on charcoal), AA contrast, `aria-current` on Blog in the nav, breadcrumb `nav`, FAQs as native `<details>`.

## Page components

- **Header** (charcoal): logo → https://kempkerrigan.co.uk/; links Home → https://kempkerrigan.co.uk/, Eye examinations → https://kempkerrigan.co.uk/#eyeexamination, About → https://kempkerrigan.co.uk/#aboutus, Blog (this site, shown as current). "Book an eye test" button → https://kempkerrigan.co.uk/#eyeexamination. **Never add an ear or hearing link.**
- **Blog home**: H1 "Kemp & Kerrigan blog", one line "Eye care advice and news from Adam Kerrigan, independent optometrist in Fulwood, Sheffield.", then post cards newest first (image, title, date, excerpt from `description` clamped to two lines).
- **Post**: breadcrumb (Blog › title), H1, meta line (date, "Updated …" when `last_modified_at` is later, "x min read" at 200 words a minute), hero with optional caption, body, optional FAQs, CTA box, "More from the blog" (up to 3 other posts; hidden when there are none).
- **CTA box** (charcoal, tan heading): "Book an eye test with Adam", "Phone the practice to arrange your appointment.", button "Call 0114 430 0221" (`tel:+441144300221`), secondary link "About eye examinations" → https://kempkerrigan.co.uk/#eyeexamination. **No online booking link.**
- **Footer** (charcoal): Kemp & Kerrigan, 8 Brooklands Avenue, Fulwood, Sheffield S10 4GA. Tel 0114 430 0221. "Opening hours on Google" → https://www.google.com/maps/place/?q=place_id:ChIJ1ZvvVgCBeUgRqcCb0rHUDLI (hours change, so **never type opening hours anywhere**). Instagram https://www.instagram.com/kempandkerrigan/, Facebook https://www.facebook.com/kempandkerrigan/, link to the main site. **No email address anywhere.**
- **404**: same style, links to the blog home and the main site, always noindex.

## SEO and AI visibility

- `jekyll-seo-tag` handles title, description, canonical, Open Graph and Twitter cards. Title format: "Post title | Kemp & Kerrigan Opticians" (`site.title` is "Kemp & Kerrigan Opticians"). `lang: en-GB`, `locale: en_GB`.
- The plugin always prints its own JSON-LD; `head.html` captures `{% seo %}` and cuts that block off, because the site writes its own structured data. Keep it that way to avoid duplicate/conflicting entities.
- Default share image for non-post pages: `adam-kerrigan-eye-examination.webp` (set in `_config.yml` defaults for pages). Posts use their hero image.
- Site-wide JSON-LD (`schema-practice.html`): `@type` Optician, name "Kemp & Kerrigan", url https://kempkerrigan.co.uk, telephone +44 114 430 0221, full postal address, hasMap (Google link), sameAs Instagram and Facebook. **No opening hours, no price range.**
- Every post: BlogPosting (headline, description, image, datePublished, dateModified, author and publisher both the practice), BreadcrumbList, and FAQPage when `faqs` exist.
- `jekyll-sitemap`, `jekyll-feed` (`/feed.xml`), `robots.txt` pointing at the sitemap when not in preview mode.
- Permalinks: `/:title/` (the slug from the filename), no dates in URLs. Once a post is live, never rename its file.

## Build checks (in `_layouts/post.html` and `_includes/figure.html`)

These deliberately `{% include %}` a file that doesn't exist, so the GitHub Pages build fails and the error names the problem. Keep them, and add new ones the same way.

- Post has no `image.path` → `ERROR-post-has-no-hero-image…`
- Hero has no `image.alt` → `ERROR-hero-image-has-no-alt-text…`
- A figure include has no `alt` → `ERROR-an-image-in-this-post-has-no-alt-text--add-alt-to-the-figure-include`
- Any `alt=""` in the rendered body (e.g. Markdown `![](…)`) → `ERROR-an-image-in-this-post-has-no-alt-text`
- An em dash (`—`, or `---` which kramdown turns into one) in the title, description, caption, alt text, FAQs or body → `ERROR-em-dash-found…`
- An FAQ missing its question or answer → `ERROR-faq-needs-both-a-question-and-an-answer`

When a build fails on GitHub, the post simply doesn't appear and the previous version stays live. Tell Tom where to look: repository → Actions tab → the failed "pages build and deployment" run.

## Content rules (all text: posts, sample text, alt text, labels, captions)

- British English. **No em dashes.** "Patients", never "customers" or "clients".
- One person runs this practice: never write "our team" or a "we" that implies staff. Never suggest dropping in to browse or try on frames.
- Adam is an **optometrist**. Never call him an ophthalmologist.
- Never mention hearing or ear services, prices, frame brand names, email addresses, online booking, or any equipment brand names.
- Don't invent facts. Health claims and statistics need a real, checked source supplied by Tom. If something is missing, leave a clearly marked `TODO:` and tell Tom.
- Spring View Marketing house style for real posts: about 2,000 words of continuous prose, question-style H2s with a direct answer in the first one to three sentences, no bullet lists or bold inside running prose, no preamble, plain language, a genuine FAQ section at the end, descriptive link text (never "click here"). The list and quote styles exist, but use them sparingly.
- Photography: authentic practice photos only. Never stock or AI-generated images. Avoid crops where equipment or frame brand names are legible when choosing a hero.
- Nothing goes live without Tom reviewing it.

## Images

- Save to `/assets/images/` as WebP, quality about 80, max 1600px wide, short descriptive lowercase-hyphenated names (e.g. `adam-kerrigan-fitting-glasses.webp`). Delete originals from the repository root after converting.
- Current library (all 1600 × 1067, 3:2):
  - `adam-kerrigan-eye-examination.webp`: Adam examining a patient's eyes (default share image)
  - `adam-kerrigan-consultation-desk.webp`: Adam at his desk holding up a pair of glasses
  - `adam-kerrigan-fitting-glasses.webp`: Adam checking the fit of glasses on a young patient
  - `adam-kerrigan-portrait-frame-wall.webp`: portrait, arms folded, in front of the frame display
  - `adam-kerrigan-portrait-patterned-wall.webp`: portrait beside the frame display and patterned wallpaper
  - `frame-display-wall.webp`: the frame display wall (a frame brand name is legible; avoid as a hero)
  - `dispensing-bench.webp`: bench with mirror and instruments
  - `patient-refraction-close-up.webp`: close-up of a patient behind the refraction head (equipment brand legible)
  - `patient-visual-field-test.webp`: patient at a visual field screener (equipment brand legible)

## How to publish a post

Create `_posts/YYYY-MM-DD-slug.md`. The slug becomes the URL (`/slug/`), so make it short and keyword-led. `_posts/2026-09-25-sample-post-layout-test.md` shows every field with comments.

```yaml
---
title: "Question-led title under about 60 characters"      # required
description: "One or two sentences, ~150 characters."        # required: meta description, share text, card excerpt
date: 2026-10-01 09:00:00 +0100                              # required; UK offset (+0100 BST, +0000 GMT). Future dates don't publish until a later build
last_modified_at: 2026-10-08 09:00:00 +0100                  # optional; shows "Updated …" and sets dateModified
image:                                                       # required: hero, card and share image
  path: /assets/images/adam-kerrigan-eye-examination.webp
  width: 1600
  height: 1067
  alt: "What the image shows, plainly"                       # required: build fails without it
image_caption: "Optional caption under the hero"             # optional
faqs:                                                        # optional: rendered as <details> and FAQPage JSON-LD
  - question: "Natural-language question?"
    answer: "Answer in Markdown. Links as [text](/other-post/)."
published: true                                              # optional; false hides the post
---
```

Body in Markdown. Use `##` for question headings and `###` beneath them. Images only through the include (never Markdown `![]()`, which has no width/height or lazy loading):

```liquid
{% include figure.html src="/assets/images/name.webp" alt="Required description" caption="Optional caption" %}
{% include figure.html src="/assets/images/name.webp" alt="Required description" class="wide" %}
```

`width`/`height` default to 1600/1067; pass them if an image has other dimensions. Put each include on its own line with a blank line above and below.

After adding a post, build locally, check it, commit to `main` and push. GitHub Pages rebuilds in about a minute.
