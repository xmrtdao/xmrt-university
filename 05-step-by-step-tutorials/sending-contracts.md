# How to Generate and Send a PFP Service Contract

## Overview

Party Favor Photo contracts are customized PDFs with client details, service terms, pricing, and signature lines. The contract generator pulls pricing from the shared data file and generates a print-ready PDF.

## Scripts

| File | Location | Purpose |
|------|----------|---------|
| `generate-pfp-contract.mjs` | `relay/tools/` | Generate a generic contract PDF |
| `send-contract.mjs` | `relay/tools/` | Customize + send contract to client |
| `send-contract-test.mjs` | `relay/tools/` | Test the send flow |
| `verify-contracts.mjs` | `relay/tools/` | Verify generated contracts |
| Pre-built PDFs | `partyfavorphoto/contracts/` | 12 pre-generated contracts (2-6hr × standard/premium) |

## Quick Start

```bash
cd relay/tools

# Generate and send a customized contract
node send-contract.mjs "client@email.com" "Client Name" "Event Name" 4 premium
```

## Parameters

| Argument | Example | Description |
|----------|---------|-------------|
| To | `hannah@dcjazzfest.org` | Client email address |
| Client | `"Hannah Kuhns"` | Client's full name |
| Event | `"DC JazzFest"` | Event name |
| Hours | `4` | 2-6 hours |
| Package | `premium` or `standard` | Pricing tier |

## What the Contract Includes

1. **Header** — PFP logo, company info, contract version
2. **Client & Event Details** — Names, date, location
3. **Service Description** — Package tier, hours, what's included
4. **Pricing** — Hourly rate, total, discount if applicable, deposit amount (50%)
5. **Payment Terms** — Deposit due at signing, balance due before event
6. **Terms & Conditions** — Cancellation policy, liability, photo usage rights
7. **Signature Lines** — Both parties with date

## Pre-Built Contracts

For common combinations without customization:

```bash
ls partyfavorphoto/contracts/
# PFP-2hr-Premium-Contract.pdf
# PFP-3hr-Standard-Contract.pdf
# etc. (12 files total)
```

These are ready to send for standard 2-6hr bookings.

## Data Source

Pricing, inclusions, and Stripe links come from:
```
partyfavorphoto/data/pfp-data.json
```

This file contains:
- `rates` — hourly rates by tier
- `inclusions` — what's included per tier
- `stripeLinks` — payment links by hours
- `maxDiscount` — maximum discount amount

## Output

Custom contracts are saved to: `reviews/custom-contracts/PFP-Contract-{Event}-{hours}hr-{tier}.pdf`

## Tags

`pfp`, `contract`, `pdf`, `pricing`, `signature`, `legal`, `party-favor-photo`
