---
title: "Service"
linkTitle: "Service"
weight: 5
description: >
  Configure the services to monitor and what to notify/action when a new release is found.
---

## Using the GitHub API
services of type `github` will monitor the most recent `tag_name` that matches both your `regex_content` and `regex_version` at
https://api.github.com/repos/OWNER/REPO/releases.

It will go through each item in that list and try using `tag_name` as the version. It will run the `url_commands` on this version, check it with `regex_version` and then check the assets against `regex_content`. If all of these pass, that version will be used.

config.yml
```
service:
  ...
  # As many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_GITHUB_SERVICE:
    type: github                                  # The type of service to monitor. github/web
    url: OWNER/REPO                               # monitor https://github.com/OWNER/REPO
    url_commands:                                 # see the `url_commands` secion below for more info on this var
      - type: regex_submatch
        regex: ^v?([0-9.]+)$                      # Since the type is 'github', this searches the tag_names, so
                                                  # the '$' is used to ensure the tag name ends in this RegEx
                                                  # and doesn't just omit a '-beta' or similar details
    web_url: 'https://example.com/{{ version }}'  # Overrides URL in the Web UI and can be used in the notifiers
    interval: 1h5m                                # Query for a version change every 65 minutes.
                                                  # y=years, w=weeks, d=days, h=hours, m=minutes, s=seconds
    semantic_versioning: true                     # Whether to enforce semantic versioning on versions queried
                                                  # (url_commands can potentially be used to format it semantically
                                                  # - https://semver.org)
    regex_content: 'example-{{ version }}-amd64   # URL queried must contain this RegEx for the new version to be
                                                  # considered valid (notified)
                                                  # for services of type 'github', this RegEx runs against the
                                                  # version assets 'name' and 'browser_download_url'
    regex_version: ^[0-9.]+[0-9]$                 # Version found must match this RegEx to be considered valid
    use_prerelease: false                         # Whether a 'prerelease' tag (on GitHub) can be used.
    auto_approve: false                           # Whether approval is required for new versions in the Web UI,
                                                  # or whether WebHooks are automatically sent (required for their
                                                  # delay var to be used)
    ignore_misses false                           # Whether to log url_command failures, e.g. RegEx returned no
                                                  # matches
    access_token: GITHUB_ACCESS_TOKEN             # GitHub access token. Used when type is github. Useful when you
                                                  # want to exceed the public rate-limit, or want to query a private
                                                  # repo
    allow_invalid_certs: false                    # Whether invalid HTTPS certificates on the query site are
                                                  # accepted or not
    icon: https://example.com/icon.png            # Icon to use on the Web UI as well as all Slacks for this
                                                  # Service that have neither an icon_emoji, nor a icon_url
    gotify:                                       # Gotify notifiers for this Service
      EXAMPLE_GOTIFY_ID:
        message: 'overriding template'
    slack:                                        # Slack notifiers for this Service
      EXAMPLE_SLACK_ID:
        message: 'overriding template'
    webhook:                                      # WebHooks for this Service
      EXAMPLE_WEBHOOK_ID:
        secret: 'service-specific secret'
```
{{< alert title="Note" >}}
the number of `gotify`/`slack`/`webhook`'s you give the Service can be any number (you can even omit the var altogether).
For these vars, if you provide a var with the same ID as in the globals for that var, the options provided in the service will override those globals. e.g. if you have `slack.main` defined in the `config.yml` with all the required fields, but would like to override the message on some services for example, you can simply defined `slack.main.message` inside the service, and it will use that var in messages for this service.
{{< /alert >}}

## Monitor outside GitHub API
The following is how you'd define a service to be monitored without using the GitHub API.

config.yml
```
service:
  EXAMPLE_WEB_ID:
    type: web                                 # Regular URL, not GitHub API
    url: https://golang.org/dl/               # URL to monitor
    url_commands:                             # url_commands to grab the latest version number
      type: regex_submatch                    # RegEx type
      regex: go([0-9.]+[0-9]+)\.src\.tar\.gz  # RegEx to find the version. The most recent version download
                                              # is linked first
    semantic_versioning: false                # Disable because GoLang omits the PATCH number for some
                                              # versions, e.g. 1.18
    ...
```

## web_url
Without defining this var, the Web UI will link to `url`, but when this var has a value, the Web UI will link to that value.
You could, for example use [message templating](/docs/help/templating), to use all/some variation of the version in this URL and make it link to the sevices changelog (which you could also link to in your update notifiers).

## url_commands
`url_commands` are defined in a YAML list, with them being executed in order starting from the top.
There are (currently) four different types of url_command that can be performed:
- [regex](#regex)
- [regex_submatch](#regex_submatch)
- [replace](#replace)
- [split](#split)

{{< alert title="Note" >}}
For a service of type 'github', 'url_commands' will run against every release 'tag_name' until a version is found that matches no 'url_command' fails on and both 'regex_version' and 'regex_content' pass. ('regex_content' acts on every assets 'name' and 'browser_download_url')
{{< /alert >}}

#### regex
```
service:
  example:
    ...
    url_commands:
      - type: regex
        regex: ' [0-9.]+ '
        index: -1
        ignore_misses: false  # Whether to log on regex matching fails
```
{{< alert title="Note" >}}
`index` of -1 will return the last match of the RegEx supplied.
{{< /alert >}}

#### regex_submatch
```
service:
  example:
    ...
    url_commands:
      - type: regex_submatch
        regex: v([0-9.]+)$
        ignore_misses: false  # Whether to log on RegEx matching fails
```
{{< alert title="Note" >}}
This RegEx will only work on the first match, so `regex_submatch` of 'v([0-9])' and text of `v1 v2 v3...`
will always return 1. The above RegEx is useful in places where they use the `v` prefix
in their versions. Removing that helps in the majority of cases to make it follow semantic
versioning.
{{< /alert >}}

#### replace
```
service:
  example:
    ...
    url_commands:
      - type: replace
        old: v
        new: ''
```
This command replaces `old` with `new`.

#### split
```
service:
  example:
    ...
    url_commands:
      - type: split
        text: test
        index: 0
        ignore_misses: false  # Whether to log on invalid index uses

```
This command will split on `text` and return `index` of that split. e.g. running this command
on '1.2.3test' with the above vars would return `1.2.3`
