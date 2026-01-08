---
title: Relay Server
description: Run your own Xtro relay server
---

# Self-Hosting the Relay Server

For those who want complete control over their data, you can run your own Xtro relay server.

<Note>
Self-hosting is coming soon. This page will be updated with full instructions when available.
</Note>

## Why Self-Host?

The relay server is a "dumb pipe"—it routes encrypted messages between your devices without reading them. But if you want to be extra sure, you can run it yourself:

- **Complete control** - Your data never touches our infrastructure
- **Custom deployment** - Run on your own VPS, home server, or even a Raspberry Pi
- **Audit everything** - The code is open source and straightforward

## Requirements

- Node.js 18+
- A server with a public IP (or use a tunnel like Cloudflare)
- SSL certificate (required for WebSocket Secure connections)

## Quick Start

```bash
# Coming soon
npx xtro-relay
```

## Docker

```bash
# Coming soon
docker run -p 8080:8080 xtro/relay
```

## Configuration

Details coming soon.

## Pointing Your Apps to Your Server

Once your relay is running, you can configure the Xtro apps to use your server instead of the default `relay.xtro.dev`.

Instructions coming soon.
