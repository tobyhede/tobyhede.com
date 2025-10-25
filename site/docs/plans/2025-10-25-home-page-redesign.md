# Home Page Redesign Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Redesign the home page to use a unified hero-based layout with static branding and a clean list of the 6 most recent blog posts.

**Architecture:** Create a reusable HeroPageLayout.astro component that both home and about pages use. Extract and reuse the simple list styling from the blog archive page. Home page shows hero image + title + 6 recent posts in clean list format (title, date, description).

**Tech Stack:** Astro 5.x, TypeScript, Astro content collections

---

## Task 1: Create HeroPageLayout Component

**Files:**
- Create: `src/layouts/HeroPageLayout.astro`

**Step 1: Create the new layout file with structure**

Create `src/layouts/HeroPageLayout.astro`:

```astro
---
import { Image } from 'astro:assets';
import type { ImageMetadata } from 'astro';
import BaseHead from '../components/BaseHead.astro';
import Footer from '../components/Footer.astro';
import FormattedDate from '../components/FormattedDate.astro';
import Header from '../components/Header.astro';

interface Props {
  title: string;
  description: string;
  pubDate?: Date;
  heroImage?: ImageMetadata;
  updatedDate?: Date;
}

const { title, description, pubDate, updatedDate, heroImage } = Astro.props;
---

<html lang="en">
  <head>
    <BaseHead title={title} description={description} />
    <style>
      main {
        width: calc(100% - 2em);
        max-width: 100%;
        margin: 0;
      }
      .hero-image {
        width: 100%;
      }
      .hero-image img {
        display: block;
        margin: 0 auto;
        border-radius: 12px;
        box-shadow: var(--box-shadow);
      }
      .prose {
        width: 720px;
        max-width: calc(100% - 2em);
        margin: auto;
        padding: 1em;
        color: rgb(var(--gray-dark));
      }
      .title {
        margin-bottom: 1em;
        padding: 1em 0;
        text-align: center;
        line-height: 1;
      }
      .title h1 {
        margin: 0 0 0.5em 0;
      }
      .date {
        margin-bottom: 0.5em;
        color: rgb(var(--gray));
      }
      .last-updated-on {
        font-style: italic;
      }
    </style>
  </head>

  <body>
    <Header />
    <main>
      <article>
        <div class="hero-image">
          {heroImage && <Image width={1020} height={510} src={heroImage} alt="" />}
        </div>
        <div class="prose">
          <div class="title">
            {pubDate && (
              <div class="date">
                <FormattedDate date={pubDate} />
                {
                  updatedDate && (
                    <div class="last-updated-on">
                      Last updated on <FormattedDate date={updatedDate} />
                    </div>
                  )
                }
              </div>
            )}
            <h1>{title}</h1>
            <hr />
          </div>
          <slot />
        </div>
      </article>
    </main>
    <Footer />
  </body>
</html>
```

**Step 2: Verify the file was created**

Run: `ls -la src/layouts/HeroPageLayout.astro`
Expected: File exists

**Step 3: Commit**

```bash
git add src/layouts/HeroPageLayout.astro
git commit -m "feat: create HeroPageLayout component

Add unified layout for hero-based pages (home and about).
Supports optional hero image, title, date, and slot content."
```

---

## Task 2: Update Home Page to Use HeroPageLayout

**Files:**
- Modify: `src/pages/index.astro` (replace entire file)

**Step 1: Backup current home page structure**

Run: `cp src/pages/index.astro src/pages/index.astro.backup`
Expected: Backup created

**Step 2: Replace home page with new implementation**

Replace the entire content of `src/pages/index.astro`:

