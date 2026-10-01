# Repository Structure and Design Responsibilities

This document explains how the CENTAUR website repository is organized, what each directory is for, and where different kinds of design decisions belong.

The site is built with Hugo and uses the Blowfish theme as a Git submodule. The project adds its own bilingual content, configuration, and layout overrides on top of that theme.

## Design philosophy

The repository follows four main ideas:

1. **Separate meaning from presentation.** Written material belongs in `content/` or `data/`; HTML structure belongs in `layouts/`; site-wide behaviour belongs in `config/`.
2. **Keep one source of truth.** A fact should be maintained once and then reused by list pages, detail pages, and homepage previews.
3. **Customize outside the theme.** Project-specific templates and styles belong at the repository root. The `themes/blowfish/` submodule should remain unchanged so that the theme can be upgraded safely.
4. **Treat the homepage as a gateway.** Homepage sections summarize the main areas of the site and link to complete section pages; they should not become independent copies of the same information.

## Repository map

```text
statlab/
├── .github/workflows/       GitHub Pages build and deployment
├── archetypes/              Defaults for new Hugo content
├── assets/
│   └── css/custom.css       Project CSS, including People and homepage spacing
├── config/_default/         Site, language, menu, markup, and theme settings
├── content/
│   ├── el/
│   │   ├── _index.md        Greek homepage introduction
│   │   ├── areas/           Greek research-area pages
│   │   ├── collaborations/  Greek collaboration page
│   │   ├── contact/         Greek contact page
│   │   ├── news/            Greek news and event records
│   │   ├── people/          Greek member records
│   │   ├── projects/        Greek research-project pages
│   │   ├── publications/    Greek publication landing pages
│   │   └── seminars/        Greek seminar-series records
│   └── en/
│       ├── _index.md        English homepage introduction
│       ├── areas/           English research-area pages
│       ├── collaborations/  English collaboration page
│       ├── contact/         English contact page
│       ├── news/            English news and event records
│       ├── people/          English member records
│       ├── projects/        English research-project pages
│       ├── publications/    English publication records and indexes
│       └── seminars/        English seminar-series records
├── data/                    Reserved for shared structured data
├── docs/                    Project documentation
├── i18n/                    Optional interface translation strings
├── layouts/
│   ├── areas/
│   ├── news/
│   ├── research_areas/
│   ├── people/
│   ├── projects/
│   ├── publications/
│   ├── seminars/
│   └── partials/
│       ├── home/
│       └── news/
├── static/images/
│   ├── logo/                CENTAUR logo variants
│   └── people/              Member images and placeholders
├── themes/blowfish/         Upstream theme Git submodule
└── public/                  Generated site output; never edit by hand
```

## Homepage architecture

The active homepage composition is intentionally compact:

1. language-specific CENTAUR logo;
2. bilingual introductory text;
3. responsive research-area navigation grid;
4. News and announcements.

There is currently **no separate People section and no separate Research Areas section** below the hero.

The active composition is controlled by:

```text
layouts/partials/home/centaur.html
layouts/partials/home/hero.html
layouts/partials/home/news.html
```

`centaur.html` includes the hero and News partials. The hero contains the logo, homepage introduction, and research-area directory.

The homepage introduction is maintained in the body of:

```text
content/en/_index.md
content/el/_index.md
```

The template renders the current page's `.Content`, so the English and Greek versions remain editorial content rather than language-specific text embedded in the layout.

Research-area links are derived automatically from the canonical Markdown pages under:

```text
content/en/areas/
content/el/areas/
```

Adding a matching research-area Markdown file therefore updates both the main Research page and the homepage research-area navigation automatically.

## Homepage logo

The language-specific full CENTAUR logos are configured in:

```text
config/_default/languages.en.toml
config/_default/languages.el.toml
```

The active files are:

```text
static/images/logo/centaur-full-en.png
static/images/logo/centaur-full-el.png
```

The corresponding configuration paths intentionally omit the `static/` prefix because Hugo publishes files under `static/` at the site root.

The redesigned full logos use the following visual hierarchy:

1. CENTAUR;
2. Department of Mathematics / National and Kapodistrian University of Athens;
3. Research Laboratory for Statistical Learning and Decision Support;
4. Center for Statistical Support and Strategic Development.

Other compact logo variants may remain available for future use in headers, documents, or constrained spaces.

