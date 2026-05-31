# Setup Manager HUD

Real-time webhook dashboard for [Setup Manager](https://github.com/jamf/setup-manager).

## Quick Start

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/motionbug/setupmanagerhud-starter)

### Before You Deploy

Generate a webhook token:

```bash
openssl rand -hex 24
```

Save this token - you'll need it for Cloudflare and Setup Manager.

When the Deploy Button opens Cloudflare, paste this value into the required
`WEBHOOK_TOKEN` secret field. This is the shared token that Setup Manager must
send with every webhook request.

The template includes `.dev.vars.example` so Cloudflare's Deploy Button shows
`WEBHOOK_TOKEN` during setup. `wrangler.toml` also declares this as a required
secret, so manual deploys fail clearly if the token has not been configured.

### After Deployment

1. Open your Worker URL: `https://setupmanagerhud.<subdomain>.workers.dev`
2. Configure Setup Manager with your webhook URL and token
3. (Optional) Set up Cloudflare Access for dashboard protection

## Upgrading

When the dashboard shows that an update is available, update the Git repository
created for your deployment and push the change so Cloudflare rebuilds it. See
[Upgrading Setup Manager HUD](UPGRADE.md) for the step-by-step guide.

## Local Development

```bash
npm install
npm run sync
npm run dev
```

## Documentation

- [Configuration](https://github.com/motionbug/setupmanagerHUD-npm/blob/main/docs/Configuration.md)
- [Security](https://github.com/motionbug/setupmanagerHUD-npm/blob/main/docs/Security.md)
- [Troubleshooting](https://github.com/motionbug/setupmanagerHUD-npm/blob/main/docs/Troubleshooting.md)

## License

[MIT](https://github.com/motionbug/setupmanagerHUD-npm/blob/main/LICENSE)
