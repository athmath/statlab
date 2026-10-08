# CENTAUR Website Architecture

**Project:** CENTAUR Laboratory Website
**Framework:** Hugo + Blowfish Theme
**Repository:** `statlab`

---

# 1. Purpose

The CENTAUR website is designed as the public website of the Statistics & Operations Research Laboratory of the Department of Mathematics, National and Kapodistrian University of Athens.

The objectives of the website are to:

* present the laboratory and its members,
* showcase research activities,
* announce seminars and scientific events,
* disseminate publications,
* publish laboratory news,
* provide contact information.

The site is intended to grow over many years while remaining easy to maintain by multiple contributors.

---

# 2. Design Philosophy

The website follows five fundamental principles.

## 2.1 Homepage as a Gateway

The homepage is **not** a single-page website.

Instead, it serves as an overview of the laboratory and directs visitors to dedicated pages containing complete information.

Each homepage section is a preview of a corresponding page.

Example:

```
Homepage
    ↓
Research (preview)
    ↓
Research page
```

---

## 2.2 One Source of Truth

Information should exist in only one place.

Pages, homepage previews and reusable components should all derive their information from the same underlying content or data.

Avoid duplicating information.

---

## 2.3 Separation of Responsibilities

The project separates:

* content,
* structured data,
* presentation,
* configuration.

Each has a different purpose.

---

## 2.4 Component-Based Design

The website is built from reusable components ("partials").

Whenever the same visual element appears in multiple places, it should be implemented once and reused.

Examples include:

* person cards,
* publication cards,
* seminar cards,
* news items.

---

## 2.5 Incremental Development

The website is developed in milestones.

Each milestone:

1. introduces one conceptual change,
2. is tested locally,
3. is committed separately,
4. is deployed through GitHub Actions.

---

# 3. Project Structure

```
statlab/

├── .github/workflows/       GitHub Pages build and deployment
├── archetypes/              Defaults for new content
├── assets/css/              Project CSS processed by Hugo
├── config/_default/         Site, language, menu and theme settings
├── content/
│   ├── en/                  English pages and records
│   └── el/                  Greek pages and records
├── data/                    Reserved for shared structured data
├── docs/                    Project documentation
├── i18n/                    Reusable interface translations
├── layouts/                 Project templates and theme overrides
├── static/images/           Logos and People images
├── themes/blowfish/         Upstream theme submodule
└── public/                  Generated output; never edit manually
```

---

# 4. Directory Responsibilities

## content/

Contains the actual pages of the website.

Examples:

* Research
* People
* Seminars
* Publications
* Collaborations
* News
* Contact

Language-specific content is organized as

```
content/
    en/
        areas/
        collaborations/
        contact/
        news/
        people/
        projects/
        publications/
        seminars/
    el/
        ...matching sections...
```

Research areas, people, projects, publications, News items and seminar-series
records are canonical Markdown content. Greek and English records use parallel
directories; matching filenames act as stable identifiers where templates
resolve relationships.

---

## data/

Reserved for reusable structured facts that are shared by several templates
and are not naturally standalone pages. The current core collections are
stored under `content/`, not duplicated in `data/`.

---

## layouts/

Contains the presentation layer.

Templates determine **how** information is displayed.

The project overrides only the templates that require customization.

Current custom template areas are:

```text
layouts/
    areas/
    news/
    people/
    projects/
    publications/
    research_areas/
    seminars/
    partials/
```

---

## layouts/partials/

Contains reusable HTML components.

Partials are ordinary Hugo partials and may be reused by any page.

They are organized by purpose rather than by technical constraints.

Current homepage partials reside in

```
layouts/partials/home/
```

News presentation metadata is normalized by
`layouts/partials/news/presentation.html`; People, project and research
components are also implemented as project-owned partials.

---

## static/

Contains static resources copied unchanged into the final website.

Typical contents include:

* images
* logos
* downloadable documents
* icons

---

## config/

Contains Hugo configuration.

