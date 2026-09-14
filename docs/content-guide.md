# CENTAUR Website Content Guide

This document explains where the content of the website is stored and how it should be maintained.

It is intended for laboratory members who update the website but are not expected to modify Hugo templates.

---

# General Principles

The website separates:

* content,
* structured data,
* presentation.

Contributors should normally edit only the content and data files.

HTML templates should only be modified when changing the design.

---

# Directory Overview

```text
content/
```

Contains complete pages.

Examples:

* Research
* People
* Seminars
* Publications
* News
* Contact

---

```text
data/
```

Contains structured information used throughout the website.

Examples:

* research areas
* seminar information
* people
* publications
* news

---

```text
layouts/
```

Contains the presentation layer.

Normally edited only by site developers.

---

```text
static/
```

Contains static files such as:

* images
* logos
* downloadable documents

---

# Homepage

The homepage contains summaries of the major sections of the website.

It is **not** intended to contain complete information.

Each homepage section links to a dedicated page.

---

# People

Purpose

Present laboratory members.

Typical information:

* name
* position
* affiliation
* photograph
* research interests
* email
* webpage

Future location:

```text
data/people.yaml
```

or

```text
data/people/
```

depending on the final organization.

---

# Research

Purpose

Present the laboratory's research areas.

Each research area should include:

* title
* short description
* keywords
* related faculty
* related publications

Current location

```text
data/research.yaml
```

---

# Seminars

The seminar activities are divided into two categories.

## Semester Seminar Series

Examples:

* Strategic Queueing
* Mathematics of Machine Learning

Each series contains:

* title
* semester
* organizers
* description
* sessions

---

## Statistics & Operations Research Seminar

Contains:

* Talks
* Presentations
* Archive

Each session should include:

* title
* speaker
* affiliation
* date
* abstract
* category

---

# Publications

Purpose

Present laboratory publications.

Initially maintained manually.

Future versions may use:

* BibTeX
* DOI metadata
* automatic imports

---

# News

Purpose

Announce laboratory activities.

Typical news items:

* publications
* grants
* awards
* conference participation
* seminar announcements
* new members

News should be concise.

Whenever possible, include a link to additional information.

---

# Contact

Contains:

* laboratory address
* email
* map
* social media (if applicable)

---

# Images

Images should be stored under

```text
static/images/
```

Suggested organization:

```text
images/

logo/

people/

seminars/

news/

research/
```

Images should be optimized before being added to the repository.

---

# Languages

The website supports:

* English
* Greek

Whenever possible, both language versions should be updated together.

Neither language should be allowed to become significantly outdated.

---

# Before Editing

Before modifying content:

1. Pull the latest version from GitHub.
2. Create your changes.
3. Preview using

```text
hugo server
```

4. Verify both language versions.
5. Commit with a descriptive message.
6. Push to GitHub.

---

# Editing Rules

* Keep text concise.
* Use consistent terminology.
* Prefer Markdown over HTML.
* Avoid duplicated information.
* Keep links up to date.
* Verify spelling before committing.

---

# Future Contributors

Contributors should normally edit only:

* `content/`
* `data/`
* `static/images/`

Changes to:

* `layouts/`
* `config/`
* `themes/`

should be made only when modifying the website design or functionality.