## Homepage styling

Small visual adjustments are handled through project-owned CSS in:

```text
assets/css/custom.css
```

This currently includes dedicated homepage classes for:

- spacing below the hero;
- spacing between the logo and introductory text;
- homepage introduction alignment.

Dedicated CSS is preferred for these fine adjustments when the needed utility classes are not present in the generated Blowfish/Tailwind CSS.

## People section

The People section groups members into:

- Faculty
- Special Teaching Staff
- Postdoctoral Researchers
- PhD Students
- Graduate Students
- Alumni
- External Collaborators

People cards use a two-column layout with a fixed 112×112 image area, optional placeholder image, language-specific sorting, and optional links such as website, Google Scholar, ORCID, GitHub, and LinkedIn.

The Greek top-menu label is `Μέλη`.

## Research areas and projects

Research areas are canonical Markdown content under `content/<language>/areas/`. English and Greek files use matching filenames as stable identifiers.

Projects are canonical Markdown content under `content/<language>/projects/` and use:

```yaml
research_areas:
  - operations-research
members:
  - burnetas
```

The `research_areas` and `members` identifiers are the single source of truth for associations. Area-specific Current Research Projects pages and member selectors are derived automatically from project metadata.

## News and Seminars model

All ordinary news and dated events are Markdown records under
`content/<language>/news/`. Their top-level discriminator is:

```yaml
news_kind: event  # dated event
# or
news_kind: news   # non-event announcement
```

Event categories are `research_talk`, `seminar_session`, `phd_defense`, and
`msc_presentation`; `general_event` is available for other events. News
categories include `outreach`, `media`, and `general`.

The News list and homepage preview use the shared presentation logic in
`layouts/partials/news/presentation.html`. The News page contains the complete
stream. `layouts/seminars/list.html` deliberately selects only records with
`news_kind: event` and category `research_talk` or `seminar_session`.
`phd_defense`, `msc_presentation`, and `general_event` therefore remain
News-only.

Persistent academic seminar series are separate Markdown records under
`content/<language>/seminars/`. They may link to their teaching materials with
`eclass_url`. A session connects to its series by setting `seminar_id` to the
series filename without `.md`. The visible section titles remain `Seminars`
and `Σεμινάρια`.

## Directory responsibilities

- `content/`: page wording, biographies, research descriptions, homepage prose, and metadata.
- `data/`: reserved for reusable structured facts that are not naturally standalone pages; current core collections live under `content/`.
- `config/`: navigation, languages, URLs, Markdown behaviour, theme options, and language-specific logo paths.
- `layouts/`: custom page composition and reusable presentation components.
- `assets/`: project-owned CSS and other source assets processed by Hugo.
- `static/`: images and files copied directly to the built site.
- `docs/`: architecture, maintenance guidance, milestones, and contributor documentation.
- `themes/blowfish/`: upstream theme; do not edit for project-specific changes.
- `public/`: generated output; never edit manually.

## Change-placement examples

- Change homepage introduction: edit `content/en/_index.md` and `content/el/_index.md`.
- Replace the full bilingual homepage logos: replace the corresponding files in `static/images/logo/` while keeping the configured filenames, or update the language configuration paths.
- Change homepage composition: edit `layouts/partials/home/`.
- Change homepage spacing/alignment: edit `assets/css/custom.css`.
- Add a research area: create matching Markdown files under the English and Greek `areas/` directories.
- Add a project: create matching project files and declare `research_areas` and `members`; do not duplicate these associations elsewhere.
- Add ordinary news or an event: create matching records under `content/<language>/news/` and set `news_kind` plus the appropriate `category`.
- Add an academic seminar series: create matching records under `content/<language>/seminars/`; use the filename as the `seminar_id` on related `seminar_session` events and set `eclass_url` on the series when materials exist.
- Change People layout: edit `layouts/people/` and project CSS as appropriate.
- Reorder navigation: edit the language-specific menu files under `config/_default/`.

## Repository hygiene

- Do not commit editor backups, lock files, `.DS_Store`, `.Rhistory`, or other machine-local files.
- Do not commit `public/`.
- Do not make ad hoc changes inside `themes/blowfish/`.
- Keep Greek and English structures aligned where both translations are intended.
- Test locally with `hugo --noBuildLock` before committing.
- Keep content, template, and theme-upgrade changes in focused commits when practical.
