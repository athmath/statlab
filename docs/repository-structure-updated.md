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
│   │   └── projects/        Greek research-project pages
│   └── en/
│       ├── _index.md        English homepage introduction
│       ├── areas/           English research-area pages
│       └── projects/        English research-project pages
├── data/                    Structured, reusable site data
├── docs/                    Project documentation
├── i18n/                    Optional interface translation strings
├── layouts/
│   ├── areas/
│   ├── research_areas/
│   ├── people/
│   ├── projects/
│   └── partials/
│       └── home/
├── static/
│   └── images/logo/         CENTAUR logo variants
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
- PhD Students
- Graduate Students
- Alumni
- Visiting Researchers

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

## Directory responsibilities

- `content/`: page wording, biographies, research descriptions, homepage prose, and metadata.
- `data/`: reusable structured facts that are not naturally standalone pages.
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
- Change People layout: edit `layouts/people/` and project CSS as appropriate.
- Reorder navigation: edit the language-specific menu files under `config/_default/`.

## Repository hygiene

- Do not commit editor backups, lock files, `.DS_Store`, `.Rhistory`, or other machine-local files.
- Do not commit `public/`.
- Do not make ad hoc changes inside `themes/blowfish/`.
- Keep Greek and English structures aligned where both translations are intended.
- Test locally with `hugo --noBuildLock` before committing.
- Keep content, template, and theme-upgrade changes in focused commits when practical.
