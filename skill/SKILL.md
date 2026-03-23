---
name: amazon-returns
description: >
  Process Amazon returns end-to-end: browse recent orders, let the user select
  items to return, submit returns through Amazon website (Whole Foods drop-off
  preferred, UPS as fallback), collect all QR codes, and email them to the user.
  Use this skill whenever the user mentions returning Amazon items, Amazon returns,
  getting refunds on Amazon orders, or wanting QR codes for drop-off.
---

# Amazon Returns Automation

Automate the full Amazon return process using browser automation.

## Prerequisites

- Browser tools required: Chrome browser automation tools
- Amazon login: User must be logged into Amazon
- Email login: User must be logged into their email provider

## User Preferences

Before starting, ask the user for:
- Email address to send QR codes to
- Return reason (default: Changed Mind)
- Drop-off preference (default: Whole Foods first, UPS fallback)
- Order lookback (default: 60 days)

## Workflow

### Phase 1: Gather Orders
Navigate to Amazon order history, scrape orders, present interactive checklist.

### Phase 2: Process Returns
For each selected item, walk through Amazon return flow automatically.

### Phase 3: Extract QR Codes
Use charCodeAt() technique to extract S3 signed URLs from return confirmation pages.
See references/technical-guide.md for critical workarounds.

### Phase 4: Email QR Codes
Compose and send email with all QR codes embedded as images.

### Phase 5: Save Local Backup
Save mobile-friendly HTML file with all QR codes.
