---
title: Quick Start
description: Get up and running with Xtro in under 5 minutes
---

# Quick Start

Get Xtro running across your Mac and iPhone. The whole setup takes about 5 minutes.

## Requirements

- **macOS 15** Sequoia or later
- **iOS 18** or later
- A VS Code-compatible editor (VS Code, Cursor, Windsurf, Trae, Kiro, etc.)
- An Xtro account

---

## Mac Setup

### 1. Install XtroMac

Download the latest Mac app from your [Xtro Dashboard](https://app.xtro.dev/dashboard). Open the `.dmg`, drag to Applications, and launch it. Sign in or create an account.

### 2. Install the IDE Extension

XtroMac automatically scans `/Applications` for VS Code forks, it reads each editor's `product.json` to identify Cursor, Windsurf, Positron, Trae, Kiro, and others. You'll see a list of detected editors with checkboxes. Check the ones you want, and XtroMac installs the Xtro extension into each.

Restart your editor(s) after installation.

### 3. Verify the Connection

Look for the **Xtro** button in the bottom-right of your editor's status bar:

- **Green**, connected to XtroMac
- **Orange**, disconnected (make sure XtroMac is running)

### 4. Create Your First Session

Click the Xtro status bar button in your editor. This opens a new terminal session that's synced through Xtro, accessible from your iPhone once paired.

---

## iPhone Setup

### 1. Install XtroiOS

Download **Xtro** from your [Xtro Dashboard](https://app.xtro.dev/dashboard) (it links to the App Store). Sign in with the same account you used on your Mac.

### 2. Subscribe

Xtro requires an active subscription. You'll be prompted to subscribe on first launch.

### 3. Pair Your Devices

On your Mac, click the Xtro menu bar icon and select **"Pair Device..."**. A QR code appears. Scan it with your iPhone's camera (or from within the Xtro iOS app). Once paired, the **Conversations** list populates with your active terminal sessions.

---

## Using Xtro

### Creating Sessions

You can create terminal sessions from either device:

- **From your editor**, click the Xtro status bar button
- **From your iPhone**, tap the **+** button (top-right on the Conversations list)

Sessions appear on both devices instantly.

### Terminal

Your iPhone shows a fully interactive terminal:

- **Double-tap** to bring up the keyboard
- **Swipe down** to dismiss the keyboard
- **Swipe left/right** to switch between sessions
- **Scroll up/down** to scroll the terminal buffer
- **Two-finger scroll** to scroll inside TUI apps (like OpenCode CLI) that normally require a mouse scroll wheel

### Quick Keys

The bottom bar provides single-tap shortcuts for common AI CLI commands (accept, reject, continue, etc.). Presets are bundled for Claude Code, Gemini CLI, Cline, Codex CLI, Cursor Agent, and more. You can customize these in Settings.

### Git Integration

Tap the branch icon to access source control:

- View commit history
- Browse diffs with syntax highlighting
- Stage/unstage files
- Create commits
- Push and pull

### Voice Input

Tap the mic icon to dictate. Your speech is transcribed and inserted directly into the terminal, useful for prompting AI agents without typing on a phone keyboard.

### Screenshot Upload

Tap the photo icon to pick an image from your library and send it into the active terminal session. Great for sharing bug screenshots, design mockups, or error messages with AI agents that support image input.

---

## Tips

- **Garbled TUI output?** Some terminal UIs (like OpenCode CLI) may render incorrectly on first load. Rotate your phone to landscape and back, this forces a re-render and fixes the layout.
- **Session persistence**, sessions stay alive as long as XtroMac is running. Close your laptop lid and reopen it, your sessions are still there.
