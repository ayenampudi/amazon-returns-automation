# Amazon Returns Plugin

Automate Amazon returns from start to finish — browse your recent orders, select items to return, submit the returns, collect all QR codes, and get them emailed to you for easy drop-off at Whole Foods or UPS.

## What it does

1. **Browse orders** — Scans your last 60 days of Amazon orders
2. **Select items** — Presents a checklist so you pick what to return
3. **Submit returns** — Processes each return automatically (Whole Foods preferred, UPS as fallback)
4. **Collect QR codes** — Extracts every QR code from the return confirmations
5. **Email everything** — Sends you a nicely formatted email with all QR codes, deadlines, and refund amounts
6. **Local backup** — Saves a phone-friendly HTML file you can pull up at the store

## Setup

- You need to be logged into **Amazon** in your browser
- You need to be logged into your **email** (Outlook, Gmail, etc.) in your browser
- The plugin uses Chrome browser automation, so the Claude in Chrome extension must be active

## Usage

Just say something like:

- "Return my Amazon stuff"
- "Process my Amazon returns"
- "I want to return some orders"
- "Get my return QR codes"

The plugin will ask for your preferences (email address, return reason, etc.) on first use.

## Customization

This plugin uses `~~email provider` as a placeholder — it works with Outlook, Gmail, Yahoo Mail, or any webmail client. See `CONNECTORS.md` for details.
