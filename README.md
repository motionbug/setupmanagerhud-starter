# Setup Manager HUD

Real-time webhook dashboard for [Setup Manager](https://github.com/jamf/setup-manager).

## Quick Start

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/motionbug/setupmanagerhud-starter&fields=%7B%22name%22:%22Webhook%20Token%22,%22secret%22:%22WEBHOOK_TOKEN%22,%22descr%22:%22Generate%20with:%20openssl%20rand%20-hex%2024%22%7D)

### Before You Deploy

Generate a webhook token:

```bash
openssl rand -hex 24
```

Save this token - you'll need it for Cloudflare and Setup Manager.

### After Deployment

1. Open your Worker URL: `https://setupmanagerhud.<subdomain>.workers.dev`
2. Configure Setup Manager with your webhook URL and token
3. (Optional) Set up Cloudflare Access for dashboard protection

## Upgrading

When a new version of Setup Manager HUD is released:

```bash
npm run upgrade
npm run deploy
```

This updates the core package, syncs new assets/migrations, and redeploys.

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
