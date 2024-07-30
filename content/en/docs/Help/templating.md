---
title: "Templating"
linkTitle: "Templating"
weight: 4
description: >
  Help with pongo2 templating
---

### Using vars
To use one of the vars available to you, enclose it in double curly brackets, e.g. to use the `version` var, you must use `{{ version }}`


### Basic changelog through web_url
Changelogs sometimes include some variation of the version, or the full version in. If the changelog for the release of version `1.2.3` of `example` is hosted at `https://example.com/changelog/1.2.3`, you could use a web_url of `https://example.com/changelog/{{ version }}` to direct to that changelog.


### Stripping parts of the version for the changelog used in web_url
We'll use [Authentik](https://goauthentik.io) (awesome app!) as an example for this changelog. Authentik, versions are (as of writing), in the style of `YEAR.MONTH.PATCH` with the changelogs found at `https://goauthentik.io/docs/releases/YEAR.MONTH`. To get our web_url to strip the `.PATCH` out, we need to run filters on the `version` var.
`https://goauthentik.io/docs/releases/{{ version | split:"." | slice:":-1" | join:"."  }}` as the web_url would produce a link to the correct changelog.

Other places may use all, or some of the version, but change the delimiter from a `.` to a `-` for example. To handle those cases, Use `{{ version | split:"." | join:"-"  }}` as the var.