```astro
---
import { getCollection } from 'astro:content';
import FormattedDate from '../components/FormattedDate.astro';
import HeroPageLayout from '../layouts/HeroPageLayout.astro';
import { SITE_DESCRIPTION, SITE_TITLE } from '../consts';

const posts = (await getCollection('blog'))
  .sort((a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf())
  .slice(0, 6);
---

<HeroPageLayout
  title={SITE_TITLE}
  description={SITE_DESCRIPTION}
>
  <style>
    .archive-list {
      list-style-type: none;
      margin: 0;
      padding: 0;
    }
    .archive-item {
      margin-bottom: 1.5rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid rgb(var(--gray-light));
    }
    .archive-item:last-child {
      border-bottom: none;
    }
    .archive-item a {
      text-decoration: none;
      color: rgb(var(--black));
      display: block;
      transition: color 0.2s ease;
    }
    .archive-item a:hover {
      color: rgb(var(--accent));
    }
    .archive-title {
      margin: 0 0 0.5rem 0;
      font-size: 1.25rem;
      line-height: 1.3;
    }
    .archive-date {
      margin: 0;
      color: rgb(var(--gray));
      font-size: 0.875rem;
    }
    .archive-description {
      margin: 0.5rem 0 0 0;
      color: rgb(var(--gray-dark));
      font-size: 0.9rem;
      line-height: 1.4;
    }
  </style>

  <ul class="archive-list">
    {
      posts.map((post) => (
        <li class="archive-item">
          <a href={`/blog/${post.id}/`}>
            <h2 class="archive-title">{post.data.title}</h2>
            <p class="archive-date">
              <FormattedDate date={post.data.pubDate} />
            </p>
            <p class="archive-description">{post.data.description}</p>
          </a>
        </li>
      ))
    }
  </ul>
</HeroPageLayout>
```

**Step 3: Verify the changes**

Run: `git diff src/pages/index.astro`
Expected: Shows transition from grid layout to HeroPageLayout with archive list

**Step 4: Build to verify no errors**

Run: `npm run build`
Expected: Build completes successfully with no errors

**Step 5: Commit**

```bash
git add src/pages/index.astro
git commit -m "refactor: update home page to use HeroPageLayout

Replace grid-based layout with unified HeroPageLayout component.
Display 6 most recent posts in simple list format (title, date, description)."
```

---

## Task 3: Update About Page to Use HeroPageLayout

**Files:**
- Modify: `src/pages/about.astro` (replace entire file)

**Step 1: Replace about page with new implementation**

Replace the entire content of `src/pages/about.astro`:

```astro
---
import AboutHeroImage from '../assets/pondering-hero.png';
import HeroPageLayout from '../layouts/HeroPageLayout.astro';
---

<HeroPageLayout
  title="Hello, I'm Toby!"
  description="Hello, I'm Toby!"
  pubDate={new Date('August 23 2025')}
  heroImage={AboutHeroImage}
>
  <p>Items of interest, in no particular order:</p>
  <ul>
    <li>vp engineering@<a href="https://cipherstash.com">cipherstash.com</a>.</li>
    <li>rust engineer.</li>
    <li>software architect.</li>
    <li>enthusiastic drummer and sporadic musician.</li>
    <li>lifts heavy things and puts them down again.</li>
    <li>often hilarious (citation needed).</li>
  </ul>
</HeroPageLayout>
```

**Step 2: Verify the changes**

Run: `git diff src/pages/about.astro`
Expected: Shows transition from BlogPost layout to HeroPageLayout

**Step 3: Build to verify no errors**

Run: `npm run build`
Expected: Build completes successfully with no errors

**Step 4: Commit**

```bash
git add src/pages/about.astro
git commit -m "refactor: update about page to use HeroPageLayout

Migrate from BlogPost.astro to unified HeroPageLayout component."
```

---

## Task 4: Add Hero Image to Home Page (Optional - If Desired)

**Note:** The design discussion didn't specify a hero image for the home page. If you want to add one:

**Files:**
- Modify: `src/pages/index.astro:7-8` (add heroImage import and prop)

**Step 1: Choose or add a hero image**

Option A: Reuse the about page hero image
Option B: Add a new branding image to `src/assets/`

**Step 2: Import and pass hero image (if using Option A)**

In `src/pages/index.astro`, add after line 1:

```astro
import HeroImage from '../assets/pondering-hero.png';
```

And modify the HeroPageLayout call to include:

```astro
<HeroPageLayout
  title={SITE_TITLE}
  description={SITE_DESCRIPTION}
  heroImage={HeroImage}
>
```

**Step 3: Build to verify**

Run: `npm run build`
Expected: Build completes successfully

**Step 4: Commit (if changes made)**

```bash
git add src/pages/index.astro
git commit -m "feat: add hero image to home page"
```

