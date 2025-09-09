# Tara Druffel (Astro theme)

A retro‑inspired, accessible, and type‑safe personal site theme built with Astro, TypeScript, and Tailwind CSS. It features Astro Content Collections for blogs and projects, a TOML‑driven site configuration, dark/light theme, and responsive, SEO‑friendly pages.

Demo: https://zaggonaut.dev


## Table of contents
- Tech stack and concepts
- Project structure
- Getting started
- Configuration (content/configuration.toml)
- Content collections
  - Adding a blog post
  - Adding a project
- Theming and customization
- Useful scripts
- Deployment
- FAQ and tips
- License


## Tech stack and concepts
- Astro 4 + TypeScript
- Tailwind CSS
- Astro Content Collections (typed frontmatter)
- TOML configuration loaded via astro:content custom collection
- Accessible, responsive components


## Project structure
At a glance:

- astro.config.mjs – Astro configuration
- src/ – Components, pages, layouts, utilities, and content collection schemas
  - src/content.config.ts – Content Collections definitions for blogs, projects, and configuration
  - src/pages/ – Route files (e.g., index.astro)
  - src/components/ – UI components (Header, Prose, ThemeToggle, Hero, etc.)
- content/ – Your editable content
  - content/configuration.toml – Site metadata, hero, navigation, labels
  - content/blogs/ – Markdown files for blog posts
  - content/projects/ – Markdown files for projects
- public/ – Static assets served as‑is
- images/ – Project images used in docs/README
- package.json – Project scripts


## Getting started
Prerequisites:
- Node 18+ recommended
- pnpm (this project is configured and tested with pnpm)

Install dependencies:
- pnpm install

Run the dev server:
- pnpm dev

Build for production:
- pnpm build

Preview the production build locally:
- pnpm preview


## Configuration (content/configuration.toml)
Global site settings live in content/configuration.toml and are loaded through the configuration collection defined in src/content.config.ts. Keys are namespaced with an underscore ("_.") due to type inference requirements.

Important sections:
- _.site.baseUrl – The canonical base URL of your site. Used for SEO meta and canonical links.
- _.globalMeta – Default metadata (title, description, longDescription, cardImage, keywords).
- _.blogMeta – Metadata for the /blog index page.
- _.projectMeta – Metadata for the /projects index page.
- _.notFoundMeta – Metadata for the 404 page.
- _.hero – Home page hero content (title, subtitle, image, ctaText, ctaUrl).
- _.personal – Profile links (name, GitHub, Twitter, LinkedIn).
- _.texts – UI copy (section headings and empty‑state strings).
- _.menu – Navigation paths (home, projects, blog).

After editing configuration.toml, restart the dev server if types or imported values change.


## Content collections
Content Collections enforce typed frontmatter for your Markdown content. Definitions live in src/content.config.ts.

Collections:
- blog – content/blogs/**/*.md
- project – content/projects/**/*.md
- configuration – content/configuration.toml

Shared conventions:
- slug is optional. If omitted, it is derived from the title (lowercase, hyphenated, non‑alphanumeric removed).
- timestamp must be a valid date (ISO 8601 is recommended). It is used for sorting and display.
- featured: true highlights items on the home page featured sections.


### Adding a blog post
1) Create a new Markdown file under content/blogs/, e.g. content/blogs/my-first-post.md
2) Provide the required frontmatter fields as defined by the blog collection schema:

---
title: My First Post
description: A quick introduction to the blog.
# Optional fields
slug: my-first-post
longDescription: A longer summary for social/SEO.
cardImage: https://example.com/og-card.png
tags: ["astro", "tutorial"]
readTime: 5
featured: false
timestamp: 2025-09-01T12:00:00Z
---

Markdown content starts here. You can use standard Markdown syntax.

3) Start or refresh the dev server (pnpm dev). Your post will be available at /blog/{slug}.

Notes:
- If slug is omitted, it is generated from title.
- Use a valid timestamp so sorting on index pages works as expected.
- cardImage should be an absolute URL if you want rich link previews.


### Adding a project
1) Create a new Markdown file under content/projects/, e.g. content/projects/my-app.md
2) Provide the required frontmatter fields as defined by the project collection schema:

---
title: My App
description: A small app that does X.
# Optional fields
slug: my-app
longDescription: A longer summary for social/SEO.
cardImage: https://example.com/og-card.png
tags: ["typescript", "astro"]
githubUrl: https://github.com/you/my-app
liveDemoUrl: https://my-app.example.com
featured: true
timestamp: 2025-08-15T10:00:00Z
---

Write details about your project here: features, stack, screenshots, links, etc.

3) Start or refresh the dev server (pnpm dev). Your project will be available at /projects/{slug}.

Tips:
- Set featured: true to show up on the home page’s Featured Projects section.
- Provide githubUrl and liveDemoUrl when available; they’re used by templates/components.


## Theming and customization
- Colors: Tailwind + CSS variables. Look in src/styles/global.css for theme variables such as:
  - --color-zag-dark, --color-zag-light, and their muted/accent variants.
- Components you might customize:
  - src/components/Header.astro – site header/navigation
  - src/components/ThemeToggle.astro – light/dark mode switch
  - src/components/Prose.astro – article typography
  - src/components/home/Hero.astro – landing hero section
  - src/components/home/FeaturedProjects.astro – projects list on home
  - src/components/home/FeaturedArticles.astro – blogs list on home
- Pages live in src/pages. The home page is src/pages/index.astro and pulls metadata from the configuration collection.


## Useful scripts
Defined in package.json:
- pnpm dev – Start the dev server
- pnpm build – Build the site to dist/
- pnpm preview – Preview the production build locally


## Deployment
- Static hosting is recommended (the site builds to dist/). Any static host that supports Astro output works (e.g., Netlify, Vercel, GitHub Pages). Configure the site base URL (.site.baseUrl) appropriately for correct canonical URLs and social meta.


## FAQ and tips
- Draft posts: You can keep drafts locally by not committing them or by placing them in content/blogs/ with future timestamps; add your own draft flag in the schema if needed.
- Images: Store public images under public/ for direct serving, or reference absolute URLs in frontmatter for cardImage.
- Navigation: Update _.menu in content/configuration.toml to change header links.
- Accessibility: Components aim for good defaults. Validate with Lighthouse/axe as you customize.


## License
This project is released under the MIT License. See LICENSE.md for details.
