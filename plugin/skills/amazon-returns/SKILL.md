---
name: amazon-returns
description: >
  Process Amazon returns end-to-end: browse recent orders, let the user select
  items to return, submit returns through Amazon's website (Whole Foods drop-off
  preferred, UPS as fallback), collect all QR codes, and email them to the user.
  Use this skill whenever the user mentions returning Amazon items, Amazon returns,
  getting refunds on Amazon orders, or wanting QR codes for drop-off. Also trigger
  when the user says things like "return my Amazon stuff", "I want to return some
  orders", "process my returns", "get my return QR codes", or "start Amazon returns".
---

# Amazon Returns Automation

Automate the full Amazon return process using browser automation — from browsing order history, to submitting return requests, to collecting QR codes and emailing them. The goal is a completely hands-free experience.

## Prerequisites

- **Browser tools required**: Chrome browser automation tools (navigate, read_page, find, computer, javascript_tool, etc.)
- **Amazon login**: The user must already be logged into Amazon in the browser
- **Email login**: The user must already be logged into their ~~email provider in the browser

## User Preferences

Before starting, ask the user (via AskUserQuestion) for any preferences not already known:

- **Email address** to send QR codes to
- **Return reason** — default to "Changed Mind" / "My needs changed"
- **Item condition** — default to "Original state, packaging not opened"
- **Refund method** — default to "Original payment method"
- **Drop-off preference** — default to "Whole Foods first, UPS as fallback"
- **Order lookback** — default to 60 days

If the user has previously set these and they're clear from context, skip the questions and use those values.

## Workflow

Follow these five phases in order. Read `references/technical-guide.md` before starting Phase 3 — it contains critical workarounds for known browser extension issues.

### Phase 1: Gather Orders

1. Navigate to `https://www.amazon.com/your-orders/orders?timeFilter=last60` (adjust the time filter based on user preference)
2. Scrape each order: item name, price, order date, order number
3. Page through results if there are multiple pages (click "Next" at bottom)
4. Skip any items that already show a return in progress
5. Build an interactive HTML checklist and save it to the outputs folder. Present it to the user so they can select which items to return. The HTML should have checkboxes next to each item with name and price, and a "Selected for Return" summary at the bottom that updates dynamically

### Phase 2: Process Returns

For each selected item, walk through Amazon's return flow:

1. Click the "Return or replace items" button on the order
2. Select the specific item checkbox if multiple items in the order
3. Select the return reason from user preferences
4. For the sub-reason or description, use a natural-sounding explanation
5. Confirm item condition per user preferences
6. Choose refund to original payment method
7. For drop-off method, prefer **Whole Foods** if available; otherwise **UPS**
8. Complete the return submission

Amazon's return flow uses a conversational/Rufus AI interface. Be adaptive — read the page at each step and respond to what's actually shown. Look for text inputs, radio buttons, and "Continue" / "Submit" buttons. The flow may vary slightly per item.

### Phase 3: Extract QR Codes

This phase has critical technical requirements. Read `references/technical-guide.md` for the charCode extraction technique and other workarounds before proceeding.

After each return is submitted, extract the QR code URL from the confirmation page and store it. Build a JSON mapping of item names to QR code URLs.

### Phase 4: Email QR Codes

Compose and send an email via the user's ~~email provider with all QR codes:

1. Open the email provider and compose a new message
2. Set **To**: the user's email address
3. Set **Subject**: "Amazon Returns - QR Codes for Drop-off (N items, $TOTAL)"
4. Inject an HTML body containing:
   - A header with "Amazon Returns - QR Codes"
   - Total refund amount and item count
   - An urgent warning if any items have deadlines within 10 days
   - A table with one row per Whole Foods item: item name, order number, deadline, QR code image (160x160), and refund amount
   - Deadlines within 10 days shown in red
   - A separate section for UPS items (instructions to print label from Amazon app)
   - A footer noting QR images are valid for 7 days with fallback instructions
5. Get explicit user confirmation before clicking Send
6. Click Send

### Phase 5: Save Local Backup & Summarize

1. Save a mobile-friendly HTML file to the outputs folder with all QR codes as a backup
2. Present the file to the user
3. Provide a summary: count of returns, total refund, which items are urgent, and any items that couldn't be processed
