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

## M3 – Hero Section

Status: Planned

Objectives:

* Update navigation chips
* Improve hero buttons
* Finalize introductory text

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

Status: Planned

Objectives:

Create the Seminars page.

Structure:

Semester Seminar Series

* current series
* archive

Statistics & Operations Research Seminar

* Talks
* Presentations
* Archive

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

Status: Planned

Objectives:

Create the News page.

Typical announcements:

* publications
* awards
* grants
* conferences
* seminar announcements
* laboratory activities

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
