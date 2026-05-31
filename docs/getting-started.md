# Getting Started

This guide is the happy path for Mac admins deploying Setup Manager HUD with
the Cloudflare Deploy Button.

Setup Manager HUD has two separate security layers:

- `WEBHOOK_TOKEN` is required. Setup Manager uses it when posting webhook events.
- Cloudflare Access is optional. It protects people viewing the dashboard.

Cloudflare Access does not replace `WEBHOOK_TOKEN`.

## What You Need

- A Cloudflare account
- A GitHub account that can create the dashboard repository
- Setup Manager ready to receive a webhook configuration
- A Mac with Terminal for generating the token and running test commands

## 1. Generate The Webhook Token

Run this on your Mac:

```bash
openssl rand -hex 24
```

Save the value somewhere secure before continuing. You will paste the same
token into Cloudflare and into your Setup Manager webhook plist.

## 2. Deploy To Cloudflare

Open the Deploy Button from the main README:

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/motionbug/setupmanagerhud-starter)

During the Cloudflare setup flow:

1. Sign in to Cloudflare if prompted.
2. Connect or select your GitHub account if prompted.
3. Let Cloudflare create or connect the dashboard repository.
4. Choose the Worker name you want to use.
5. Paste your generated token into the required `WEBHOOK_TOKEN` field.
6. Start the deployment.

The Deploy Button uses `.dev.vars.example` so Cloudflare knows to ask for
`WEBHOOK_TOKEN`. Do not leave this value blank.

## 3. Save The Dashboard Values

After Cloudflare finishes deploying, save these values:

| Value | Example |
|-------|---------|
| Dashboard URL | `https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev` |
| Webhook URL | `https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev/webhook` |
| Webhook token | The value from `openssl rand -hex 24` |

Use the Worker URL Cloudflare gives you. Your Worker name may not be
`setupmanagerhud`.

## 4. Check Health

Open the health endpoint in a browser or run:

```bash
curl https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev/api/health
```

Healthy storage looks like this:

```json
{"status":"healthy","d1":"connected","durable_objects":"connected"}
```

If D1 or Durable Objects are not connected, see
[Troubleshooting](troubleshooting.md#dashboard-shows-a-storage-warning).

## 5. Configure Setup Manager

Add the webhook URL and token to each webhook in your Setup Manager
configuration plist:

```xml
<key>webhooks</key>
<dict>
  <key>started</key>
  <dict>
    <key>url</key>
    <string>https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev/webhook</string>
    <key>token</key>
    <string>paste-your-secret-here</string>
  </dict>
  <key>finished</key>
  <dict>
    <key>url</key>
    <string>https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev/webhook</string>
    <key>token</key>
    <string>paste-your-secret-here</string>
  </dict>
</dict>
```

The `token` value must exactly match the `WEBHOOK_TOKEN` value saved in
Cloudflare.

## 6. Send A Test Event

Before relying on live Setup Manager enrollments, send one test event:

```bash
curl -X POST https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev/webhook \
  -H "Authorization: Bearer your-token-here" \
  -H "Content-Type: application/json" \
  -d '{"name":"Started","event":"com.jamf.setupmanager.started","timestamp":"2025-01-01T00:00:00Z","started":"2025-01-01T00:00:00Z","modelName":"Test Mac","modelIdentifier":"Mac15,3","macOSBuild":"24A335","macOSVersion":"15.0","serialNumber":"TEST001","setupManagerVersion":"2.0.0"}'
```

- `200 OK` means the token matched and the Worker accepted the event.
- `401 Unauthorized` means the token is missing or does not match.
- `503 Service Unavailable` usually means D1 is missing, unbound, or not migrated.

Refresh the dashboard and confirm the test event appears.

## 7. Optional: Protect Dashboard Viewing

The dashboard can be public or protected with Cloudflare Access.

Cloudflare Access protects humans opening the dashboard, API, and WebSocket.
Setup Manager devices still post to `/webhook`, and that route must continue to
use `WEBHOOK_TOKEN`.

See [Security](security.md#cloudflare-access-optional-dashboard-authentication)
for the Cloudflare Access setup steps.
