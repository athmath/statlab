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

Use matching filenames in the English and Greek directories for translated
versions of the same member.

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

Projects are stored under `content/<language>/projects/` and declare their
research-area and member relationships in front matter.

---

# Seminars

The visible section titles remain **Seminars** and **Σεμινάρια**. The page
combines persistent academic seminar-series records with selected dated events.

## Academic seminar series

Store each series as a Markdown record under:

```text
content/<language>/seminars/
```

A typical series defines:

```yaml
title: "Series title"
description: "Short description"
semester: "Spring 2027"
start_date: 2027-02-15
end_date: 2027-06-15
eclass_url: "https://eclass.uoa.gr/..."
```

The filename without `.md` is the stable series identifier. The series page is
the persistent public record; use `eclass_url` to direct participants to eClass
for teaching materials.

## Talks and seminar sessions

Dated talks and sessions are event records under
`content/<language>/news/`, not child pages of a seminar series. Use:

```yaml
news_kind: event
category: seminar_session
seminar_id: series-filename
```

`seminar_id` links a `seminar_session` to its series. Independent research
talks use `category: research_talk` and do not require `seminar_id`.

Only `research_talk` and `seminar_session` events are surfaced on the Seminars
page. PhD defenses and MSc presentations remain on the News page only.

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

The News collection is the canonical source for all announcements and dated
events:

```text
content/<language>/news/
```

Use `news_kind: event` for dated events and `news_kind: news` for ordinary
announcements. Their category vocabularies are:

| Kind | Categories |
| --- | --- |
| `event` | `research_talk`, `seminar_session`, `phd_defense`, `msc_presentation` |
| `news` | `outreach`, `media`, `general` |

`general_event` may be used for a dated event outside the four academic event
categories. Like defenses and MSc presentations, it is News-only.

For events, `date` is the publication date and `event_date` is the scheduled
date and time. Event records may also define `presenter`, `affiliation`,
`attendance`, `venue`, `online_url`, and `event_link`. Use `seminar_id` only for
a `seminar_session` linked to a persistent seminar-series record.

The News page displays every record. The homepage displays the four most recent
records by publication date. The Seminars page independently selects only the
`research_talk` and `seminar_session` event categories.

To publish a news item or event through the social-media RSS feed, set:

```yaml
social_publish: true
```

Items without this setting, or with `social_publish: false`, remain on the
website but are not included in the social-media feed. Social-media publishing
is enabled only for English news items; the corresponding Greek translation is
not published separately.

The generated feed path is `/en/news/social.xml`.

For events, the social feed prepends `event_date` and, when present, `venue` to
the item description. The RSS **Description** field therefore contains the
event date, venue, and short summary, while **Content** contains a plain-text
version of the Markdown body. The RSS **Pubdate** field is the website
publication date, not the event date.

Description is limited to 500 characters and Content to 1,900 characters so a
LinkedIn post assembled from Title, Description, Content, and Link remains
within the platform's post length limit. The link leads to the complete
announcement when the body is longer.

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
