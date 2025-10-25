# Home Page Redesign Design

**Date:** 2025-10-25
**Status:** Approved

## Overview

Redesign the home page to use a consistent hero-based layout (similar to the about page) with a static branding hero image and a simple list of the 6 most recent blog posts.

## Goals

- Unify the home page and about page under a consistent layout pattern
- Replace the grid-based post display with a clean, simple list format
- Use hero image as branding (not linked to most recent post)
- Show title, date, and description for each post (no images in list)

## Architecture

### Component Structure

**New unified layout component:**
- `src/layouts/HeroPageLayout.astro` - Shared layout for hero-based pages

**Modified pages:**
- `src/pages/index.astro` - Home page using new layout
- `src/pages/about.astro` - About page using new layout

**Existing layout:**
- `src/layouts/BlogPost.astro` - Keep for actual blog posts (different use case)

### Data Flow

```
┌─────────────────┐
│  index.astro    │──┐
└─────────────────┘  │
                     ├──► HeroPageLayout.astro ──► Header/Footer
┌─────────────────┐  │
│  about.astro    │──┘
└─────────────────┘
```

Both pages use HeroPageLayout with different props and slot content.

## Component Interface

### HeroPageLayout.astro Props

```typescript
interface Props {
  title: string;            // Required - page title
  description: string;      // Required - for display and SEO
  pubDate?: Date;          // Optional - show date (about page uses this)
  heroImage?: ImageMetadata; // Optional - hero branding image
  updatedDate?: Date;      // Optional - last updated date
}
```

### Usage Examples

**Home Page:**
```astro
<HeroPageLayout
  title={SITE_TITLE}
  description={SITE_DESCRIPTION}
  heroImage={HeroImage}
>
  <!-- Blog post list (6 most recent) -->
</HeroPageLayout>
```

**About Page:**
```astro
<HeroPageLayout
  title="Hello, I'm Toby!"
  description="About Toby"
  pubDate={new Date('August 23 2025')}
  heroImage={AboutHeroImage}
>
  <!-- About content -->
</HeroPageLayout>
```

## Layout Structure

### HeroPageLayout.astro Structure

```
┌──────────────────────────────────┐
│ <Header />                       │
├──────────────────────────────────┤
│ [Hero Image - Full Width]        │
│ (if heroImage provided)          │
├──────────────────────────────────┤
│                                  │
│  ┌────────────────────────┐     │
│  │  Centered Title Area   │     │
│  │  (720px max width)     │     │
│  │                        │     │
│  │  [Date] (if provided)  │     │
│  │  <Title>               │     │
│  │  ──────────────        │     │
│  └────────────────────────┘     │
│                                  │
│  ┌────────────────────────┐     │
│  │  Content Slot          │     │
│  │  (720px max width)     │     │
│  │                        │     │
│  │  <slot />              │     │
│  └────────────────────────┘     │
│                                  │
├──────────────────────────────────┤
│ <Footer />                       │
└──────────────────────────────────┘
```

## Home Page Blog Post List

### Display Format

Show 6 most recent posts in simple list format (reuse archive styles):

**Each post displays:**
- Title (h2, 1.25rem, linked)
- Publication date (0.875rem, gray)
- Description (0.9rem, gray)
- Bottom border separator
- Hover state: title color changes to accent

**No images in the list** - clean, text-focused presentation.

### Styling

Reuse existing archive list styles from `blog/index.astro`:
- `.archive-list` - vertical list container
- `.archive-item` - individual post wrapper
- `.archive-title` - post title
- `.archive-date` - publication date
- `.archive-description` - post description

### Data Query

```astro
const posts = (await getCollection('blog'))
  .sort((a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf())
  .slice(0, 6);  // Limit to 6 most recent
```

## Styling Details

### HeroPageLayout Styles

**Hero Image:**
- Full width display
- 12px border radius
- Box shadow (var(--box-shadow))
- Responsive scaling

**Title Section:**
- Centered alignment
- Date above title (gray, smaller font)
- Horizontal rule below title
- Padding for spacing

**Content Area:**
- Max width: 720px
- Centered with auto margins
- Responsive padding: calc(100% - 2em)
- Inherits prose styles from BlogPost layout

### Responsive Behavior

**Mobile (< 720px):**
- Content area: full width with padding
- Hero image: scales with viewport
- Post list: stacks vertically
- Font sizes: maintain readability

**Desktop:**
- Content area: fixed 720px width
- Hero image: full width with max constraints
- Post list: optimal line length for reading

## Page-Specific Behavior

### Home Page
- Hero image: branding/static
- No date in title section
- Title: Site title (SITE_TITLE)
- Content: 6 recent posts in simple list format

### About Page
- Hero image: personal branding
- Date: August 23, 2025 (shows above title)
- Title: "Hello, I'm Toby!"
- Content: personal bio and interests

## Migration Notes

### Deprecated Patterns
- Grid-based post display on home page
- Hero image as link to most recent post
- Image thumbnails in post lists

### Preserved Patterns
- BlogPost.astro layout for actual blog posts
- Blog archive page at /blog/ (unchanged)
- Individual post pages (unchanged)

## SEO Considerations

- Both pages provide title and description for meta tags
- Home page uses SITE_TITLE and SITE_DESCRIPTION
- About page has custom title and description
- Hero images include alt text (empty string for decorative images)

## Success Criteria

1. Home page displays hero image as static branding
2. 6 most recent posts show in simple list format (title, date, description)
3. Home page and about page share HeroPageLayout component
4. Responsive design works on mobile and desktop
5. No code duplication between home and about pages
6. Consistent styling with existing site design system
