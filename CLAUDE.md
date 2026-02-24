# Incotroceni.ro – Claude Code Context

Site-ul oficial al **Asociației Incotroceni** – o asociație civică din cartierul Cotroceni, București.
Conținut în română. Jekyll static site, hosted on GitHub Pages.

---

## Architecture

| Layer | Tool |
|---|---|
| Static site generator | Jekyll 3.9.2 (pinned by `github-pages` gem) |
| Theme | [`daviddarnes/alembic`](https://github.com/daviddarnes/alembic) via `jekyll-remote-theme` |
| Production hosting | GitHub Pages (auto-deploy on push to `master`) |
| PR preview hosting | Vercel (builds on every PR; skips production via `ignoreCommand` once set up) |
| CSS customization | `_includes/site-styles.html` (because `css_inline: true` in `_config.yml`) |

**Vercel uses `Gemfile.vercel` (Jekyll 4.x) instead of the main `Gemfile` (Jekyll 3.9.2)** because Jekyll 3.9.2 is incompatible with Ruby 3.3 (Logger API breaking change). GitHub Pages uses the main Gemfile, which is fine because GH Pages controls its own Ruby version.

---

## Collections

### `_evenimente/` — Event pages

- Files: `_evenimente/YYYY-MM-event-slug.md`
- URL: `/evenimente/YYYY-MM-event-slug/` (trailing slash is essential — see Bug History)
- Layout: `event`
- Front matter fields:
  ```yaml
  layout: event
  title: "Bazar de Cotroceni ~ Octombrie 2024"
  date: 14 October 2024          # or range: "14-15 June 2025"
  time: 10.00 – 22.00
  event_image: "../../assets/images/events-2024-10-bazar-1.jpg"
  facebook_link: https://www.facebook.com/events/XXXXX   # optional
  photo_gallery: https://...                              # optional
  ```

### `_proiecte/` — Project pages

- Files: `_proiecte/project-slug.md`
- Layout: `project`

---

## Supporting Subpages (harta, qr, afis)

Event sub-pages (printable map, QR codes, poster) are **root-level `.md` files** with an explicit `permalink:` that matches the collection URL structure.

**Do not put these in `_evenimente/`** — Jekyll would generate them under the collection path but with wrong relative asset paths.

Pattern:
```yaml
---
layout: page-hide-title
title: Harta Bazar de Cotroceni 2024
permalink: /evenimente/2024-10-bazar-de-cotroceni/harta
---
```

Naming convention for root files: `bazar-[luna]-[an]-harta.md`, `bazar-[luna]-[an]-qr.md`, `bazar-[luna]-[an]-afis.md`

Existing root subpages:
- `bazar-oct-2022-harta.md` / `bazar-oct-2022-qr.md` / `bazar-oct-2022-afis.md`
- `bazar-mai-2023-harta.md` / `bazar-mai-2023-qr.md`
- `bazar-oct-2023-harta.md` / `bazar-oct-2023-qr.md`
- `2024-bazar-oct-harta.md`
- `bazar-iunie-2025-harta.md`

---

## Image Naming Conventions

All images go in `assets/images/`.

### Event cover / hero images

```
events-YYYY-MM-[event-slug]-1.jpg      ← preferred (recent pattern)
events-YYYY-MM-[event-slug]-cover.jpg  ← older pattern, also accepted
```

### Event photo gallery images

```
events-YYYY-MM-[event-slug]-2.jpg
events-YYYY-MM-[event-slug]-3.jpg
...
events-YYYY-MM-[event-slug]-6.jpg
```

Referenced in the event `.md` file as:
```liquid
{% include figure.html image="../../assets/images/events-2024-10-bazar-2.jpg" position="center" %}
```

Note the `../../` prefix — this is required because event collection pages are 2 levels deep.

### QR code images

```
qr_code_harta_bazar_[slug]_[YYYY].png           ← QR for online map
qr_code_harta_bazar_print_[slug]_[YYYY].png     ← QR for printable map
```

Example: `qr_code_harta_bazar_print_oct_2023.jpg`

### Image sizing guidance

- Cover / hero images: **max 800×600px, ≤200KB** (JPEG quality ~80)
- Gallery photos: **max 1200×900px, ≤400KB**
- QR codes: **PNG, 500×500px** is plenty
- The 2024 bazaar images were accidentally added at 3–4MB each — avoid this

---

## Bug History

**Routing bug (introduced 29 Sep 2022, fixed Feb 2025):**
`permalink: /evenimente/:path` (no trailing slash) generated `.html` files instead of directory-based `index.html` files. The workaround was duplicating event pages in the project root with explicit `permalink:` front matter. Fixed by adding the trailing slash: `permalink: /evenimente/:path/`.

---

## Adding a New Event

Use the `/new-event` skill: it will collect all necessary details and create the boilerplate files.

Manual steps:
1. Create `_evenimente/YYYY-MM-event-slug.md`
2. Add images to `assets/images/` (see naming conventions above)
3. Create root subpages for harta/qr/afis if needed
4. Commit and push to a branch → Vercel will build a preview

---

## Known Issues / Future Work

- **Google Analytics**: Using UA-118125506-1 (Universal Analytics) which is deprecated. Should migrate to GA4.
- **Large images**: Some event images are 3–4MB. Should be compressed before committing.
- **No image compression pipeline**: Images are committed raw. Consider adding an automated step.
- **CSS customization**: Not yet applied (design improvement deferred).
