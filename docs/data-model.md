# CENTAUR Website Data Model

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

Contains the site's canonical pages and content records.

Examples:

* Research
* People
* Seminars
* Publications
* Collaborations
* News
* Contact

---

```text
data/
```

Reserved for structured information that must be reused by several templates
and is not naturally a page. The current research areas, people, projects,
publications, News items, events, and seminar series live under `content/`.

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

Current location:

```text
content/<language>/people/
```

The filename without `.md` is the stable member identifier used by project
records.

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
content/<language>/areas/
```

The filename without `.md` is the stable research-area identifier used by
project records.

---

# Seminars

Academic seminar series and individual dated sessions are different record
types.

## Seminar-series record

Persistent series live under `content/<language>/seminars/`. Their filename
without `.md` is the series identifier. Supported series metadata includes:

```yaml
title: "Series title"
description: "Short description"
semester: "Spring 2027"
start_date: 2027-02-15
end_date: 2027-06-15
eclass_url: "https://eclass.uoa.gr/..."
```

`eclass_url` is the link to the external location for seminar materials.

## Seminar-session relationship

An individual session is an event under `content/<language>/news/`:

```yaml
news_kind: event
category: seminar_session
seminar_id: series-filename
```

`seminar_id` refers to the series filename without `.md`. Research talks use
`category: research_talk`; only these two categories are selected by the
Seminars page. The public titles remain `Seminars` and `Σεμινάρια`.

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

All announcements and dated events live under `content/<language>/news/`.
`news_kind` is the primary discriminator:

| `news_kind` | Meaning | Categories |
| --- | --- | --- |
| `event` | Dated event | `research_talk`, `seminar_session`, `phd_defense`, `msc_presentation` |
| `news` | Non-event announcement | `outreach`, `media`, `general` |

`general_event` is also supported for other dated events. An event uses `date`
for its publication date and `event_date` for its scheduled date and time.

The News page contains all records. The Seminars page is a filtered secondary
view containing only `research_talk` and `seminar_session`. PhD defenses, MSc
presentations, and general events remain News-only.

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
