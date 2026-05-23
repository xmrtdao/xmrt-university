# How to Create a Photo Booth Template

## Overview

The PFP Template Builder generates print-ready 4×6 photo booth strip templates with AI-generated backgrounds, photo frame cutouts, event text, and decorative elements. Output is 1200×1800 PNG at 300 DPI.

## Script

| File | Location | Purpose |
|------|----------|---------|
| `pfp-template-builder.mjs` | `relay/tools/` | Generate a photo booth template |

## Usage

```bash
cd relay/tools

# Generate a template with AI background
node pfp-template-builder.mjs "Event Name" "Event Date" "elegant"
```

## Parameters

| Argument | Example | Description |
|----------|---------|-------------|
| Event name | `"DC JazzFest"` | Displayed prominently on the template |
| Event date | `"Sept 5-6, 2026"` | Optional, shown below event name |
| Style | `"elegant"` | One of: `elegant`, `modern`, `classic` |

### Styles

| Style | Vibe | Colors |
|-------|------|--------|
| `elegant` | Gold accents, serif font | Gold, cream, warm tones |
| `modern` | Clean, minimalist | Gray, white, sans-serif |
| `classic` | Traditional, formal | Dark navy, serif font |

## What It Generates

```
┌─────────────────────────────────┐
│        1200 × 1800 px            │
│          (4×6 @ 300 DPI)         │
│                                  │
│  ┌───────────────────────────┐   │
│  │  ✦  Photo Frame Slot 1   │   │
│  │     (rounded corners)     │   │
│  └───────────────────────────┘   │
│  ┌───────────────────────────┐   │
│  │  ✦  Photo Frame Slot 2   │   │
│  └───────────────────────────┘   │
│  ┌───────────────────────────┐   │
│  │  ✦  Photo Frame Slot 3   │   │
│  └───────────────────────────┘   │
│                                  │
│     ★★★ EVENT NAME ★★★          │
│         Event Date               │
│     partyfavorphoto.com          │
└─────────────────────────────────┘
```

Two identical strips side by side with a center cut line for printing.

## Workflow

1. **Generate AI background** (optional — via MuAPI or other tool)
2. **Run the template builder** with event details and style
3. **Preview the output** in `relay/pfp-outputs/`
4. **Submit for QC** — templates need visual review before sending to clients
5. **Send to client** or use in social media posts

## Output

Templates are saved to: `relay/pfp-outputs/{event-name}-{style}-{timestamp}.png`

## Tags

`pfp`, `template`, `photo-booth`, `strip`, `design`, `sharp`, `party-favor-photo`
