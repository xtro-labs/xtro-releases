# How Xtro Works

Xtro lets you code on your Mac and continue on your iPhone. But how does your terminal session get from one device to another? This page explains the architecture and security model.

**The short version:** The relay server just moves your data between devices. It doesn't read it, doesn't store it, doesn't log it. We couldn't look at your code even if we wanted to.

## Three-Part Architecture

Xtro comprises three integrated components:

1. **Mac App (XtroMac)** - Runs on your Mac, hosts your terminal sessions, connects to VS Code, and transmits data to the relay server

2. **iOS App (XtroiOS)** - Runs on your iPhone/iPad, receives data from the relay server, and displays your terminal in real-time

3. **Relay Server** - Connects your Mac and iPhone across networks by routing messages between paired devices

```
┌─────────────┐                            ┌─────────────┐
│   XtroiOS   │◄───── WebSocket (cloud) ──►│             │
└─────────────┘                            │ xtro-relay  │
                                           │             │
┌─────────────┐                            │             │
│   XtroMac   │◄───── WebSocket (cloud) ──►│             │
└──────┬──────┘                            └─────────────┘
       │
       │ Local WebSocket (port 8765)
       ▼
┌─────────────┐
│   VS Code   │
└─────────────┘
```

## Why the Relay Server Exists

Your phone and computer are usually on different networks. They can't talk directly to each other.

Without the relay server, you'd need to:
- Configure port forwarding on your router
- Find your public IP address
- Hope your mobile network doesn't block incoming connections (spoiler: it does)

The relay server solves this by acting as a meeting point. Both your Mac and iPhone connect *out* to the relay server, which then routes messages between them. Think of it like an automated switchboard—both devices call in, and the switchboard connects them to each other.

## Security Model

### We Can't Read Your Data

Xtro uses two layers of encryption:

**Layer 1: The Tunnel (TLS)**
All connections to the relay use WSS (WebSocket Secure)—the same encryption that protects your banking. This prevents anyone on the network from snooping on your traffic.

**Layer 2: End-to-End Encryption**
Here's the important part: your terminal data is encrypted *before* it leaves your Mac, using a key that only your paired devices know. The relay server sees encrypted blobs and forwards them. It can't decrypt them. It doesn't have the key.

This means even if someone compromised our servers—whether a rogue employee, a hacker, or a state actor with a court order—they'd get nothing but encrypted gibberish. The relay terminates the TLS tunnel, but the payload inside remains encrypted end-to-end. We literally cannot read your data.

The server doesn't:
- Parse your terminal output
- Store your session content
- Log your commands
- Analyze what you're working on

It just sees "device A wants to send encrypted bytes to device B" and makes that happen. The relay is a dumb pipe—it routes data, nothing more.

**Don't take our word for it.** The relay server is ~1,500 lines of JavaScript, and we're open-sourcing it. Read every line yourself. You'll find no data collection, no logging, no decryption attempts. What you see is what you get.

### Same-Account Enforcement

Your devices can only pair with each other if they're logged into the same account. When your Mac generates a pairing QR code, the relay server validates that the iPhone scanning it belongs to the same user. Different accounts = pairing rejected.

### JWT Authentication

Every connection to the relay server requires a valid JWT token from Supabase. The server validates:
- Token signature and expiration
- User account status
- Active subscription

Invalid tokens are rejected immediately. No token = no connection.

### Subscription Validation

The relay server checks your subscription status on every connection. Active or trialing subscriptions work. Expired subscriptions get a 403 error. This happens server-side, so even a modified client can't bypass it.

### Message Routing Isolation

Messages are only routed between devices that:
1. Are explicitly paired (via QR code)
2. Belong to the same user account
3. Have active connections

The server validates user ownership on every message. If someone somehow got your device ID, they still couldn't receive your messages without your account credentials.

## Data Flow

When you type in your terminal:

1. **Mac receives keystroke** from VS Code or the native Mac terminal view
2. **Mac sends message** through the WebSocket connection to the relay server
3. **Relay validates** the sender's pairing and routes to the paired device
4. **iOS receives message** and updates the terminal display in real-time
5. **iOS sends ACK** back to confirm receipt (for connection health monitoring)

Terminal output flows the same way, just in reverse.

### Resilient Connections

Mobile networks are unreliable. Xtro handles this with:

- **Grace periods**: If your iPhone briefly disconnects (switching towers, entering elevator), the relay holds your session for 15 seconds before considering you offline
- **Automatic reconnection**: Apps automatically reconnect when network returns
- **Message queuing**: Important notifications queue up if you're offline and deliver when you reconnect
- **Ping monitoring**: The server detects zombie connections (WiFi died but socket didn't close) and cleans them up

## Local VS Code Integration

Beyond cloud relay, the Mac app also runs a local WebSocket server on port 8765. This lets the Xtro VS Code extension communicate directly with the Mac app without going through the internet—lower latency and works even offline.

## Open Source

The relay server is approximately 1,500 lines of JavaScript. We're open-sourcing it so you can:
- Audit the code yourself
- Verify our security claims
- Confirm there's no sneaky data collection

The code is straightforward: authenticate users, validate pairings, forward messages. No hidden complexity, no surprise features.

### Run Your Own Server

For those who are extra paranoid (we respect that), we'll provide everything you need to run your own relay server. Point your apps at your own infrastructure, and your data never touches our servers at all.

This is on our roadmap. When available, you'll be able to deploy with a single command to your own VPS, Docker host, or even a Raspberry Pi on your home network.

## Infrastructure

The relay server runs on Hetzner VPS in multiple regions:
- **USA (Ashburn)**: relay.xtro.dev
- **Singapore**: relay-sg.xtro.dev

Devices automatically connect to the nearest server for lower latency.

---

**Bottom line:** The relay server is a dumb pipe. It moves bytes between your devices and has no idea what's inside them. We don't read your code, we don't store your sessions, we don't analyze your terminal output. We just make the connection work—nothing more, nothing less.
