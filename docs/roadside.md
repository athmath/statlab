# CENTAUR Website Roadmap

This document tracks the planned development of the CENTAUR website.

The roadmap is organized into milestones. Each milestone should correspond to one or more well-defined Git commits.

---

# Current Status

## Completed

* Initial Hugo project setup
* Blowfish theme integration
* GitHub repository configuration
* GitHub Actions deployment to GitHub Pages
* Bilingual site structure (English / Greek)
* Custom CENTAUR homepage
* Laboratory logo
* Homepage section reorganization

---

# Version 1.0

The goal of Version 1.0 is to launch a complete and professional laboratory website containing the essential information about CENTAUR.

---

## M1 – Homepage Architecture ✓

Status: Completed

Objectives:

* Reorganize homepage sections
* Remove obsolete sections
* Establish homepage as an overview page

---

## M2 – Homepage Terminology

Status: Planned

Objectives:

* Rename "Events" to "Seminars"
* Replace temporary event terminology
* Introduce the new seminar structure

---

## M3 – Homepage Design

Status: In progress

Objectives:

* Replace the section-navigation chips with links to individual research areas
* Use a simple, non-pill treatment for the research-area links
* Keep the homepage single-column and retain only the News preview below them

---

## M4 – Main Navigation

Status: Planned

Objectives:

Create the top-level navigation:

* Home
* People
* Research
* Seminars
* Publications
* Collaborations
* News
* Contact

---

## M5 – People

Status: Planned

Objectives:

Create the People page.

Initial contents:

* Director
* Faculty
* PhD Students
* MSc Students
* Alumni (optional)
* Visitors (optional)

Future versions may include:

* personal webpages
* photographs
* research interests
* publications

---

## M6 – Research

Status: Planned

Objectives:

Create the Research page.

Initial contents:

* Research Areas
* Short descriptions
* Related faculty

Future versions may include:

* projects
* software
* funding
* publications by area

---

## M7 – Seminars

Status: Implemented for the current architecture

Objectives:

Maintain the Seminars page as a focused view of actual seminar activity.

Current structure:

* persistent academic seminar-series records under `content/<language>/seminars/`;
* current/upcoming and past series derived from `start_date` and `end_date`;
* eClass links for series materials through `eclass_url`;
* research talks and seminar sessions reused from the News collection;
* sessions linked to their series through `seminar_id`;
* only `research_talk` and `seminar_session` events surfaced here.

The public titles remain `Seminars` and `Σεμινάρια`.

---

## M8 – Publications

Status: Planned

Objectives:

Create the Publications page.

Initial version:

* recent publications
* manual maintenance

Future versions:

* BibTeX integration
* filtering
* search
* author pages

---

## M9 – News

Status: Implemented for the current architecture

Objectives:

Maintain one complete announcement stream for ordinary news and dated events.

Current model:

* `news_kind: event` or `news_kind: news`;
* academic event categories `research_talk`, `seminar_session`, `phd_defense`,
  and `msc_presentation`;
* news categories such as `outreach`, `media`, and `general`;
* PhD defenses and MSc presentations remain News-only;
* an opt-in English social-media RSS feed is generated for records with
  `social_publish: true`.

---

## M10 – Contact

Status: Planned

Objectives:

Create the Contact page.

Include:

* laboratory location
* map
* contact details
* email
* social links (if applicable)

---

## M11 – Final Review

Status: Planned

Objectives:

* responsive testing
* accessibility review
* typography review
* image optimization
* bilingual consistency
* link checking
* final proofreading

---

# Version 2.0

These features are intentionally postponed.

Possible additions:

* Research Projects
* Software
* Teaching
* Resources
* Collaborations
* Consulting
* Laboratory Services
* Search
* Automatic publication import
* Seminar registration
* Calendar integration

---

# Development Workflow

Each milestone follows the same process.

1. Discuss the design.
2. Implement one logical change.
3. Test locally using `hugo server`.
4. Commit with a descriptive Git message.
5. Push to GitHub.
6. Verify deployment on GitHub Pages.

---

# Development Principles

* Keep the homepage concise.
* Avoid duplicate information.
* Prefer reusable partials.
* Keep data separate from presentation.
* Preserve compatibility with the Blowfish theme whenever practical.
* Complete one milestone before starting the next.

---

# Version History

## Version 0.1

Initial Hugo project.

---

## Version 0.2

Custom homepage and GitHub Pages deployment.

---

## Version 1.0 (planned)

First public release of the CENTAUR laboratory website.
