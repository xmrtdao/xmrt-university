# How to Generate and Send a PFP Quote

## Overview

Party Favor Photo quotes are branded PDFs with setup photos, package recommendations, pricing breakdown, and a call to action. Two generators exist — one in the relay tools and one in the partyfavorphoto repo.

## Relay Tool (preferred)

### Scripts

| File | Location | Purpose |
|------|----------|---------|
| `generate-pfp-quote.mjs` | `relay/tools/` | Creates the quote PDF |
| `send-quote-email.mjs` | `relay/tools/` | Emails the quote to client |

### Usage

```bash
cd relay/tools

# Step 1: Generate the quote PDF
node generate-pfp-quote.mjs "Client Name" "Event Name" 4 premium
# Output: reviews/quotes/PFP-Quote-{Event}-{hours}hr.pdf

# Step 2: Email it to the client
node send-quote-email.mjs "client@email.com" "Client Name" "Event Name" 4 premium
```

### Parameters

| Argument | Example | Description |
|----------|---------|-------------|
| Client name | `"Hannah Kuhns"` | Client's full name |
| Event name | `"DC JazzFest"` | Event name (used in PDF filename) |
| Hours | `4` | 2, 3, 4, 5, or 6 |
| Package | `premium` or `standard` | Pricing tier (omit = standard) |

### Pricing Reference

| Package | Hourly Rate | Discount |
|---------|-------------|----------|
| Standard (2x6 strips) | $249/hr | $100 off eligible |
| Premium (4x6 prints) | $349/hr | $100 off eligible |

Discount applies for: military, school events, or pay-in-full.

### What Happens

1. Script loads setup photos from `Party Favor Photo set up photos/`
2. Generates a PDF with logo, photos, pricing table, and Stripe payment link
3. Saves to `reviews/quotes/PFP-Quote-{Event}-{hours}hr.pdf`
4. Send script attaches PDF and delivers via Resend
5. Email sent from `bookings@partyfavorphoto.com`

## Partyfavorphoto Repo Tool (alternative)

```bash
cd partyfavorphoto
node quotes/generate.mjs "Client" "Event" 4 premium
node quotes/send-email.mjs "client@email.com" "Client" "Event" 4 premium
```

Reads pricing and Stripe links from `data/pfp-data.json`.

## Stripe Booking Links

| Hours | Standard | Premium |
|-------|----------|---------|
| 2hr | `buy.stripe.com/8x25kD7ezg6h4iC15YbZe03` | — |
| 3hr | `buy.stripe.com/9B63cv9mH07j3eyeWObZe06` | — |
| 4hr | `buy.stripe.com/eVqcN556r4nz16qeWObZe04` | — |

## Tags

`pfp`, `quote`, `pdf`, `pricing`, `resend`, `email`, `party-favor-photo`
