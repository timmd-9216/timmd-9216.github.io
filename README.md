# timmd-9216.github.io

Course site for **Seminario de Ingeniería Industrial II (9216)** — Taller de Modelos Matemáticos y de Datos, FIUBA.

Live at: https://timmd-9216.github.io

---

## Structure

```
├── index.html                        # Home page
├── translations.json                 # Home page strings (es + en)
├── projects/
│   ├── translations.json             # All project detail page strings (es + en)
│   └── trust.html                    # Project detail page (reference template)
├── blog/
│   ├── translations.json             # All blog post strings (es + en)
│   ├── index.html                    # Blog index (list of posts)
│   └── 2025-mediaparty.html          # Example blog post
└── assets/
    ├── docs/
    │   └── Syllabus.pdf
    └── images/
        ├── projects/                 # Project-specific images
        └── blog/                     # Blog post images
```

---

## How to run locally

```bash
python -m http.server 8000
```

Then open http://localhost:8000 in your browser.

> You must use a local server (not open the file directly) because each page fetches its `translations.json` via HTTP.

---

## How translations work

All visible text lives in `translations.json` files. Each has two top-level keys — `"es"` and `"en"` — with matching keys side by side.

- Home page text → `translations.json`
- Project detail page text → `projects/translations.json`
- Blog posts and blog index text → `blog/translations.json`

To update text: open the relevant `translations.json`, find the key, edit both `"es"` and `"en"` values, save and refresh.

Some values contain HTML (links, line breaks) — these use `data-i18n-html` in the HTML file. Keep the HTML valid when editing them.

**Language detection:** auto-detects browser language on first visit (English → `en`, anything else → `es`). The user can toggle via the nav link (`?lang=es` / `?lang=en`), saved to `localStorage`.

---

## How to add a new text element

1. Add the key to the relevant `translations.json` under both `"es"` and `"en"`:
   ```json
   "my_key": "Texto en español"
   "my_key": "Text in English"
   ```
2. Add the attribute to the HTML element:
   - Plain text: `data-i18n="my_key"`
   - HTML content (links, `<br/>`, etc.): `data-i18n-html="my_key"`

---

## How to add a new project detail page

1. Copy `projects/trust.html` → `projects/myproject.html`
2. Replace all `trust_` key references with `myproject_` (or whatever prefix you choose)
3. Add the corresponding keys to `projects/translations.json` under both `"es"` and `"en"`, following the same prefix pattern
4. Add a "Read more" link in the `#projects` section of `index.html` pointing to `projects/myproject.html`
5. Put project images in `assets/images/projects/myproject/`

---

## How to add a new blog post

1. Copy `blog/2025-mediaparty.html` → `blog/YYYY-slug.html`
2. Replace all `mediaparty_` key references with your post's prefix
3. Add the corresponding keys to `blog/translations.json` under both `"es"` and `"en"`
4. Add a new `.post-card` entry in `blog/index.html` pointing to your new file
5. Put post images in `assets/images/blog/YYYY-slug/`

---

## How to add a new section to the home page

1. Add the HTML block to `index.html` with `data-i18n` or `data-i18n-html` attributes on each text element
2. Add the corresponding keys to both `"es"` and `"en"` in `translations.json`
3. Non-text content (images, fixed URLs, course code) can be hardcoded directly in the HTML

---

## Deploying

Static GitHub Pages site — push to `main` and the site updates automatically. No build step required.
