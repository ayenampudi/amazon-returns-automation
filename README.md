# Amazon Returns Automation

A Claude skill and plugin that fully automates the Amazon return process — from browsing your order history, to submitting return requests, to extracting QR codes and emailing them to you for easy drop-off at Whole Foods or UPS.

## The Problem

Returning items on Amazon is tedious. For every item you need to: navigate to the order, click through the return flow, pick a reason, choose a drop-off method, get the QR code, and somehow get it to your phone for the store. Multiply that by several items and it's 30+ minutes of clicking.

## The Solution

Say "return my Amazon stuff" and Claude handles the entire process:

1. **Scans your orders** — Pulls your last 60 days of Amazon orders
2. **You pick what to return** — Shows an interactive checklist with item names and prices
3. **Submits every return** — Walks through Amazon's return flow for each item automatically
4. **Extracts QR codes** — Pulls QR code images from confirmation pages using a charCode extraction technique (see technical guide)
5. **Emails you everything** — Sends a formatted email with all QR codes, deadlines, and refund amounts
6. **Saves a backup** — Creates a phone-friendly HTML file with all QR codes

## How It Works

This project uses Claude's browser automation capabilities (via Claude in Chrome) to interact with Amazon's website as a logged-in user. The key technical challenge was extracting QR code URLs from Amazon's S3-hosted signed URLs, which the browser extension blocks from direct string extraction. The solution uses a charCode conversion technique documented in the technical guide.

## Repository Structure

```
.
├── skill/                    # Standalone Claude skill (personal use)
│   └── SKILL.md              # Self-contained skill with hardcoded preferences
├── plugin/                   # Distributable Claude plugin (shareable)
│   ├── .claude-plugin/
│   │   └── plugin.json       # Plugin manifest
│   ├── skills/
│   │   └── amazon-returns/
│   │       ├── SKILL.md      # Customizable skill (asks user preferences)
│   │       └── references/
│   │           └── technical-guide.md  # QR extraction techniques & workarounds
│   ├── CONNECTORS.md         # Tool placeholder documentation
│   └── README.md             # Plugin-specific docs
└── LICENSE
```

## Two Ways to Use It

### 1. Standalone Skill (for yourself)
The `skill/` directory contains a single SKILL.md you can drop into your Claude skills folder. It has hardcoded preferences and runs fully automatically.

### 2. Plugin (to share with others)
The `plugin/` directory is a full Claude plugin that anyone can install. It asks users for their preferences on first run and works with any email provider (Outlook, Gmail, Yahoo, etc.).

## Prerequisites

- Claude desktop app with Cowork mode or Claude Code
- Claude in Chrome browser extension
- Logged into Amazon in your browser
- Logged into your email provider in your browser

## Technical Highlights

- **charCode extraction**: Bypasses browser extension URL blocking by converting characters to integer arrays
- **Adaptive return flow**: Handles Amazon's conversational Rufus AI interface and varying return steps
- **HTML email injection**: Composes rich HTML emails with embedded QR code images directly in webmail
- **S3 signed URL embedding**: QR codes are embedded as live S3 URLs in emails (valid for 7 days)

See `plugin/skills/amazon-returns/references/technical-guide.md` for full technical details.

## License

MIT
