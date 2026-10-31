# Kodok Marine CMS

Git-based content management system for the [Kodok Marine](https://kodok-marine.vercel.app) website.

Content is written in Markdown with YAML frontmatter. The website fetches content directly from this repository via the GitHub API — no build step required. Push a new file and the website picks it up within minutes (cached for 5 minutes at the edge).

---

## Repository Structure

```
content/
├── blog/       # Long-form articles (Reef Science, equipment guides, etc.)
├── stories/    # Short cards that appear on the homepage Stories section
└── corals/     # Coral database entries with care parameters
```

Each content type supports bilingual content (`lang: en` and `lang: id`). The website displays only the language matching the visitor's locale; if no translation exists it falls back to English.

### Naming Convention

| Content Type | Pattern            | Example                       |
| ------------ | ------------------ | ----------------------------- |
| Blog (EN)    | `topic-name.md`    | `bounce-mushroom-coral.md`    |
| Blog (ID)    | `topic-name-id.md` | `bounce-mushroom-coral-id.md` |
| Stories (EN) | `topic-name.md`    | `bounce-mushrooms.md`         |
| Stories (ID) | `topic-name-id.md` | `bounce-mushrooms-id.md`      |
| Corals (EN)  | `coral-name.md`    | `zoanthids.md`                |
| Corals (ID)  | `coral-name-id.md` | `zoanthids-id.md`             |

---

## ⚠️ YAML Frontmatter Rules

- **Always quote values that contain `:` or `!!`** — these are special YAML characters.  
  ✅ `title: "Bounce Mushroom: Everyone Loves those Balloons!!"  `  
  ❌ `title: Bounce Mushroom: Everyone Loves those Balloons!!`
- Dates must be in `YYYY-MM-DD` format.
- Arrays can use inline format `[item1, item2]` or block format (with `- item` lines).

---

## Content Types

### 1. Blog Posts (`content/blog/`)

Long-form articles. Each post gets its own page at `/blog/[slug]`.

**Frontmatter schema:**

```yaml
---
title: "Your Post Title" # Required. Quote if it contains : or !
date: 2026-10-31 # Required. YYYY-MM-DD
category: Reef Science # Required. Used for filtering on the blog page
tags: [coral, soft-coral, zoa] # Optional. Used for filtering
author: Izac Aqsha # Optional. Defaults to "Izac Aqsha"
thumbnail: /data/thumbnails/filename.jpeg # Optional. Path to image
lang: en # Required. "en" or "id"
slug: custom-slug # Optional. Auto-generated from title if omitted
---
Post body in Markdown goes here.
```

**Supported `category` values** (for consistent filtering):

- `Reef Science`
- `Equipment`
- `Water Chemistry`
- `Livestock`
- `DIY`
- `Beginner Guide`

**Example file:** `content/blog/zoanthids-flowers-of-the-sea.md`

```yaml
---
title: "Zoanthids, the flowers of the sea"
date: 2026-10-31
category: Reef Science
tags: [coral, soft-coral, zoanthids]
author: Izac Aqsha
thumbnail: /data/thumbnails/zoanthids.jpeg
lang: en
---
Zoanthids, or what people like to call "Zoa"...
```

---

### 2. Stories (`content/stories/`)

Short display cards that appear in the **Stories** section on the homepage. Each story is a teaser that links to an external URL (Instagram, YouTube) or an internal blog post.

**Frontmatter schema:**

```yaml
---
title: "Story Title" # Required. Quote if it contains : or !
description: "Short description." # Required. Shown as the card teaser text (~150 chars)
category: REEF SCIENCE # Required. Displayed as a badge. Use ALL CAPS.
categoryColor: info # Required. Bootstrap color: info | success | warning | primary | secondary
url: /blog/post-slug # Required. Internal path OR full external URL
thumbnail: /data/thumbnails/filename.jpeg # Required. Card image
featured: true # Optional. Featured stories appear larger / first
date: 2026-10-31 # Required. YYYY-MM-DD
lang: en # Required. "en" or "id"
---
Optional short body text (not displayed in the card, only if fetched directly).
```

**Supported `categoryColor` values:**

| Value       | Color        | Use for                          |
| ----------- | ------------ | -------------------------------- |
| `info`      | Cyan/teal    | Science, biology content         |
| `success`   | Green        | Events, community, positive news |
| `warning`   | Yellow/amber | Chemistry, parameters            |
| `primary`   | Blue         | Equipment, gear                  |
| `secondary` | Gray         | General, misc                    |
| `danger`    | Red          | Warnings, urgent                 |

**Example file:** `content/stories/bounce-mushrooms.md`

```yaml
---
title: "Bounce Mushroom Coral: Everyone Loves those Balloons!!"
description: Bounce mushroom corals are among the most eye-catching corals in the reef hobby.
category: REEF SCIENCE
categoryColor: info
url: /blog/bounce-mushroom-coral
thumbnail: /data/thumbnails/bounce-mushrooms.jpeg
featured: true
date: 2026-10-25
lang: en
---
```

---

### 3. Coral Database (`content/corals/`)

Structured coral care entries. Each entry gets its own page at `/corals/[slug]` with a parameters table, compatibility info, and a full care guide.

**Frontmatter schema:**

```yaml
---
title: "Common Name" # Required
scientificName: Genus species # Optional
slug: coral-name # Required. Used as the URL slug
lang: en # Required. "en" or "id"

# Classification
type: soft # Required. soft | lps | sps | nps
difficulty: beginner # Required. beginner | intermediate | advanced

# Media
thumbnail: /data/thumbnails/filename.jpeg # Optional
gallery: # Optional. List of additional image paths
  - /data/thumbnails/image1.jpeg
  - /data/thumbnails/image2.jpeg

# Water Parameters
waterParameters:
  temperature: "24-27°C"
  salinity: "1.024-1.026"
  pH: "8.1-8.4"
  alkalinity: "8-12 dKH"
  calcium: "400-450 ppm"
  magnesium: "1250-1350 ppm"
  nitrate: "< 10 ppm"
  phosphate: "< 0.1 ppm"

# Care requirements
lighting: low-medium # low | low-medium | medium | medium-high | high
lightingPAR: "50-150" # PAR range as a string
flow: low-medium # low | low-medium | medium | medium-high | high
placement: bottom-middle # bottom | bottom-middle | middle | middle-top | top | anywhere
growthRate: fast # slow | moderate | fast

# Behavior
toxicity: high # none | low | medium | high
aggressiveness: medium # low | medium | high

# Compatibility
compatibleWith:
  - "LPS corals"
  - "Reef-safe fish"
incompatibleWith:
  - "Aggressive LPS"

# Metadata
tags:
  - beginner-friendly
  - colorful
featured: true # Optional. Featured corals appear first in the database
date: 2026-10-31 # Required. YYYY-MM-DD
---
Full care guide in Markdown.
Use ## headings for sections like Care Overview, Lighting, Flow, Feeding, Propagation, etc.
```

**`type` values:**

| Value  | Meaning                                             |
| ------ | --------------------------------------------------- |
| `soft` | Soft corals (Zoanthids, Mushrooms, Leathers, GSP)   |
| `lps`  | Large Polyp Stony (Hammer, Torch, Frogspawn, Brain) |
| `sps`  | Small Polyp Stony (Acropora, Montipora, Stylophora) |
| `nps`  | Non-Photosynthetic (Dendrophyllia, Sun corals)      |

**Example file:** `content/corals/zoanthids.md`

```yaml
---
title: Zoanthids
scientificName: Zoantharia spp.
slug: zoanthids
lang: en
type: soft
difficulty: beginner
thumbnail: /data/thumbnails/zoanthids.jpeg
waterParameters:
  temperature: "24-27°C"
  alkalinity: "8-12 dKH"
  calcium: "400-450 ppm"
lighting: low-medium
lightingPAR: "50-150"
flow: low-medium
placement: bottom-middle
growthRate: fast
toxicity: high
aggressiveness: medium
compatibleWith: ["LPS corals", "Reef-safe fish"]
incompatibleWith: ["Aggressive LPS"]
tags: [beginner-friendly, colorful, fast-growing]
featured: true
date: 2026-10-31
---
Zoanthids are one of the most popular corals in the hobby...
```

---

## Adding New Content

### Adding a Blog Post

1. Create `content/blog/post-slug.md` (English)
2. Create `content/blog/post-slug-id.md` (Indonesian) — copy the EN file and translate
3. Add thumbnail image to the website's `dist/data/thumbnails/` folder (or use an external URL)
4. Commit and push both files

### Adding a Story Card

1. Create `content/stories/story-name.md` (English)
2. Create `content/stories/story-name-id.md` (Indonesian)
3. Set `featured: true` on the 3 most important stories per language — these appear prominently on the homepage
4. Commit and push

### Adding a Coral Database Entry

1. Create `content/corals/coral-name.md` (English)
2. Create `content/corals/coral-name-id.md` (Indonesian)
3. Fill in all care parameters in the frontmatter
4. Write the care guide body in Markdown
5. Commit and push

---

## Workflow

```
Edit .md file  →  git commit  →  git push  →  website updates (within 5 min)
```

The website API caches responses for 5 minutes (`s-maxage=300`) and serves stale for up to 30 minutes (`stale-while-revalidate=1800`). For immediate refresh on the live site, the Vercel cache needs to expire or be purged.

---

## Live URLs

| Content        | CMS Path                    | Live URL                                        |
| -------------- | --------------------------- | ----------------------------------------------- |
| Blog list      | —                           | `https://kodok-marine.vercel.app/blog`          |
| Blog post      | `content/blog/[slug].md`    | `https://kodok-marine.vercel.app/blog/[slug]`   |
| Stories        | `content/stories/[name].md` | Homepage stories section                        |
| Coral database | —                           | `https://kodok-marine.vercel.app/corals`        |
| Coral detail   | `content/corals/[slug].md`  | `https://kodok-marine.vercel.app/corals/[slug]` |

---

## Related Repositories

- **Website**: [IzacFE/Kodok_Marine_Web](https://github.com/IzacFE/Kodok_Marine_Web)
- **CMS**: [IzacFE/km-blog-cms](https://github.com/IzacFE/km-blog-cms) ← you are here
