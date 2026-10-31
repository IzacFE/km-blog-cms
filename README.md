# Kodok Marine CMS

Git-based content management system for Kodok Marine website.

## Structure

```
content/
├── blog/          # Blog posts
└── stories/       # Featured stories
```

## Content Format

Each content file uses markdown with frontmatter:

```markdown
---
title: Post Title
date: 2026-10-31
category: Category Name
tags: [tag1, tag2]
featured: true
thumbnail: /path/to/image.jpg
lang: en
---

Content goes here in markdown format.
```

## Usage

1. Add new content files to appropriate directories
2. Commit and push to this repository
3. The main website will automatically fetch and display the content
