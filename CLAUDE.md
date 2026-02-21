# Project constraints

Plain HTML, CSS, and vanilla JS only. No frameworks, libraries, build tools, package managers, transpilers, preprocessors, or bundlers. Must run correctly on GitHub Pages (static hosting, no server-side code).

---

# Architecture

## File structure

```
├── index.html                  # Home page template
├── translations.json           # Home page strings (es + en)
├── projects/
│   ├── index.html              # Projects listing page
│   ├── translations.json       # All project page strings (es + en)
│   └── trust.html              # Project detail page (reference template)
├── blog/
│   ├── index.html              # Blog listing page
│   ├── translations.json       # All blog post strings (es + en)
│   └── 2025-mediaparty.html    # Blog post (reference template)
└── assets/
    ├── docs/Syllabus.pdf
    └── images/
        ├── projects/           # Project-specific images
        └── blog/               # Blog post images
```

## i18n system

All visible text lives in `translations.json` files — never hardcoded in HTML. Each file has two top-level keys `"es"` and `"en"` with identical key sets.

- `data-i18n="key"` → sets `textContent` (use for plain text)
- `data-i18n-html="key"` → sets `innerHTML` (use when the value contains links or `<br/>`)

Language is resolved in this order: `?lang=` URL param → `localStorage` → browser language → default `es`.

One `translations.json` per section type:
- Home page strings → `translations.json`
- Project pages → `projects/translations.json`
- Blog posts → `blog/translations.json`

Keys are namespaced by slug: `trust_title`, `trust_lead`, `mediaparty_body_1`, etc.

---

# How to add a new project detail page

1. Copy `projects/trust.html` → `projects/myproject.html`
2. Replace all `trust_` key references in the HTML with `myproject_`
3. Add the corresponding keys to `projects/translations.json` under both `"es"` and `"en"`:
   ```json
   "myproject_page_title": "...",
   "myproject_lead": "...",
   "myproject_body_1": "...",
   "myproject_card_title": "...",
   "myproject_card_desc": "..."
   ```
4. Add a card to `projects/index.html` and uncomment its link
5. Put images in `assets/images/projects/myproject/` and reference them directly in the HTML (image paths don't need translation)

# How to add a new blog post

1. Copy `blog/2025-mediaparty.html` → `blog/YYYY-slug.html`
2. Replace all `mediaparty_` key references in the HTML with your post's prefix
3. Add the corresponding keys to `blog/translations.json` under both `"es"` and `"en"`:
   ```json
   "mypost_title": "...",
   "mypost_date": "...",
   "mypost_lead": "...",
   "mypost_body_1": "..."
   ```
4. Add a `.post-card` entry in `blog/index.html` pointing to the new file
5. Put images in `assets/images/blog/YYYY-slug/`

# How to add a new section to the home page

1. Add the HTML block to `index.html` with `data-i18n` or `data-i18n-html` on each text element
2. Add the keys to both `"es"` and `"en"` in `translations.json`
3. Hardcode anything that doesn't need translation (images, fixed URLs, course code)

# Style conventions

- Copy the `<style>` block from any existing page — all pages share the same CSS inline
- Use existing classes: `.card`, `.hero`, `.lead`, `.muted`, `.link`, `.container`
- The i18n `<script>` is identical across all pages except the `fetch()` path:
  - Home: `fetch('translations.json')`
  - Projects: `fetch('../projects/translations.json')`
  - Blog: `fetch('../blog/translations.json')`
- Never add external CSS or JS files
