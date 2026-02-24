# Skill: new-event

Creates all boilerplate files for a new event page on incotroceni.ro.

## What this skill does

1. Collects event details from the user
2. Creates the main event collection file in `_evenimente/`
3. Creates supporting subpages (harta, qr, afis) in the project root
4. Outputs a checklist of required images with exact filenames and resize specs

---

## Step 1 — Collect event details

Ask the user for the following. All fields marked * are required.

| Field | Example |
|---|---|
| Event title* | `Bazar de Cotroceni ~ Octombrie 2025` |
| Event slug* | `2025-10-bazar-de-cotroceni` (used in URL and filenames) |
| Date* | `11 October 2025` or `11-12 October 2025` for multi-day |
| Time* | `10.00 – 22.00` |
| Facebook event URL | `https://www.facebook.com/events/XXXXX` |
| Photo gallery URL | (leave blank if none yet) |
| Has printable map? (harta) | yes / no |
| Has QR codes page? | yes / no |
| Has poster page? (afis) | yes / no |
| Number of event photos | 1–6 (cover + gallery images) |

The **slug** must follow the pattern `YYYY-MM-event-name` (e.g. `2025-10-bazar-de-cotroceni`).
The **image prefix** is derived from the slug: `events-YYYY-MM-[event-name]` (e.g. `events-2025-10-bazar`).

---

## Step 2 — Create the main event file

Create `_evenimente/{slug}.md`:

```markdown
---
layout: event
title: {title}
date: {date}
time: {time}
event_image: "../../assets/images/{image_prefix}-1.jpg"
facebook_link: {facebook_url}
---

{SHORT INTRO PARAGRAPH — 2-3 sentences describing the event. Ask user to provide or write a placeholder.}

**Harta online a evenimentului** poate fi consultată [aici]({google_maps_link}), iar cea printabilă, [aici](https://incotroceni.ro/evenimente/{slug}/harta).

{EVENT CONTENT — leave placeholder if user hasn't provided it yet:}

<!-- TODO: Add event description, points of interest, partners -->

Evenimentul Bazar de Cotroceni este organizat de Asociația Incotroceni și co-creat alături de comunitatea cartierului - locuitori și business-uri locale.
```

If the user hasn't provided Google Maps link or event description, add `<!-- TODO -->` placeholders and note them for the user.

Omit the harta line if user said no printable map.

---

## Step 3 — Create supporting subpages

### Harta page (if requested)

Create `bazar-{month}-{year}-harta.md` in the project root:

```markdown
---
layout: page-hide-title
title: Harta {title}
permalink: /evenimente/{slug}/harta
---

<iframe src="{google_drive_preview_url}" frameborder="0" marginheight="0" marginwidth="0" style="position: absolute;top: 0;left: 0;bottom: 0;right: 0;width: 100%;height: 100%;"  allow="autoplay"></iframe>
```

Note: the Google Drive preview URL has the format `https://drive.google.com/file/d/{FILE_ID}/preview`. Ask user to upload the printable map PDF to Google Drive and share the file ID, or add a `<!-- TODO -->` placeholder.

### QR codes page (if requested)

Create `bazar-{month}-{year}-qr.md` in the project root:

```markdown
---
layout: page-hide-title
title: QR Codes {title}
permalink: /evenimente/{slug}/qr
---

# QR Code pentru harta online

{% include figure.html image="../../assets/images/qr_code_harta_bazar_{month_en}_{year}.png" position="center" %}

# QR Code pentru harta printabila

{% include figure.html image="../../assets/images/qr_code_harta_bazar_print_{month_en}_{year}.png" position="center" %}
```

Where `{month_en}` is the English month abbreviation in lowercase (e.g. `oct`, `mai`, `iun`).

### Afis / poster page (if requested)

Create `bazar-{month}-{year}-afis.md` in the project root:

```markdown
---
layout: page-hide-title
title: Afis {title}
permalink: /evenimente/{slug}/afis
---

<iframe src="{google_drive_preview_url}" frameborder="0" marginheight="0" marginwidth="0" style="position: absolute;top: 0;left: 0;bottom: 0;right: 0;width: 100%;height: 100%;"  allow="autoplay"></iframe>
```

---

## Step 4 — Output image checklist

After creating the files, output this checklist for the user so they know exactly what images to prepare and where to put them.

---

### Image checklist for {title}

All images go in `assets/images/`.

#### Event photos (required)

| Filename | Role | Target size | Max file size |
|---|---|---|---|
| `{image_prefix}-1.jpg` | Cover / hero (used in event header) | 1200×800px | 200KB |
| `{image_prefix}-2.jpg` | Gallery photo | 1200×900px | 400KB |
| `{image_prefix}-3.jpg` | Gallery photo | 1200×900px | 400KB |
| `{image_prefix}-4.jpg` | Gallery photo | 1200×900px | 400KB |
| `{image_prefix}-5.jpg` | Gallery photo | 1200×900px | 400KB |
| `{image_prefix}-6.jpg` | Gallery photo | 1200×900px | 400KB |

(Only include rows for the number of photos the user said they'll have.)

#### QR codes (if qr page requested)

| Filename | Role | Format | Size |
|---|---|---|---|
| `qr_code_harta_bazar_{month_en}_{year}.png` | QR for online Google Maps link | PNG | 500×500px |
| `qr_code_harta_bazar_print_{month_en}_{year}.png` | QR for printable PDF map link | PNG | 500×500px |

**How to generate QR codes:**
- Generate at [qr-code-generator.com](https://www.qr-code-generator.com/) or similar
- Use PNG format, white background, 500×500px minimum
- Test each QR before committing

#### Image resize tips

Use any of these tools to resize and compress before committing:
- **macOS/Windows**: [Squoosh.app](https://squoosh.app) (free, browser-based)
- **CLI**: `ffmpeg -i input.jpg -vf scale=1200:-1 -q:v 3 output.jpg`
- **ImageMagick**: `magick input.jpg -resize 1200x900 -quality 80 output.jpg`

**Do not commit images over 400KB** — they slow down GitHub Pages and the site has no CDN.

---

## Step 5 — Summary

After completing all steps, show the user:

1. List of files created
2. The image checklist (from Step 4)
3. Next steps:
   - Add images to `assets/images/`
   - Fill in any `<!-- TODO -->` placeholders in the event file
   - Commit to a new branch (e.g. `feature/event-{slug}`)
   - Open a PR — Vercel will build a preview automatically
