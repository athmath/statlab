# Repository Structure and Design Responsibilities

This document explains how the CENTAUR website repository is organized, what
each directory is for, and where different kinds of design decisions belong.

The site is built with [Hugo](https://gohugo.io/) and uses the Blowfish theme as
a Git submodule. The project adds its own bilingual content, configuration, and
layout overrides on top of that theme.

## Design philosophy

The repository follows four main ideas:

1. **Separate meaning from presentation.** Written material belongs in
   `content/` or `data/`; HTML structure belongs in `layouts/`; site-wide
   behaviour belongs in `config/`.
2. **Keep one source of truth.** A fact should be maintained once and then
   reused by list pages, detail pages, and homepage previews.
3. **Customize outside the theme.** Project-specific templates and styles
   belong at the repository root. The `themes/blowfish/` submodule should remain
   unchanged so that the theme can be upgraded safely.
4. **Treat the homepage as a gateway.** Homepage sections summarize the main
   areas of the site and link to complete section pages; they should not become
   independent copies of the same information.

The practical rule is:

| If the change affects... | Put it in... |
| --- | --- |
| Wording, biographies, research descriptions, or page metadata | `content/` |
| Reusable structured facts shared by several pages | `data/` |
| Navigation, languages, URLs, Markdown handling, or theme options | `config/` |
| HTML structure, page composition, cards, lists, or homepage sections | `layouts/` |
| Project CSS processed by Hugo | `assets/` |
| Images and downloads copied without processing | `static/` |
| Default front matter for newly created content | `archetypes/` |
| Theme internals | Prefer a root-level override; do not edit `themes/blowfish/` |
| Contributor and architecture guidance | `docs/` |
| Build and deployment automation | `.github/workflows/` |

## Repository map

```text
statlab/
├── .github/workflows/       GitHub Pages build and deployment
├── archetypes/              Defaults for new Hugo content
├── assets/                  Source assets processed by Hugo
├── config/_default/         Site, language, menu, markup, and theme settings
├── content/
│   ├── el/                  Greek pages and section directories
│   │   ├── areas/           Greek research-area pages
│   │   └── projects/        Greek research-project pages
│   └── en/                  English pages and section directories
│       ├── areas/           English research-area pages
│       └── projects/        English research-project pages
├── data/                    Structured, reusable site data
├── docs/                    Project documentation
├── i18n/                    Optional interface translation strings
├── layouts/                 Project templates and theme overrides
│   ├── areas/               Research-area list and detail templates
│   ├── people/              People list and profile templates
│   ├── projects/             Research-project detail templates
│   └── partials/            Reusable presentation components
│       └── home/            Homepage sections
├── static/                  Files copied directly to the built site
├── themes/blowfish/         Upstream theme Git submodule
└── public/                  Generated site output; never edit by hand
```

## Directory responsibilities

### `.github/workflows/`

Contains deployment automation. `hugo.yml` checks out the repository and its
submodules, installs the configured Hugo version, builds the site, and publishes
`public/` to GitHub Pages when changes reach `main`.

This folder owns the **delivery design**: how source files become the deployed
website. It does not own page content or visual layout.

### `archetypes/`

Contains templates used when creating new pages with `hugo new`. The current
`default.md` supplies default front matter.

This folder owns **authoring defaults**, not the rendering of existing pages.

### `assets/`

Contains source assets that Hugo may transform, bundle, fingerprint, or minify.
Project CSS belongs in `assets/css/` when it should participate in Hugo's asset
pipeline.

This folder owns the **source styling layer**. Files that merely need to be
copied unchanged should go in `static/` instead.

### `config/_default/`

Contains the site's global configuration, divided by responsibility:

- `hugo.toml` defines the base URL, default language, outputs, taxonomies,
  pagination, and other Hugo-wide behaviour.
- `languages.el.toml` and `languages.en.toml` define language-specific site
  settings.
- `menus.el.toml` and `menus.en.toml` define navigation independently for each
  language.
- `markup.toml` controls Markdown rendering.
- `params.toml` controls project and Blowfish theme options.

This folder owns **site-wide behaviour and design settings**. It should not hold
page prose or custom HTML.

### `content/`

Contains the pages editors maintain. The first level separates Greek (`el`) and
English (`en`) content. Both language trees should normally have matching
sections:

- `areas/` — research-area overview and individual research areas;
- `projects/` — individual research projects and the projects section index;
- `people/` — member listing and individual profiles;
- `seminars/` — seminar information;
- `publications/` — publication information;
- `news/` — news and announcements;
- `contact/` — contact details;
- `support/` — support information;
- `_index.md` — the homepage or a section's landing-page content.

Files named `_index.md` describe a section itself. Other Markdown files usually
represent individual pages within that section. Front matter stores metadata
such as title, description, category, weight, and research interests; the body
stores longer prose.

This folder owns **editorial design and information hierarchy**: what the site
says, which section a page belongs to, and how pages are ordered. Greek and
English files are separate editorial sources, not automatic translations.

#### Frozen research-area and project structure

The Research Areas/Projects structure is currently considered stable. Each
language has parallel `areas/` and `projects/` directories:

```text
content/
├── en/
│   ├── areas/
│   │   ├── _index.md
│   │   ├── probability.md
│   │   ├── statistics.md
│   │   ├── machine-learning.md
│   │   ├── operations-research.md
│   │   └── data-science.md
│   └── projects/
│       ├── _index.md
│       └── project-slug.md
└── el/
    ├── areas/
    │   ├── _index.md
    │   ├── probability.md
    │   ├── statistics.md
    │   ├── machine-learning.md
    │   ├── operations-research.md
    │   └── data-science.md
    └── projects/
        ├── _index.md
        └── project-slug.md
```

Adding a Markdown file to an `areas/` directory automatically adds that area to
the language's main Research page and homepage Research Areas list. The
filename without `.md` is the stable area identifier. English and Greek area
files must therefore use matching filenames even though their displayed titles
and prose differ.

Area pages are ordered by `weight`. Their standard body headings use explicit,
language-independent anchors so that the page navigation remains reliable:

```markdown
## Overview {#overview}
## Research Themes {#research-themes}
```

The corresponding Greek headings use the same `#overview` and
`#research-themes` identifiers. The Current Research Projects navigation item
and section appear only when the area has at least one matching current
project.

Each project is stored once per language under `projects/`. A typical project
file begins:

```yaml
---
title: "Project title"
description: "A short description with an optional [link](https://example.com)."
team: "**Lab Member**, in collaboration with External Collaborator from Institution."
status: current
weight: 10
research_areas:
  - operations-research
  - data-science
---
```

The `research_areas` values refer to area filenames, not translated titles. A
project can list any number of areas and is added automatically to every
matching area page. Unknown identifiers are ignored without causing a build
error. If a matching area file is created later, the project appears there
automatically on the next build. Only projects with `status: current` appear in
the Current Research Projects sections.

The optional `team` field is a Markdown-enabled prose line shown immediately
below the project description without a visible label. Lab members can be
emphasized manually with Markdown bold text while collaborators and affiliations
remain ordinary prose. It deliberately does not link projects to People records.

Individual project pages list and link their existing research areas. Unknown
or not-yet-created areas remain hidden until the corresponding area page
exists. Short project descriptions and team lines support inline Markdown.

### `data/`

Contains reusable structured information when a collection of facts needs to
be read by several templates or pages and is not naturally a standalone content
page. Research areas are not stored here: their Markdown pages under `content/`
are the canonical source for the Research page and homepage list.

This folder owns the **structured information model**. Avoid maintaining the
same facts in both `data/` and `content/`; choose one canonical source and have
templates derive other views from it.

### `docs/`

Contains internal documentation about architecture, content conventions, and
the data model. These files guide contributors and are not normally published as
site pages.

This folder owns **project knowledge and maintenance rules**.

### `i18n/`

Reserved for Hugo interface strings that must be translated, such as labels
generated by templates. It is different from `content/el/` and `content/en/`,
which contain complete pages.

This folder owns **small reusable interface translations**. Page text should
remain under `content/`.

### `layouts/`

Contains project-owned Hugo templates. A file here overrides the corresponding
template from Blowfish without modifying the theme submodule.

- `layouts/areas/list.html` controls the research-area listing.
- `layouts/areas/single.html` controls individual research-area pages.
- `layouts/projects/single.html` controls individual project pages and links
  each project back to its existing research areas.
- `layouts/people/list.html` groups and displays people.
- `layouts/people/single.html` controls individual profile pages.
- `layouts/partials/research-panel.html` is a reusable research-area component.
- `layouts/partials/project-summary.html` renders projects within research-area
  pages, including Markdown links in short descriptions.
- `layouts/partials/home/` contains the components used to assemble the
  homepage, including the hero, people, research, seminars, publications, and
  support sections.

This folder owns **structural and component design**: which fields are shown,
their HTML hierarchy, and how reusable page sections are composed. Broad visual
styling should be expressed through classes and project CSS rather than repeated
inline across templates.

`layouts/areas/single0.html` appears to be an alternate or historical template.
It should either be documented as an intentional reference or removed after
comparison so that future maintainers do not mistake it for an active layout.

### `static/`

Contains files Hugo copies directly to the published site without processing.
The current files are the CENTAUR logo variants under `static/images/logo/`.
Future photographs and downloadable documents can also live here, organized by
purpose.

This folder owns **final static media**. Do not edit generated copies under
`public/`.

### `themes/blowfish/`

Contains the upstream Blowfish theme as a Git submodule. Blowfish provides the
base visual system, default templates, components, and theme behaviour.

This folder owns the **upstream foundation**, not CENTAUR-specific design.
Project changes should be made through configuration, root-level layout
overrides, and root-level assets. Direct theme edits make upgrades difficult and
should be avoided.

### `public/`

Contains the generated website produced by Hugo. It is ignored by Git and can be
recreated at any time with a build.

This folder owns nothing authoritative. **Never edit it manually**: every change
will be overwritten by the next build.

## How a page is assembled

A typical page follows this flow:

```text
config + content/data + project layout override + Blowfish defaults
                              |
                              v
                         Hugo build
                              |
                              v
                           public/
                              |
                              v
                       GitHub Pages
```

For example, a research-area page gets its title and description from
`content/<language>/areas/`, its custom structure from `layouts/areas/`, its
shared visual foundation from Blowfish, and global behaviour from
`config/_default/`.

Project associations follow this flow:

```text
project research_areas identifiers
              |
              v
matching area filenames in the same language
              |
              v
automatic project summaries on each matching area page
```

## Change-placement examples

- Correct a person's biography: edit the corresponding files under
  `content/el/people/` and `content/en/people/`.
- Reorder a navigation item: edit the matching language menu file under
  `config/_default/`.
- Change how all people are grouped: edit `layouts/people/list.html`.
- Change how one profile is displayed: edit `layouts/people/single.html`.
- Add a research area: create matching Markdown files under the English and
  Greek `areas/` directories and assign their ordering weights.
- Add a project: create matching Markdown files under the English and Greek
  `projects/` directories and list the relevant area filename identifiers in
  `research_areas`.
- Add a homepage section: create or update a partial under
  `layouts/partials/home/` and include it from the homepage composition.
- Change colors or spacing across the site: use configuration where Blowfish
  exposes an option; otherwise add project CSS under `assets/css/`.
- Add a logo or downloadable PDF: place it under `static/`.
- Change the upstream theme version: update the `themes/blowfish` submodule in a
  dedicated change and test the complete site afterward.

## Repository hygiene

- Do not commit editor backups, lock files, `.DS_Store`, `.Rhistory`, or other
  machine-local files.
- Do not commit `public/`; it is build output.
- Do not make ad hoc changes inside `themes/blowfish/`.
- Keep the Greek and English section structures aligned where both translations
  are intended to exist.
- Test locally with `hugo --noBuildLock` before committing.
- Keep content changes, template changes, and theme upgrades in focused commits
  when practical; this makes review and rollback easier.
