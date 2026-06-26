# Cartagena Up — Development Rules

## Bilingual Rule (CRITICAL)
This site is bilingual: English at / and Spanish at /es/.

EVERY user-facing string in templates must use:
```
{{ if eq .Site.Language.Lang "es" }}
  Spanish
{{ else }}
  English
{{ end }}
```

For partials where `.` is place data (not page), use `site.Language.Lang` instead of `.Site.Language.Lang`:
```
{{ $lang := site.Language.Lang }}
{{ if eq $lang "es" }}Spanish{{ else }}English{{ end }}
```

NO hardcoded text in any single language is ever acceptable in a template file.
This includes: labels, button text, link text, placeholder text, badge text,
navigation text, footer text, CTA text.

## Data files
YAML data files store content in the original language (usually Spanish for
local content, or neutral English). Templates handle all translation of UI labels.
Never put UI labels in data files.

Bilingual content fields convention:
- `field` = Spanish (base, written by wife)
- `field_en` = English (written by Yatan)
- Templates select the right one and fall back to whatever exists

## Template locations (Hugo v0.146.0+)
- Partials: layouts/_partials/ (NOT layouts/partials/)
- Home page: layouts/home.html (NOT layouts/index.html)
- Base template: layouts/baseof.html
- Partial calls: `{{ partial "name.html" . }}`
  (NOT `{{ partial "_partials/name.html" . }}`)

## Hugo data access
Always use: `hugo.Data.places.work`
Never use: `.Site.Data.places.work` (deprecated)

## CSS
All colors via CSS custom properties in assets/css/main.css.
Never hardcode hex colors in template files.

## Images
- Place photos: static/images/places/work/[slug].jpg
- Always include `loading="lazy"` on place images
- Always commit image files before pushing — missing images will not appear on Cloudflare

## Freshness
The `VERIFIED: [date]` badge is the most important UI element.
It must always be visible and prominent on every place card. Never remove or hide it.

## Content workflow
1. Collect data in the field (WiFi speeds, photos, notes)
2. Write YAML in Claude chat
3. Claude Code creates the files
4. `git add -A && git commit && git push` — Cloudflare auto-deploys

## Adding a new work listing
Three files required:
1. `data/places/work/[slug].yaml` — all data
2. `content/en/work/[slug].md` — front matter only (title, slug, dataKey, layout: single-place)
3. `content/es/work/[slug].md` — same front matter

## Events
- Past events are filtered by template, not deleted from data files
- Ongoing exhibitions: use `end_date` to control visibility, reset `date` to today so they sort correctly
- `t0` field required on all events for time display
- `notes` field: Spanish; `notes_en` field: English translation
- `active: true` required for event to appear

## Hugo Version & Docs

This site runs Hugo v0.163.x (latest stable).
Hugo changes fast — always verify against current docs.

Before writing or modifying any Hugo template, configuration,
or build-related code, fetch and check the relevant page at:
https://gohugo.io/documentation/

Key pages to check for common tasks:
- Template lookup: https://gohugo.io/templates/lookup-order/
- Data templates: https://gohugo.io/templates/data/
- Multilingual: https://gohugo.io/content-management/multilingual/
- Configuration: https://gohugo.io/configuration/
- Sitemap: https://gohugo.io/templates/sitemap/

Never rely on memory for Hugo syntax — the template system
changed significantly in v0.146.0 and continues to evolve.
When in doubt, fetch the docs first.

## Multilingual sitemap behaviour (Hugo constraint)

With `defaultContentLanguageInSubdir = false`, Hugo generates:
- `/sitemap.xml` — sitemap index (references /en/ and /es/ sitemaps)
- `/en/sitemap.xml` — English pages sitemap (URLs are at root: /work/, /events/)
- `/es/sitemap.xml` — Spanish pages sitemap (URLs at /es/work/, etc.)

The `/en/sitemap.xml` reference in the index is a Hugo architectural
constraint, not a bug. Page URLs inside are correct (at root).
For Google Search Console submit `/sitemap.xml` — Google follows
the index, reads the correct root-level page URLs, and indexes correctly.

Do not attempt to change this with per-language baseURL settings —
that triggers "multihost mode" which outputs each language into
a separate directory (public/en/, public/es/) which is not what we want.

## Key bilingual strings reference
| Label | ES | EN |
|---|---|---|
| Verified badge | VERIFICADO | VERIFIED |
| Outlets | Enchufes | Outlets |
| Hours | Horario | Hours |
| Also here | También aquí | Also here |
| Open in Maps | Ver en Maps | Open in Maps |
| Something changed? | ¿Algo cambió? | Something changed? |
| Getting there | Cómo llegar | Getting there |
| Current conditions | Estado actual | Current conditions |
| Last visited | Última visita | Last visited |
| Crowd | Afluencia | Crowd |
| Cost | Costo | Cost |
| On until | Hasta el | On until |
| Confirmed | confirmado | confirmed |
| Free (price tag) | gratis | Free |
| Paid (price tag) | pago | Paid |
| No results | Sin resultados | No results |
| Search placeholder | Buscar evento o lugar… | Search events or venues… |
| All (filter) | Todos | All |
| Last updated | Última actualización | Last updated |
| By someone who lives here | Por alguien que vive aquí | By someone who lives here |
