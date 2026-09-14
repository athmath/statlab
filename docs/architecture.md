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

├── archetypes/
├── assets/
├── config/
├── content/
├── data/
├── docs/
├── i18n/
├── layouts/
├── static/
├── themes/
└── public/      (generated)
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
* News
* Contact

Language-specific content is organized as

```
content/
    en/
    el/
```

---

## data/

Contains structured information used to build parts of the website.

Examples:

* research areas
* seminar information
* publications
* people
* news

The goal is to keep structured information separate from HTML.

---

## layouts/

Contains the presentation layer.

Templates determine **how** information is displayed.

The project overrides only the templates that require customization.

---

## layouts/partials/

Contains reusable HTML components.

Partials are ordinary Hugo partials and may be reused by any page.

They are organized by purpose rather than by technical constraints.

Current homepage partials reside in

```
layouts/partials/home/
```

Future reusable components (cards, widgets, etc.) may be organized into additional subdirectories.

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
hero
people
research
seminars
publications
news
```

The homepage acts as the entry point to the website.

Each section provides a concise overview together with a link to the corresponding full page.

---

# 6. Homepage Sections

Version 1.0 contains the following sections.

1. Hero
2. People
3. Research
4. Seminars
5. Recent Publications
6. Latest News

Future versions may extend the homepage, but unnecessary sections should be avoided.

---

# 7. Main Navigation

Version 1.0 navigation:

* Home
* People
* Research
* Seminars
* Publications
* News
* Contact

Each navigation item corresponds to a dedicated page.

---

# 8. Seminars

The seminar activities are organized into two categories.

## Semester Seminar Series

Semester-long thematic seminars.

Examples:

* Strategic Queueing
* Mathematics of Machine Learning

---

## Statistics & Operations Research Seminar

The laboratory's permanent research seminar.

Contains:

* Talks
* Presentations
* Archive

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

* Research Projects
* Software
* Teaching
* Resources
* Collaborations
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
