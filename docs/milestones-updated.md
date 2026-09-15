# CENTAUR Website Milestones

## Milestone 1 — Site foundation

**Status: completed**

- Hugo site with Blowfish submodule established.
- Greek and English content trees created.
- Language-specific menus and configuration established.
- GitHub Pages deployment workflow established.
- Project overrides kept outside `themes/blowfish/`.

## Milestone 2 — People section

**Status: completed for current stage**

- Member categories established: Faculty, Special Teaching Staff, PhD Students, Graduate Students, Alumni, Visiting Researchers.
- Greek navigation label set to `Μέλη`.
- Two-column People cards implemented with fixed 112×112 photos.
- Placeholder-image support added.
- Alphabetical ordering implemented per language.
- Optional external links supported.
- Category index added at the top of the People page.
- Interests remain free text rather than linked research tags.

## Milestone 3 — Research areas and project associations

**Status: completed for current architecture**

- Research areas moved to canonical Markdown content under `content/<language>/areas/`.
- Main Research page derives its list automatically.
- English and Greek area files use matching filenames as stable identifiers.
- Current Research Projects pages are generated separately by area.
- Projects use `research_areas` and `members` as the single source of truth for associations.
- Area-specific member selectors are derived automatically from matching current projects.
- Project summaries and project pages resolve and link existing research areas and members automatically.

## Milestone 4 — Homepage

**Status: completed for current stage**

The homepage is now treated as a gateway rather than a duplicate of the site's main sections.

### Current composition

1. Language-specific redesigned CENTAUR logo.
2. Short bilingual introduction.
3. Responsive research-area navigation grid.
4. News and announcements.

There is currently no separate People section and no separate Research Areas section below the hero.

### Content and language handling

- English homepage introduction: `content/en/_index.md`
- Greek homepage introduction: `content/el/_index.md`
- The hero renders `.Content`, so bilingual prose remains in content rather than in the layout.
- Research-area links are derived automatically from the canonical area pages.

### Visual design

- Full English and Greek CENTAUR logos redesigned.
- Active files:
  - `static/images/logo/centaur-full-en.png`
  - `static/images/logo/centaur-full-el.png`
- Logo hierarchy emphasizes CENTAUR, NKUA affiliation, the Research Laboratory designation, and then the formal Center name.
- Intro text is justified on larger screens, with left alignment available for small screens.
- Fine spacing is handled with dedicated classes in `assets/css/custom.css`.
- Spacing between logo, introduction, research links, and News has been adjusted.

### Active homepage templates

- `layouts/partials/home/centaur.html`
- `layouts/partials/home/hero.html`
- `layouts/partials/home/news.html`

Older homepage partials may remain in the repository but are not part of the active composition.

## Milestone 5 — Research content expansion

**Status: next / in progress**

The architecture is in place. The next stage is mainly editorial:

- continue adding and refining research-area content;
- add current projects gradually;
- verify project-to-area and project-to-member associations as content grows;
- refine the Research landing page only where needed after more content exists.

The homepage research-area links already point to the corresponding research pages, so no separate homepage data source is required.

## Later work

Deferred until the core content is more mature:

- reconsider an **About** item/page;
- add further homepage sections only if they provide information not already represented clearly elsewhere;
- expand publications, seminars, support, and related sections as content grows;
- refine compact logo variants for uses outside the homepage;
- clean up or document historical/alternate layout files such as `layouts/areas/single0.html`.

## Current architecture rule

> Content is the source of truth; layouts derive presentation from it; the homepage summarizes and links rather than duplicating content.
