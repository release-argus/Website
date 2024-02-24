---
title: "Notify"
linkTitle: "Notify"
weight: 3
description: >
  Configure global notifiers that can be used by any Service.
---

We are running [Shoutrrr 0.8](https://containrrr.dev/shoutrrr/v0.8/services/overview/), which supports sending notifications to the services listed below.

The 'URL Fields' go under `url_fields` and 'Query/Param Props' go under `params`.

Below I have used capitals in some key naming for better readability (ignore that Argus will convert that to lower-case as it shouldn't break anything).

The lines prefixed with a # are optional vars that aren't required.

Environment variables in the format '${ENV_VAR}' can be used in the `options.*`, `params.*` and `url_fields.*` fields.

The config for each of these is layed out in the format of
```yaml
notify:
  ...
  # As many of these (below) as you like, just ensure they have unique ID's
  EXAMPLE_NOTIFY_ID:
    type: shoutrrr
    options:
#     delay: AhBmCs                             # Time to wait when a new release is found before sending this message
#     max_tries: 3                              # Maximum number of times to try sending this message until a send is successful
#     message: >-                               # Message template
#       <{{ service_url }}|{{ service_id }}> -
#       {{ version }} released{% if web_url %}
#       (<{{ web_url }}|changelog>){% endif %}
    url_fields: {}
    params: {}
```
where the vars available in `options` stay the same every time (so they'll be ommitted below).

#### message templating

The `message` used in the Notify messages can be customised with Django-style templating courtesy of [pongo2](https://www.schlachter.tech/solutions/pongo2-template-engine/).

The default message template is
`<{{ service_url }}|{{ service_id }}> - {{ version }} released{% if web_url %} (<{{ web_url }}|changelog>){% endif %}`
, which with a `service_id` of 'example_service', a `service_url` of 'example.com', no `web_url` and the `version` that triggered
this message being '1.2.3', woud trigger a message of `example_service - 1.2.3 released` (example_service would be a clickable link to the service_url). If the Service had a `web_url` defined, then ' (changelog)' would appear at the end, where the 'changelog' text would be a clickable link to that `web_url`.

The vars that can be used in both the title and message templates are:
- service_id
- service_url
- web_url
- version

The default Slack message format assumes that when `web_url` is defined, it is a link to the services changelog. Then, in any notifications by Argus about that service,
you should get a clickable 'changelog' button that takes you to that `web_url`.


For further guidance and other helpful examples on the templating used, start by looking [here](/docs/help/templating).


{{< tabpane text=true right=true >}}
  {{% tab header="**types**:" disabled=true /%}}
  {{% tab header="bark" %}}
[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/bark/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: bark
    url_fields:
      DeviceKey: DEVICE_KEY
      Host: your_host
#     Path: /foo
#   params:
#     Badge: 0                           # Number displayed next to App icon
#     Copy: foo                          # Value to be copied
#     Group: admins                      # Group of the notification
#     Icon: https://example.com/icon.png # URL to an image
#     Scheme: http/https                 # Server protocol
#     Sound: ''                          # Fromm https://github.com/Finb/Bark/tree/master/Sounds
#     Title: Release                     # Notification title
#     URL: https://example.com           # URL to open when notification is tapped
```
  {{% /tab %}}
  {{% tab header="discord" %}}
- As of writing, Discord Webhooks are in the format of `https://discord.com/api/webhooks/WEBHOOK_ID/TOKEN` (`<Server Settings> - <Integrations> - <Webhooks>`)

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/discord/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: discord
    url_fields:
      Token: TOKEN
      WebhookID: WEBHOOK_ID
#   params:
#     Avatar: https://release-argus.io/favicons/android-chrome-512x512.png
#     Title: Release
#     Username: Argus
```
  {{% /tab %}}
  {{% tab header="smtp" %}}
- email notifications

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/email/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: smtp
    url_fields:
#     Username: NAME
#     Password: PASSWORD123
      Host: your_host
#     Port: 25
    params:
#     FromName: Argus
      FromAddress: foo@bar.com
      ToAddresses: name@you.com,other@you.com
#     Auth: Plain                         # None/Plain/CRAMMD5/Unknown/OAuth2
#     Subject: '{{ service_id }} Release'
#     UseHTML: no
#     UseStartTLS: yes
```
  {{% /tab %}}
  {{% tab header="googlechat" %}}
- Example Google Chat incoming Webhook URL `https://chat.googleapis.com/v1/spaces/ FOO /messages?key= bar &token= baz`

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/googlechat/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: googlechat
    url_fields:
      Raw: chat.googleapis.com/v1/spaces/FOO/messages?key=bar&token=baz
```
  {{% /tab %}}
  {{% tab header="gotify" %}}
- Create an application on the Gotify Web UI, and use that URL with the token for the application you make.

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/email/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: gotify
    url_fields:
      Host: gotify.example.io
#     Port: 443
#     Path: sub/path          # e.g. for gotify.example.io/sub/path
      Token: TOKEN
#   params:
#     DisableTLS: no
#     Priority: 0
#     Title: Argus
```
  {{% /tab %}}
  {{% tab header="ifttt" %}}
[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/ifttt/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: ifttt
    url_fields:
      WebhookID: WEBHOOK_ID
    params:
      Events: event1,event2,...
#     Title: Argus
#     UseMessageAsValue: 2
#     UseTitleAsValue: 0
#     Value1:
#     Value2:
#     Value3:
```
  {{% /tab %}}
  {{% tab header="join" %}}
- Go to the [Join Webapp](https://joinjoaomgcd.appspot.com/)
- Select your device
- Click **Join API**
- Your `deviceId` is shown in the top
- Click **Show** next to `API Key` to see your key

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/join/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: join
    url_fields:
      APIKey: KEY
    params:
      Devices: ID1,ID2,...
#     Icon: https://release-argus.io/favicons/android-chrome-512x512.png
#     Title: Argus
```
  {{% /tab %}}
  {{% tab header="mattermost" %}}
- Go to the menu in the top left and navgate to `<Integrations> - <Incoming Webhooks>`.

Example Mattermost Webhook - `https://mattermost.example.io/hooks/TOKEN`

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/mattermost/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: mattermost
    url_fields:
#     Username: Argus
      Host: mattermost.example.io
#     Port: 443
#     Path: sub/path              # e.g. for mattermost.example.io/sub/path/hooks/TOKEN
      Token: TOKEN
#     Channel: releases
#   params:
#     Icon: https://release-argus.io/favicons/android-chrome-512x512.png # url/emoji
```
  {{% /tab %}}
  {{% tab header="ntfy" %}}
[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/ntfy/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: ntfy
    url_fields:
#     Username: Argus
#     Password: PASS123
      Host: mattermost.example.io
#     Port: 443
      Topic: Release123
#   params:
#     Actions: ''                        # see https://docs.ntfy.sh/publish/#action-buttons
#     Attach: https://example.com        # URL of an attachment
#     Cache: yes                         # Cache messages
#     Click: https://example.com         # URL to open when notification is clicked
#     Email: name@example                # Email address for email notifications
#     Filename: ''                       # Filename for attachment
#     Firebase: yes                      # Send to Firebase
#     Icon: https://example.com/icon.png # URL to an icon
#     Priority: default                  # Priority of the notification
#     Scheme: https                      # Server protocol
#     Tags: ''                           # Comma separated list of tags that may/may not map to eemojis
#     Title: Argus                       # Title of the notification
```
  {{% /tab %}}
  {{% tab header="matrix" %}}
[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/matrix/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: matrix
    url_fields:
#     User: NAME
      Password: PASS123
      Host: matrix.example.io
#     Port: 443
#   params:
#     DisableTLS: no
#     Rooms: '!ROOM_ID,ALIAS' # Needs the quotes as `!` is a YAML special character
#     Title: Argus
```
  {{% /tab %}}
  {{% tab header="opsgenie" %}}
Go to `<Settings> - <Integrastion List> - <API>`

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/opsgenie/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: opsgenie
    url_fields:
#     Host: api.opsgenie.com # e.g. api.eu.opsgenie.com for EU instances
#     Port: 443
      APIKey: API_KEY
#   params:
#     Actions: 'An action'                        # Custom actions that will be available for the alert
#     Alias: 'alias for the alert'                # Client-defined identifier of the alert
#     Description: 'Argus found a new release'    # Description field of the alert
#     Details: '{"key1": "val1", "key2": "val2"}' # Map of key-value pairs to use as custom properties of the alert
#     Entity: 'Example entity'                    # Entity field of the alert that is generally used to specify which
#                                                 # domain the Source field of the alert
#     Note: 'Note on Argus'                       # Additional note that will be added while creating the alert
#     Priority: 'P1'                              # Priority level of the alert. Possible values are P1/P2/P3/P4/P5
#     Responders: '[{"id":"4513b7ea-3b91-438f-b7e4-e3e54af9147c","type":"team"},{"name":"NOC","type":"team"}]'
#                                                 # Teams, users, escalations and schedules that the alert will be
#                                                 # routed to send
#     Source: 'Argus'                             # Source field of the alert
#     Tags: 'argus othertag'                      # Tags of the alert
#     Title: 'Argus'                              # Notification title, optionally set by the sender
#     User: 'Argus'                               # Display name of the request owner
#     VisibleTo: '[{"id":"4513b7ea-3b91-438f-b7e4-e3e54af9147c","type":"team"},{"name":"rocket_team","type":"team"}]'
#                                                 # Teams and users that the alert will become visible to without
#                                                 # sending any notification
```
  {{% /tab %}}
  {{% tab header="pushbullet" %}}
- Get your `token` by creating an **Access Token** at https://www.pushbullet.com/#settings/account
- Get your `targets` by going to https://www.pushbullet.com/#devices. Click on the device on the left pane and the URL should change to https://www.pushbullet.com/#devices/\<TARGET\>

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/pushbullet/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: pushbullet
    url_fields:
      Token: ACCESS_TOKEN
      Targets: DEVICE1,DEVICE2
#   params:
#     Title: Argus
```
  {{% /tab %}}
  {{% tab header="pushover" %}}
To get your \<User Key\>, go to your [Pushover dashboard](https://pushover.net/), and view it at the top right.

You can also make an Application specific for Argus by clicking 'Create an Application/API Token' at the bottom of that dashboard

In the device list, the Name column is the field used to refer to your devices

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/pushover/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: pushover
    url_fields:
      Token: TOKEN
      User: USER_KEY # User Key OR API Token/Key
#   params:
#     Devices: 'device1,device2'
#     Priority: 0
#     Title: Argus
```
  {{% /tab %}}
  {{% tab header="rocketchat" %}}
Example URL `username@host:port/TOKEN_A/TOKEN_B/CHANNEL`

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/rocketchat/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: rocketchat
    url_fields:
#     Username: foo
      Host: rocketchat.example.io
#     Port: 443
#     Path: sub/path          # e.g. for rocketchat.example.io/sub/path
      TokenA: TOKEN_A
      TokenB: TOKEN_B
      Channel: CHANNEL
```
  {{% /tab %}}
  {{% tab header="slack" %}}
Either a Webhook, or the Slack Bot API can be used.

To get a token for a Bot:
- Create an App for your bot with Slacks '[Creating an app](https://api.slack.com/authentication/basics#creating)' guide.
- Install the App into your workspace ([Slack docs](https://api.slack.com/authentication/basics#installing)).
- From [Apps](https://api.slack.com/apps), select your App and go to 'OAuth & Permissions'
- Copy the 'Bot User OAuth Token'
- e.g. `xoxb:012345678901-0123456789012-phq24qfrhqnm2mtmz54dwkop

To use Webhooks:
- Create an App for your bot with Slacks '[Creating an app](https://api.slack.com/authentication/basics#creating)' guide.
- Enable incoming Webhooks for that app ([Slack docs](https://api.slack.com/messaging/webhooks#enable_webhooks))
- Create an incoming Webhook ([Slack docs](https://api.slack.com/messaging/webhooks#create_a_webhook))
- e.g. https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX
- becomes 'hook:T00000000-B00000000-XXXXXXXXXXXXXXXXXXXXXXXX

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/slack/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: slack
    url_fields:
      Token: 'xoxb:012345678901-0123456789012-phq24qfrhqnm2mtmz54dwkop'
      Channel: argus
#   params:
#     BotName: Argus
#     Color: ''                                                          # Message left-hand border color
#     Icon: https://release-argus.io/favicons/android-chrome-512x512.png # URL or Emoji to use
#     Title: Release                                                     # Prepended text above the message
```
  {{% /tab %}}
  {{% tab header="teams" %}}
- Create an 'Incoming Webhook' following '[Microsoft's docs](https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook#create-an-incoming-webhook-1)'.

Example URL `https://<organization>.webhook.office.com/webhookb2/<Group>@<Tenant>/IncomingWebhook/<AltId>/<GroupOwner>`

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/teams/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: teams
    url_fields:
      Group: GROUP
      Tenant: TENANT
      AltID: ALT_ID
      GroupOwner: GROUP_OWNER
    params:
#     Color: ''
      Host: example.webhook.office.com
#     Title: ''
```
  {{% /tab %}}
  {{% tab header="telegram" %}}
- To get a token, [talk to the botfather](https://core.telegram.org/bots#6-botfather).
- Chats
    - `@channel-name` can only be used for public channels. The name can be found by going to `Channel info`, just replace the `t.me/` prefix with an `@`.
    - `chat_id` is required for private channels/group chats/private chats. To get the ID, you can forward a message (from the target chat) to [@UserInfoBot](https://t.me/userinfobot) or [@JsonDumpBot](https://t.me/jsondumpbot) and view it at `Id` and `message.forward_from_chat.id` from those bots respectively.
    - (The above bots are created and hosted by [@nadam](https://github.com/nadam) and their sources are available to view at [nadam/userinfobot](https://github.com/nadam/userinfobot) and [nadam/jsondumpbot](https://github.com/nadam/jsondumpbot))

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/telegram/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: telegram
    url_fields:
      Token: TOKEN_FROM_BOTFATHER
    params:
      Chats: '@channel-name,@other-channel,chat_id'
#     Notification: yes                     # No will send the message silently
#     ParseMode: None                       # None/Markdown/HTML/MarkdownV2
#     Preview: yes                          # Whether to display web page previews for URLs in the message
#     Title: Argus
```
  {{% /tab %}}
  {{% tab header="zulip" %}}
Zulip Chat

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/zulip/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: zulip
    url_fields:
      BotMail: bot@release-argus.io
      BotKey: BOT_KEY
      Host: zulip.example.io
#   params:
#     Stream: 'mystream'
#     Topic: 'Argus'
```
  {{% /tab %}}
  {{% tab header="shoutrrr" %}}
`Shoutrrr` will expect the full Shoutrrr URL and so could be used for any of the services in the Shoutrrr v0.6 docs. Any params specified in `params` will also be passed.

[Shoutrrr docs](https://containrrr.dev/shoutrrr/v0.7/services/overview/)

```yaml
notify:
  ...
  # as many of these (below) as you like, just ensure they have unique ID's.
  EXAMPLE_NOTIFY:
    type: shoutrrr
    url_fields:
      raw: matrix://user:password@host:port/?rooms=!roomID1,roomAlias2&disableTLS=yes
```
  {{% /tab %}}
{{% /tabpane %}}
