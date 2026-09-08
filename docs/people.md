# People

This document describes the structure of the **People** section of the CENTAUR website.

## Directory structure

```
content/
├── en/
│   └── people/
│       ├── _index.md
│       ├── burnetas.md
│       ├── economou.md
│       └── ...
└── el/
    └── people/
        ├── _index.md
        ├── burnetas.md
        ├── economou.md
        └── ...
```

Each member has one Markdown file in each language.

---

## Images

Images are stored in

```
static/images/people/
```

Example:

```
static/images/people/
    burnetas.jpg
    economou.jpg
    placeholder.svg
```

If the `image` field is omitted, `placeholder.svg` is displayed automatically.

All images are displayed at a fixed size and cropped automatically to fit.

---

## Front matter

A typical member file is

```yaml
---
title: "Apostolos Burnetas"

given_name: "Apostolos"
last_name: "Burnetas"
sort_name: "Burnetas, Apostolos"

category: faculty

position: Professor
office: "Office 301"

summary: Professor of Operations Research.

image: burnetas.jpg

email: "aburnetas@math.uoa.gr"

website: "https://www.math.uoa.gr/~aburnetas"
scholar: "https://scholar.google.com/..."
orcid: "https://orcid.org/..."
github: "https://github.com/..."
linkedin: "https://www.linkedin.com/in/..."

interests:
  - Operations Research
  - Markov Decision Processes
  - Reinforcement Learning
  - Strategic Queueing
---
```

---

## Required fields

- `title`
- `given_name`
- `last_name`
- `sort_name`
- `category`
- `position`

---

## Recommended fields

- `summary`
- `interests`

---

## Optional fields

- `image`
- `office`
- `email`
- `website`
- `scholar`
- `orcid`
- `github`
- `linkedin`

All optional fields may be omitted.

---

## Categories

The supported categories are

- `faculty`
- `edip`
- `phd`
- `graduate`
- `visiting`
- `alumni`

Each category is displayed only if it contains at least one member.

---

## Ordering

Members are ordered alphabetically within each category using

```
sort_name
```

The convention is

```
LastName, GivenName
```

Examples

```
Burnetas, Apostolos
Economou, Antonis
Meligotsidou, Loukia
```

This field determines only the ordering of members.

---

## External links

Use the following conventions.

| Field | Value |
|------|------|
| email | Email address |
| website | Full HTTPS URL |
| scholar | Full Google Scholar URL |
| orcid | Full ORCID URL |
| github | Full GitHub profile URL |
| linkedin | Full LinkedIn profile URL |

---

## Interests

The `interests` field is free text.

Example

```yaml
interests:
  - Operations Research
  - Strategic Queueing
  - Reinforcement Learning
```

Interests are displayed exactly as entered.

They are **not** linked to research areas.

---

## Images

If

```yaml
image: burnetas.jpg
```

is specified, the image is loaded from

```
static/images/people/burnetas.jpg
```

If `image` is omitted, the site displays

```
static/images/people/placeholder.svg
```

---

## Design principles

The People section follows these principles.

- One Markdown file per person and language.
- One image file per person (optional).
- All external profile links are optional.
- All profile URLs are stored as complete HTTPS URLs.
- Categories are maintained manually.
- Members are ordered by `sort_name`.
- Interests are descriptive only and are not clickable.
- Person names are not clickable.

## Future extensions

Possible future enhancements include:

- Person detail pages.
- Automatic links from research areas to members.
- Filtering by category or research interest.
- Search by member name.
- Additional external profiles.
- Research area metadata separate from displayed interests.