Configuration is split into multiple files, including:

* site configuration
* language configuration
* menus
* markup options
* theme parameters

---

## themes/

Contains the Blowfish theme.

Project customizations should be implemented outside the theme whenever possible.

The theme itself should remain unmodified to simplify upgrades.

---

## docs/

Project documentation.

Documents describe the architecture, conventions and development process rather than Hugo itself.

---

# 5. Homepage Architecture

The homepage is assembled using the custom homepage layout.

```
index.html
        ↓
centaur.html
        ↓
hero: logo + research-area directory
news
```

The homepage acts as the entry point to the website.

Research-area entries link directly to their corresponding area pages. The
homepage does not duplicate the People, Research, Seminars, Publications, or
Collaborations sections; those remain available through the main navigation.

---

# 6. Homepage Sections

Version 1.0 contains the following sections.

1. Institutional logo
2. Language-specific introduction
3. Research Areas directory
4. Latest News and announcements

Future versions may extend the homepage, but unnecessary sections should be avoided.

---

# 7. Main Navigation

Version 1.0 navigation:

* Home
* People
* Research
* Seminars
* Publications
* Collaborations
* News
* Contact

Each navigation item corresponds to a dedicated page.

---

# 8. News and Seminars

News and dated events share one announcement collection under
`content/<language>/news/`. The `news_kind` field identifies the record type:

* `event` for a dated event;
* `news` for a non-event announcement.

The category vocabulary is separate for each kind. Academic event categories
are `research_talk`, `seminar_session`, `phd_defense`, and
`msc_presentation`. General events may use `general_event`. News categories
include `outreach`, `media`, and `general`.

The News page is the complete announcement stream. The Seminars page reuses
only event records whose category is `research_talk` or `seminar_session`.
Consequently, `phd_defense`, `msc_presentation`, and `general_event` remain
News-only and are not surfaced on the Seminars page.

Academic seminar series are persistent records under
`content/<language>/seminars/`. A series may define `semester`, `start_date`,
`end_date`, and `eclass_url`; the last field links to the eClass location where
materials are maintained. A `seminar_session` event links to its parent series
with `seminar_id`, using the seminar-series filename without `.md` as the
stable identifier.

This gives the two sections distinct responsibilities:

```text
content/<language>/news/       dated events and ordinary news
              |
              +--> News page: all records
              +--> Seminars page: research_talk + seminar_session only

content/<language>/seminars/   persistent academic seminar-series records
```

The public page titles remain `Seminars` in English and `Σεμινάρια` in
Greek. The custom rendering is owned by `layouts/news/`,
`layouts/seminars/list.html`, and
`layouts/partials/news/presentation.html`.

---

# 9. Multilingual Design

The website is bilingual.

Languages currently supported:

* English
* Greek

Language-specific textual content belongs in `content/`.

Interface strings should use Hugo internationalization where appropriate.

Templates should remain language-independent whenever possible.

---

# 10. Deployment

Development workflow:

```
Edit source
      ↓
Local preview (hugo server)
      ↓
Git commit
      ↓
Git push
      ↓
GitHub Actions
      ↓
GitHub Pages
```

The `public/` directory is generated automatically and is not edited manually.

---

# 11. Development Principles

When extending the website:

* keep content separate from presentation,
* reuse partials whenever possible,
* avoid duplication,
* keep the homepage concise,
* create dedicated pages for substantial content,
* preserve compatibility with the Blowfish theme whenever practical,
* implement one logical change per Git commit.

---

# 12. Future Evolution

Potential future additions include:

* Software
* Teaching
* Resources
* Search
* Publication database integration

These are intentionally excluded from Version 1.0 until they represent active and regularly maintained laboratory activities.

---

# 13. Guiding Principle

The CENTAUR website is intended to be a long-lived academic resource rather than a static brochure.

Its architecture prioritizes:

* clarity,
* maintainability,
* extensibility,
* multilingual support,
* reuse of components,
* ease of contribution by future laboratory members.
