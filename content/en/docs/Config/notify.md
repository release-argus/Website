---
title: "Notify"
linkTitle: "Notify"
weight: 3
description: >
  Configure global notifiers that can be used by any Service.
---

We are running [Shoutrrr 0.25](https://shoutrrr.nickfedor.com/v0.25.0/services/overview/), which supports sending notifications to the services listed below.

The 'URL Fields' go under `url_fields` and 'Query/Param Props' go under `params`.

Below I have used capitals in some key naming for better readability (ignore that Argus will convert that to lower-case as it shouldn't break anything).

The lines prefixed with a # are optional vars that aren't required.

{{< alert title="Note" >}}
Environment variables in the format '${ENV_VAR}' can be used in the `options.*`, `params.*` and `url_fields.*` fields.
{{< /alert >}}

{{% alert title="Renamed params" %}}
For Shoutrrr 0.25, Argus renames these when it loads the config, logging a deprecation warning for each:

- bark: `Scheme` becomes `DisableTLS` (`http` = `yes`, `https` = `no`)
- gotify: `InsecureSkipVerify` becomes `SkipTLSVerify`
- matrix: `url_fields.Username` becomes `User`
- ntfy: `DisableTLS` and `DisableTLSVerification` become `SkipTLSVerify`, as ntfy's `DisableTLS` meant skipping TLS verification
- ntfy: `Scheme` becomes `DisableTLS_` (`http` = `yes`, `https` = `no`), which the next major release renames to `DisableTLS`
- opsgenie: `Priority` `1`-`5` becomes `P1`-`P5`
- gotify: `Date` is removed, as Gotify sets the date of each message
{{% /alert %}}

{{< alert title="Note" >}}
When the config loads, Argus logs a warning for a notifier whose URL Shoutrrr rejects, and that notifier fails each send. It also logs any `params` the notifier's type does not support, and leaves them out of each send.
{{< /alert >}}

The config for each of these is laid out in the format of
```yaml
notify:
  ...
  # As many of these (below) as you like, just ensure they have unique IDs
  EXAMPLE_NOTIFY_ID:
    type: gotify
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
where the vars available in `options` stay the same every time (so they'll be omitted below).

#### message templating

The `message` used in the Notify messages can be customised with Django-style templating courtesy of [pongo2](https://www.schlachter.tech/solutions/pongo2-template-engine/).

The default message template is
`<{{ service_url }}|{{ service_id }}> - {{ version }} released{% if web_url %} (<{{ web_url }}|changelog>){% endif %}`
, which with a `service_id` of 'example_service', a `service_url` of 'example.com', no `web_url` and the `version` that triggered
this message being '1.2.3', would trigger a message of `example_service - 1.2.3 released` (example_service would be a clickable link to the service_url). If the Service had a `web_url` defined, then ' (changelog)' would appear at the end, where the 'changelog' text would be a clickable link to that `web_url`.

The vars that can be used in both the title and message templates are:
- `service_id`
- `service_name`
- `service_url`
- `icon`
- `icon_link_to`
- `web_url`
- `latest_version`
- `approved_version`
- `deployed_version`
- `tags` (e.g. `tags | join','` - use `tags.0` for first element, `tags.1` for seconds, etc.)

The default Slack message format assumes that when `web_url` is defined, it is a link to the services changelog. Then, in any notifications by Argus about that service,
you should get a clickable 'changelog' button that takes you to that `web_url`.


For further guidance and other helpful examples on the templating used, start by looking [here](/docs/help/templating).


{{< tabpane text=true right=true >}}
  {{% tab header="**types**:" disabled=true /%}}
  {{% tab header="bark" %}}
  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/bark/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: bark
      url_fields:
        DeviceKey: DEVICE_KEY
        Host: your_host
  #     Port: 443
  #     Path: sub/path                     # e.g. for your_host/sub/path
  #   params:
  #     Badge: 0                           # Number displayed next to App icon
  #     Copy: foo                          # Value to be copied
  #     DisableTLS: no                     # Send over HTTP instead of HTTPS
  #     Group: admins                      # Group of the notification
  #     Icon: https://example.com/icon.png # URL to an image
  #     SkipTLSVerify: no                  # Whether to skip TLS certificate verification
  #     Sound: ''                          # From https://github.com/Finb/Bark/tree/master/Sounds
  #     Title: Release                     # Notification title
  #     URL: https://example.com           # URL to open when notification is tapped
  ```
  {{% /tab %}}
  {{% tab header="discord" %}}
  - As of writing, Discord Webhooks are in the format of `https://discord.com/api/webhooks/WEBHOOK_ID/TOKEN` (`<Server Settings> - <Integrations> - <Webhooks>`)
  - Mentions in the message (`@everyone`, `@here`, `<@&ROLE_ID>`) only ping when listed in `Mentions`.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/discord/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: discord
      url_fields:
        Token: TOKEN
        WebhookID: WEBHOOK_ID
  #   params:
  #     Avatar: https://release-argus.io/favicons/android-chrome-512x512.png
  #                          # Override the webhook default avatar with specified URL
  #     Color: 0x50D9ff      # The color of the left border for plain messages
  #     ColorDebug: 0x7b00ab # The color of the left border for debug messages
  #     ColorError: 0xd60510 # The color of the left border for error messages
  #     ColorInfo: 0x2488ff  # The color of the left border for info messages
  #     ColorWarn: 0xffc441  # The color of the left border for warning messages
  #     JSON: no             # Whether to send the whole message as the JSON payload instead of using it as the 'content' field
  #     Mentions: ''         # Comma separated mentions that ping - everyone/here/user:ID/role:ID
  #     SplitLines: yes      # Whether to send each line as a separate embedded item
  #     Thread_ID: ''        # The thread ID to send the message to
  #     Thread_Name: ''      # Name of a thread to create for each message in a forum or media channel
  #     Title: Release       # Notification title
  #     Username: Argus      # Override the webhook default username
  ```
  {{% /tab %}}
  {{% tab header="generic" %}}
  - Post to any HTTP endpoint.
  - `Headers`, `JSON_Payload_Vars` and `Query_Vars` each take a JSON map as a string.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/specialized/generic/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: generic
      url_fields:
  #     Username: argus                           # For HTTP Basic Auth
  #     Password: PASS123                         # For HTTP Basic Auth
        Host: generic.example.io
  #     Port: 443
  #     Path: sub/path                            # e.g. for generic.example.io/sub/path
  #     Headers: '{"X-Example": "foo"}'           # Additional HTTP headers
  #     JSON_Payload_Vars: '{"key": "value"}'     # Extra keys to add to the JSON payload
  #     Query_Vars: '{"foo": "bar"}'              # Extra query parameters (prefix a reserved name with an underscore)
  #   params:
  #     ContentType: application/json             # Value of the Content-Type header
  #     DisableTLS: no                            # Send over HTTP instead of HTTPS
  #     MessageKey: message                       # Payload key to put the message under
  #     Method: POST                              # HTTP request method - CONNECT/DELETE/GET/HEAD/OPTIONS/POST/PUT/TRACE
  #     SkipTLSVerify: no                         # Whether to skip TLS certificate verification
  #     Template: ''                              # Template used to build the request payload
  #     Title: Argus                              # Notification title
  #     TitleKey: title                           # Payload key to put the title under
  ```
  {{% /tab %}}
  {{% tab header="googlechat" %}}
  - Example Google Chat incoming Webhook URL `https://chat.googleapis.com/v1/spaces/ FOO /messages?key= bar &token= baz`

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/googlechat/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: googlechat
      url_fields:
        Raw: chat.googleapis.com/v1/spaces/FOO/messages?key=bar&token=baz
  ```
  {{% /tab %}}
  {{% tab header="gotify" %}}
  - Create an application on the Gotify Web UI, and use that URL with the token for the application you make.
  - The token must be an application token (15 characters starting with `A`, or starting with `gtfya.`). Client tokens are rejected when the config loads.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/gotify/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: gotify
      url_fields:
        Host: gotify.example.io
  #     Port: 443
  #     Path: sub/path          # e.g. for gotify.example.io/sub/path
        Token: TOKEN
  #   params:
  #     Extras: ''
  #     DisableTLS: no          # Send over HTTP instead of HTTPS
  #     Priority: 0
  #     SkipTLSVerify: no       # Whether to skip TLS certificate verification
  #     Title: Argus            # Notification title
  #     UseHeader: yes          # Send the token in the X-Gotify-Key header instead of the URL query
  ```
  {{% /tab %}}
  {{% tab header="homeassistant" %}}
  - Create a **Long-Lived Access Token** at the bottom of your Home Assistant profile page (`<Profile> - <Security>`).

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/homeassistant/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: homeassistant
      url_fields:
        Host: homeassistant.example.io
  #     Port: 443
  #     Path: sub/path                 # e.g. for homeassistant.example.io/sub/path
        Token: TOKEN                   # Long-lived access token
  #   params:
  #     DisableTLS: no                 # Send over HTTP instead of HTTPS
  #     NID: ''                        # Persistent notification ID - overwrites any notification with that ID
  #     Service: ''                    # Action to call, e.g. 'notify.mobile_app_phone' (empty creates a persistent notification)
  #     SkipTLSVerify: no              # Whether to skip TLS certificate verification
  #     Targets: ''                    # Comma separated list of notify targets, e.g. device1,device2
  #     Title: Argus                   # Notification title
  ```
  {{% /tab %}}
  {{% tab header="ifttt" %}}
  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/ifttt/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: ifttt
      url_fields:
        WebhookID: WEBHOOK_ID
      params:
        Events: event1,event2,...
  #     Title: Argus              # Notification title
  #     MessageValue: 2           # Value field (1-3) to set to the notification message
  #     TitleValue: 0             # Value field (0-3) to set to the notification title (0 = None)
  #     Value1: ''
  #     Value2: ''
  #     Value3: ''
  ```
  {{% /tab %}}
  {{% tab header="join" %}}
  - Go to the [Join Webapp](https://joinjoaomgcd.appspot.com/)
  - Select your device
  - Click **Join API**
  - Your `deviceId` is shown in the top
  - Click **Show** next to `API Key` to see your key

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/join/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: join
      url_fields:
        APIKey: KEY
      params:
        Devices: ID1,ID2,... # Comma separated list of device IDs
  #     Icon: https://release-argus.io/favicons/android-chrome-512x512.png
  #                          # Icon URL
  #     Title: Argus         # Notification title
  ```
  {{% /tab %}}
  {{% tab header="lark" %}}
  - Add a **Custom Bot** to your Lark/Feishu group, and copy the token from the end of its webhook URL (`.../open-apis/bot/v2/hook/TOKEN`).

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/lark/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: lark
      url_fields:
  #     Host: open.larksuite.com  # Use open.feishu.cn for Feishu
        Token: TOKEN
  #   params:
  #     Link: https://example.com # URL to link to from the message
  #     Secret: ''                # Custom bot secret, for signature verification
  #     Title: Argus              # Message title
  ```
  {{% /tab %}}
  {{% tab header="matrix" %}}
  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/matrix/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: matrix
      url_fields:
  #     User: NAME
        Password: PASS123
        Host: matrix.example.io
  #     Port: 443
  #   params:
  #     DisableTLS: no          # Send over HTTP instead of HTTPS
  #     Rooms: '!ROOM_ID,ALIAS' # Room aliases, or with ! prefix, room IDs (Needs the quotes as `!` is a YAML special character)
  #     SkipTLSVerify: no       # Whether to skip TLS certificate verification
  #     Title: Argus            # Notification title
  ```
  {{% /tab %}}
  {{% tab header="mattermost" %}}
  - Go to the menu in the top left and navigate to `<Integrations> - <Incoming Webhooks>`.

  Example Mattermost Webhook - `https://mattermost.example.io/hooks/TOKEN`

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/mattermost/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: mattermost
      url_fields:
  #     Username: Argus
        Host: mattermost.example.io
  #     Port: 443
        Token: TOKEN
  #     Channel: releases
  #   params:
  #     DisableTLS: no    # Send over HTTP instead of HTTPS
  #     Icon: https://release-argus.io/favicons/android-chrome-512x512.png
  #                       # Use emoji or URL as icon (based on presence of http(s):// prefix)
  #     SkipTLSVerify: no # Whether to skip TLS certificate verification
  #     Title: Argus      # Notification title
  ```
  {{% /tab %}}
  {{% tab header="mqtt" %}}
  - Publishes to an MQTT 5 broker, over TLS unless `DisableTLS: yes` is set.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/mqtt/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: mqtt
      url_fields:
  #     Username: argus
  #     Password: PASS123
        Host: mqtt.example.io
  #     Port: 1883        # Defaults to 8883 with TLS, 1883 without
        Topic: argus/releases
  #   params:
  #     CleanSession: yes # Start with a clean session
  #     ClientID: ''      # MQTT client identifier (default: 'shoutrrr' and 12 random characters)
  #     DisableTLS: no    # Connect without TLS
  #     QoS: AtMostOnce   # Quality of Service level - AtMostOnce/AtLeastOnce/ExactlyOnce (or 0/1/2)
  #     Retained: no      # Retain the message on the broker
  #     SkipTLSVerify: no # Whether to skip TLS certificate verification
  ```
  {{% /tab %}}
  {{% tab header="notifiarr" %}}
  - Get your API key from your [Notifiarr account](https://notifiarr.com/) settings page.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/notifiarr/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: notifiarr
      url_fields:
        APIKey: API_KEY
  #   params:
  #     Name: Argus                              # App/script name shown in the notification
  #     Channel: '123456789'                     # Discord channel ID for the notification
  #     Thumbnail: https://example.com/thumb.png # Thumbnail URL for the embed
  #     Image: https://example.com/image.png     # Image URL for the embed
  #     Color: 50D9ff                            # Color for the embed
  ```
  {{% /tab %}}
  {{% tab header="ntfy" %}}
  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/ntfy/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: ntfy
      url_fields:
  #     Username: Argus
  #     Password: PASS123
        Host: ntfy.example.io
  #     Port: 443
        Topic: Release123
  #   params:
  #     Actions: ''                        # see https://docs.ntfy.sh/publish/#action-buttons
  #     Attach: https://example.com        # URL of an attachment
  #     Cache: yes                         # Cache messages
  #     Click: https://example.com         # Website opened when notification is clicked
  #     DisableTLS_: no                    # Send over HTTP instead of HTTPS (renamed DisableTLS in the next major release)
  #     Email: name@example                # Email address for email notifications
  #     Filename: ''                       # Filename for attachment
  #     Firebase: yes                      # Send to Firebase
  #     Icon: https://example.com/icon.png # URL to an icon
  #     Markdown: no                       # Enable Markdown formatting
  #     Priority: default                  # Priority of the notification - min/low/default/high/max
  #     SkipTLSVerify: no                  # Whether to skip TLS certificate verification
  #     Tags: ''                           # Comma separated list of tags that may/may not map to emojis
  #     Title: Argus                       # Notification title
  #     Token: ''                          # Access token for Bearer auth (e.g. 'tk_xxxxxxxx'), takes precedence over Username/Password
  ```
  {{% /tab %}}
  {{% tab header="opsgenie" %}}
  Go to `<Settings> - <Integration List> - <API>`

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/incident/opsgenie/)

  ```yaml
  notify:
    ...
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
  #     Title: 'Argus'                              # Notification title
  #     User: 'Argus'                               # Display name of the request owner
  #     VisibleTo: '[{"id":"4513b7ea-3b91-438f-b7e4-e3e54af9147c","type":"team"},{"name":"rocket_team","type":"team"}]'
  #                                                 # Teams and users that the alert will become visible to without
  #                                                 # sending any notification
  ```
  {{% /tab %}}
  {{% tab header="pagerduty" %}}
  - Add an **Events API v2** integration to a PagerDuty service, and copy its integration key (32 hex characters).

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/incident/pagerduty/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: pagerduty
      url_fields:
  #     Host: events.pagerduty.com
  #     Port: 443
        IntegrationKey: 0123456789abcdef0123456789abcdef
  #   params:
  #     Action: trigger             # Type of event - trigger/acknowledge/resolve
  #     Client: Argus               # Name of the monitoring client triggering this event
  #     Client_URL: ''              # URL of the monitoring client triggering this event
  #     Contexts: ''                # Context links/images, e.g. 'link:https://example.com,image:https://example.com/i.png'
  #     Dedup_Key: ''               # Identifies the alert, at most 255 characters (required for acknowledge/resolve)
  #     Details: '{"key": "value"}' # Additional details about the incident, as a JSON object
  #     Severity: error             # Severity of the event - critical/error/warning/info
  #     Source: default             # Location of the affected system, preferably a hostname or FQDN
  #     Title: Argus                # Sent as the event summary
  ```
  {{% /tab %}}
  {{% tab header="pushbullet" %}}
  - Get your `token` by creating an **Access Token** at https://www.pushbullet.com/#settings/account
  - Get your `targets` by going to https://www.pushbullet.com/#devices. Click on the device on the left pane and the URL should change to https://www.pushbullet.com/#devices/\<TARGET\>

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/pushbullet/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: pushbullet
      url_fields:
        Token: ACCESS_TOKEN
        Targets: DEVICE1,DEVICE2
  #   params:
  #     Title: Argus # Notification title
  ```
  {{% /tab %}}
  {{% tab header="pushover" %}}
  To get your \<User Key\>, go to your [Pushover dashboard](https://pushover.net/), and view it at the top right.

  You can also make an Application specific for Argus by clicking 'Create an Application/API Token' at the bottom of that dashboard

  In the device list, the Name column is the field used to refer to your devices

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/pushover/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: pushover
      url_fields:
        Token: TOKEN
        User: USER_KEY # User Key OR API Token/Key
  #   params:
  #     Devices: 'device1,device2'
  #     EncryptionKey: ''          # 256-bit AES key as 64 hex characters (matching the key set in the Pushover app),
  #                                # to end-to-end encrypt the title and message
  #     Expire: 1h                 # How long an emergency (Priority: 2) notification repeats, at most 3h
  #     Priority: 0                # Message priority from -2 (lowest) to 2 (emergency)
  #     Retry: 15m                 # Time between repeats of an emergency (Priority: 2) notification, at least 30s
  #     Timestamp: 0               # Unix time in seconds to show instead of when Pushover received the message
  #     Title: Argus               # Notification title
  ```
  {{% /tab %}}
  {{% tab header="rocketchat" %}}
  Example URL `username@host:port/TOKEN_A/TOKEN_B[/CHANNEL]`

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/rocketchat/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: rocketchat
      url_fields:
  #     Username: foo
        Host: rocketchat.example.io
  #     Port: 443
        TokenA: TOKEN_A
        TokenB: TOKEN_B
  #     Channel: CHANNEL  # Defaults to the webhook's channel
  #   params:
  #     DisableTLS: no    # Send over HTTP instead of HTTPS
  #     SkipTLSVerify: no # Whether to skip TLS certificate verification
  ```
  {{% /tab %}}
  {{% tab header="shoutrrr" %}}
  Pass a raw [Shoutrrr URL](https://shoutrrr.nickfedor.com/v0.25.0/services/overview/) through directly. Use this for any service that doesn't have its own tab above (e.g. `xmpps://` for XMPP over implicit TLS), or when you'd rather build the URL yourself.

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: shoutrrr
      url_fields:
        Raw: 'slack://TOKEN@CHANNEL'  # The full Shoutrrr URL to send to
  ```
  {{% /tab %}}
  {{% tab header="signal" %}}
  - Sends through a [signal-cli-rest-api](https://github.com/bbernhard/signal-cli-rest-api) server.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/signal/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: signal
      url_fields:
  #     User: argus                          # For HTTP Basic Auth
  #     Password: PASS123                    # For HTTP Basic Auth
        Host: signal-api.example.io
  #     Port: 8080
        Source: '+15551234567'               # Source phone number, with country code
        Recipients: '+15557654321,group.abc' # Comma separated phone numbers, group IDs, or u:usernames
  #   params:
  #     Attachments: ''                      # Comma separated raw base64 attachments; a data: value is one URI
  #     DisableTLS: no                       # Send over HTTP instead of HTTPS
  #     NotifySelf: yes                      # Notify the sender's devices
  #     SkipTLSVerify: no                    # Whether to skip TLS certificate verification
  #     TextMode: None                       # Message text mode - None/Normal/Styled
  #     Title: Argus                         # Prepended to the message body
  #     Token: ''                            # API token for Bearer authentication
  ```
  {{% /tab %}}
  {{% tab header="signalgrid" %}}
  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/push/signalgrid/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: signalgrid
      url_fields:
        ClientKey: CLIENT_KEY
        Channel: CHANNEL_TOKEN
  #   params:
  #     Critical: no # Deliver as a critical alert
  #     Title: Argus # Notification title
  #     Type: INFO   # Notification severity - SUCCESS/INFO/WARN/CRIT
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

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/slack/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: slack
      url_fields:
        Token: 'xoxb:012345678901-0123456789012-phq24qfrhqnm2mtmz54dwkop'
        Channel: argus
  #   params:
  #     BotName: Argus                                                     # Bot name
  #     Color: ''                                                          # Message left-hand border color
  #     Icon: https://release-argus.io/favicons/android-chrome-512x512.png # URL or Emoji to use
  #     Title: Release                                                     # Prepended text above the message
  #     Thread_TS: ''                                                      # TS value of the parent message (to send message as reply in thread)
  ```
  {{% /tab %}}
  {{% tab header="smtp" %}}
  - email notifications
  - Addresses need a domain (e.g. `root@localhost`, not `root`).
  - For OAuth2, set `Auth: OAuth2` and either put the access token in `Password`, or set `ClientID`, `TokenURL` and `ClientSecret` and/or `RefreshToken` for Shoutrrr to fetch one.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/email/smtp/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: smtp
      url_fields:
  #     Username: NAME
  #     Password: PASSWORD123
        Host: your_host
  #     Port: 25
      params:
        FromAddress: foo@bar.com                # E-mail address that the mail are sent from
  #     FromName: Argus                         # Name of the sender
        ToAddresses: name@you.com,other@you.com # List of recipient e-mails (comma-separated)
  #     Auth: Plain                             # SMTP authentication method - None/Plain/Login/CRAMMD5/OAuth2/Unknown
  #     ClientHost: ''                          # SMTP client hostname
  #     ClientID: ''                            # OAuth2 client ID, for getting the access token
  #     ClientSecret: ''                        # OAuth2 client secret, for getting the access token
  #     RefreshToken: ''                        # OAuth2 refresh token, for refreshing the access token
  #     Scopes: ''                              # Comma separated OAuth2 scopes, for the client credentials grant
  #     TokenURL: ''                            # OAuth2 token endpoint, for getting the access token
  #     Encryption: Auto                        # Encryption method - None/ExplicitTLS/ImplicitTLS/Auto
  #     Subject: '{{ service_id }} Release'     # The subject of the sent mail
  #     Timeout: 10s                            # Timeout for the dial and SMTP session (a positive 'AhBmCs' duration)
  #     UseHTML: no                             # Whether the message being sent is in HTML
  #     UseStartTLS: yes                        # Whether to use StartTLS encryption
  #     RequireStartTLS: no                     # Fail if StartTLS is enabled but unsupported
  #     SkipTLSVerify: no                       # Whether to skip TLS certificate verification
  ```
  {{% /tab %}}
  {{% tab header="teams" %}}
  - Microsoft retired Office 365 Connectors, so Teams notifications now use a **Power Automate workflow**. Create a 'Post to a channel when a webhook request is received' workflow following '[Microsoft's docs](https://support.microsoft.com/en-US/teams/apps-service/create-incoming-webhooks-with-workflows-for-microsoft-teams)' and copy the workflow's HTTP POST URL.

  {{< alert title="Note" >}}
  The old Office 365 Connector format (`Group`/`Tenant`/`AltID`/`GroupOwner` url_fields) will still pass validation, but will fail at send time. Migrate to the Power Automate workflow URL below.
  {{< /alert >}}

  - The URL must keep its `sig` query parameter, and its host must be `*.logic.azure.com` (or `.us`/`.cn`) or `*.environment.api.powerplatform.com`.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/teams/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: teams
      params:
        Host: 'https://prod-00.westus.logic.azure.com:443/workflows/.../triggers/manual/paths/invoke?...&sig=...'
                      # The full Power Automate workflow POST URL
  #     Color: 00ff00 # Message theme color (hex)
  #     Title: Argus  # Notification title
  ```
  {{% /tab %}}
  {{% tab header="telegram" %}}
  - To get a token, [talk to the botfather](https://core.telegram.org/bots#6-botfather).
  - Chats
      - `@channel-name` can only be used for public channels. The name can be found by going to `Channel info`, just replace the `t.me/` prefix with an `@`.
      - `chat_id` is required for private channels/group chats/private chats. To get the ID, you can forward a message (from the target chat) to [@UserInfoBot](https://t.me/userinfobot) or [@JsonDumpBot](https://t.me/jsondumpbot) and view it at `Id` and `message.forward_from_chat.id` from those bots respectively.
      - (The above bots are created and hosted by [@nadam](https://github.com/nadam) and their sources are available to view at [nadam/userinfobot](https://github.com/nadam/userinfobot) and [nadam/jsondumpbot](https://github.com/nadam/jsondumpbot))

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/telegram/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: telegram
      url_fields:
        Token: TOKEN_FROM_BOTFATHER
      params:
        Chats: '@channel-name,@other-channel,chat_id' # Chat IDs or Channel names (using @channel-name)
  #     APIURL: https://api.telegram.org              # Bot API server base URL, e.g. for a self-hosted Bot API server
  #     Notification: yes                             # If disabled, sends the message silently
  #     ParseMode: None                               # How the text message should be parsed - None/Markdown/HTML/MarkdownV2
  #     Preview: yes                                  # If disabled, no web page preview will be displayed for URLs
  #     SkipTLSVerify: no                             # Whether to skip TLS certificate verification
  #     Title: Argus                                  # Notification title
  ```
  {{% /tab %}}
  {{% tab header="twilio" %}}
  - SMS through Twilio. Find the Account SID (`AC` + 32 hex characters) and Auth Token on the Twilio Console.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/sms/twilio/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: twilio
      url_fields:
        AccountSID: AC0123456789abcdef0123456789abcdef
        AuthToken: AUTH_TOKEN      # Twilio Auth Token, or the API key secret with APIKey
        FromNumber: '+15551234567' # Phone number, alphanumeric sender ID, or Messaging Service SID
        ToNumbers: '+15557654321,+15550000000'
                                   # Comma separated recipient phone numbers
  #   params:
  #     APIKey: ''                 # API key SID, which authenticates with AuthToken as its secret
  #     Title: Argus               # Notification title
  ```
  {{% /tab %}}
  {{% tab header="wecom" %}}
  - Add a **Group Bot** to a WeCom group chat, and copy the `key=` value from its webhook URL.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/wecom/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: wecom
      url_fields:
        Key: WEBHOOK_KEY
  #   params:
  #     Mentioned_List: ''        # Comma separated user IDs to mention, or @all
  #     Mentioned_Mobile_List: '' # Comma separated mobile numbers to mention, or @all
  ```
  {{% /tab %}}
  {{% tab header="xmpp" %}}
  - Connects with STARTTLS. For implicit TLS (`xmpps://`), use the `shoutrrr` type with a raw URL.
  - At least one of `To` or `Rooms` is required.

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/xmpp/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: xmpp
      url_fields:
        User: argus                 # JID localpart or full JID
        Password: PASS123
        Host: xmpp.example.io
  #     Port: 5222
      params:
        To: user@example.io         # Comma separated recipient JIDs
  #     Rooms: room@conf.example.io # Comma separated MUC room JIDs
  #     DisableTLS: no              # Connect without TLS
  #     Nick: Argus                 # MUC nickname
  #     RoomPassword: ''            # Password for protected MUC rooms
  #     SkipTLSVerify: no           # Whether to skip TLS certificate verification
  #     Title: Argus                # Prepended to the message body
  ```
  {{% /tab %}}
  {{% tab header="zulip" %}}
  Zulip Chat

  [Shoutrrr docs](https://shoutrrr.nickfedor.com/v0.25.0/services/chat/zulip/)

  ```yaml
  notify:
    ...
    EXAMPLE_NOTIFY:
      type: zulip
      url_fields:
        BotMail: bot@release-argus.io
        BotKey: BOT_KEY
        Host: zulip.example.io
  #     Port: 443
  #   params:
  #     Stream: mystream     # Stream to send the message to
  #     Topic: Argus         # Topic within the stream
  #     Type: stream         # Message type - stream/direct
  #     To: user1@x.com,123  # Comma-separated user IDs or emails for direct messages (Type: direct)
  #     Title: Argus         # Notification title
  #     Read_By_Sender: no   # Mark the message as read by the sending bot
  #     SkipTLSVerify: no    # Whether to skip TLS certificate verification
  ```
  {{% /tab %}}
{{% /tabpane %}}