---

## Task 5: Visual Verification and Testing

**Step 1: Start development server**

Run: `npm run dev`
Expected: Server starts on localhost:4321

**Step 2: Verify home page**

Visit: `http://localhost:4321/`

Check:
- [ ] Page title displays correctly (site title from SITE_TITLE)
- [ ] Horizontal rule appears below title
- [ ] Exactly 6 blog posts are listed
- [ ] Each post shows: title (linked), date, description
- [ ] No images in the blog post list
- [ ] Hover effect works on post titles (color changes to accent)
- [ ] Layout is responsive (test mobile width)
- [ ] Hero image displays (if added in Task 4)

**Step 3: Verify about page**

Visit: `http://localhost:4321/about/`

Check:
- [ ] Hero image displays at top
- [ ] Date appears above title
- [ ] Title is "Hello, I'm Toby!"
- [ ] Horizontal rule appears below title
- [ ] Bio content displays correctly
- [ ] Layout matches home page structure

**Step 4: Verify blog archive page (unchanged)**

Visit: `http://localhost:4321/blog/`

Check:
- [ ] Archive page is unchanged
- [ ] Shows all blog posts (not just 6)
- [ ] Uses same list styling as new home page

**Step 5: Stop development server**

Press: `Ctrl+C`

---

## Task 6: Production Build Verification

**Step 1: Clean previous build**

Run: `rm -rf dist`
Expected: dist directory removed

**Step 2: Run production build**

Run: `npm run build`
Expected: Build completes successfully with output showing:
- 8 pages built
- Optimized images generated
- Sitemap created
- No errors or warnings

**Step 3: Preview production build**

Run: `npm run preview`
Expected: Preview server starts

**Step 4: Spot check production build**

Visit: `http://localhost:4321/` (or the port shown)

Check:
- [ ] Home page renders correctly
- [ ] Links work
- [ ] Images load
- [ ] About page renders correctly

**Step 5: Stop preview server**

Press: `Ctrl+C`

---

## Task 7: Cleanup and Final Commit

**Step 1: Remove backup file (if created)**

Run: `rm -f src/pages/index.astro.backup`
Expected: Backup removed

**Step 2: Verify git status**

Run: `git status`
Expected: Working tree is clean (all changes committed)

**Step 3: Review commit history**

Run: `git log --oneline -5`
Expected: Shows the commits from this implementation

---

## Testing Checklist

**Functional Requirements:**
- [x] Home page uses HeroPageLayout component
- [x] About page uses HeroPageLayout component
- [x] Home page shows exactly 6 most recent posts
- [x] Posts display in simple list format (title, date, description)
- [x] No images in post list
- [x] Hero image is static (not linked to most recent post)
- [x] Both pages have consistent layout structure

**Visual Requirements:**
- [x] Home page title displays
- [x] Horizontal rule separator appears
- [x] Post list styling matches archive page
- [x] Hover effects work
- [x] Responsive on mobile and desktop
- [x] About page shows date above title
- [x] Hero images display correctly

**Build Requirements:**
- [x] Production build succeeds
- [x] No TypeScript errors
- [x] No console errors in browser
- [x] All pages generate correctly

---

## Rollback Plan

If issues arise, rollback steps:

```bash
# Restore original files
git revert HEAD~3..HEAD

# Or reset to before changes
git reset --hard <commit-before-changes>

# Rebuild
npm run build
```

---

## Notes for Engineer

**DRY (Don't Repeat Yourself):**
- HeroPageLayout eliminates duplication between home and about pages
- Archive list styles are copied to home page (acceptable since scoped to that page)

**YAGNI (You Aren't Gonna Need It):**
- Only 6 posts on home page (not configurable)
- No fancy transitions or animations
- No filtering or sorting UI

**Testing:**
- This is a UI refactoring, so manual visual testing is primary
- Build verification ensures no TypeScript/integration errors
- Test both desktop and mobile viewports

**Commits:**
- Small, focused commits per component/page
- Conventional commit format (feat/refactor)
- Descriptive commit messages

**Working Directory:**
- All commands assume you're in `/Users/tobyhede/psrc/tobyhede.com/site/.worktrees/home-page-redesign/site/`
