# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based blog website for tobyhede.com, built using the Astro blog starter template. The project is a personal blog/portfolio site with support for Markdown/MDX content, RSS feeds, and responsive design.

## Development Commands

**Working Directory:** Always work from `/Users/tobyhede/psrc/tobyhede.com/site/`

| Command | Purpose |
|---------|---------|
| `npm install` | Install dependencies |
| `npm run dev` | Start development server at localhost:4321 |
| `npm run build` | Build production site to ./dist/ |
| `npm run preview` | Preview production build locally |
| `npm run astro` | Run Astro CLI commands |
| `npm run astro -- --help` | Get Astro CLI help |

## Architecture

### Content Management
- **Blog Posts**: Located in `src/content/blog/` as Markdown/MDX files
- **Content Schema**: Defined in `src/content.config.ts` with frontmatter validation using Zod
- **Content Collections**: Uses Astro's content collections API with `getCollection()` for type-safe content queries

### Site Structure
- **Pages**: File-based routing in `src/pages/`
  - `index.astro` - Homepage
  - `blog/index.astro` - Blog listing page
  - `blog/[...slug].astro` - Dynamic blog post pages
  - `rss.xml.js` - RSS feed generation
- **Layouts**: `src/layouts/BlogPost.astro` - Blog post template
- **Components**: Reusable Astro components in `src/components/`
- **Global Config**: Site title and description in `src/consts.ts`

### Key Features
- TypeScript with strict mode enabled
- MDX support for rich content
- RSS feed auto-generation
- Sitemap integration
- Responsive image handling with Astro's Image component
- Global CSS variables for theming

### Development Notes
- The site uses Astro 5.x with modern ESM modules
- Content schema validates: title, description, pubDate, updatedDate, heroImage
- Blog posts are automatically sorted by publication date (newest first)
- Static assets go in `public/` directory
- The site URL is currently set to 'https://example.com' in astro.config.mjs