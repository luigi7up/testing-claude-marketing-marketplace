---
description: >
  Campaign visualizer. Generates the client's index.html overview page — a self-contained HTML
  file that inlines the client's business profile, competitive research, marketing strategy,
  and all campaign iterations (ad asset previews + results). Invoke this skill after any
  campaign execution or whenever the overview page needs to be refreshed.
tools:
  - Write
  - Read
  - Bash
---

# Campaign Visualizer

You generate `index.html` for a client — a single self-contained HTML file that inlines all their memory files and campaign results into one navigable overview page.

## Input

You will receive:
- The client path prefix (e.g. `plugins/aida/clients/example-com/`) — all reads and the final write target this directory

## Step 1 — Gather data

Read all of the following in parallel:
- `[client-path]/meta.md`
- `[client-path]/memory/business_profile.md`
- `[client-path]/memory/competitors.md` (if it exists)
- `[client-path]/memory/marketing_strategy.md` (if it exists)

List all campaign versions:
```bash
ls [client-path]campaigns/ 2>/dev/null | grep -E '^v[0-9]+$' | sort -V
```

For each version found, read in parallel:
- `[client-path]campaigns/[v]/results/gbp-listing.md` (if exists)
- `[client-path]campaigns/[v]/results/social-media.md` (if exists)
- `[client-path]campaigns/[v]/results/google-ads.md` (if exists)

List asset files in each version:
```bash
ls [client-path]campaigns/[v]/assets/ 2>/dev/null
```

## Step 2 — Generate index.html

Write `[client-path]index.html`. The file must be fully self-contained — no external CSS, JS, or font dependencies. All styles go in a `<style>` block in `<head>`.

---

### Page layout

Two-column layout: fixed left sidebar (~220px) + scrollable main content area.

**Left sidebar** — `position:fixed`, full viewport height, `background:#1a1a2e`, `color:#fff`, `padding:24px 16px`, `width:220px`, `overflow-y:auto`

Contents:
- "Aida" wordmark at top — bold, `color:#4f9cf9`, `font-size:20px`
- Business name below — `font-size:13px`, `color:#a0aec0`, `margin-bottom:24px`
- Navigation `<a>` links that anchor-scroll to page sections — `font-size:13px`, `color:#a0aec0`, `text-decoration:none`, `display:block`, `padding:5px 0`, hover `color:#fff`
  - "Business Profile"
  - "Competitive Research" (only if competitors.md exists)
  - "Marketing Strategy" (only if marketing_strategy.md exists)
  - "Campaigns" heading (not a link, just a label — `color:#fff`, `font-size:11px`, `text-transform:uppercase`, `letter-spacing:.08em`, `margin:16px 0 6px`)
  - One sub-link per campaign version: "Campaign v1", "Campaign v2"… — indented 12px

**Main content area** — `margin-left:220px`, `padding:32px`, `background:#f7f8fc`, `min-height:100vh`

Each section is a white card: `background:#fff`, `border-radius:10px`, `box-shadow:0 1px 4px rgba(0,0,0,.08)`, `padding:28px`, `margin-bottom:24px`

---

### Sections

**Business Profile** (`id="business-profile"`)
- `<h2>` heading "Business Profile" — `color:#1a1a2e`
- Render the full content of `business_profile.md` as HTML (see markdown conversion rules below)

**Competitive Research** (`id="competitive-research"`) — only if `competitors.md` exists
- Render the full content of `competitors.md` as HTML

**Marketing Strategy** (`id="marketing-strategy"`) — only if `marketing_strategy.md` exists
- Render the full content of `marketing_strategy.md` as HTML

**Campaigns** (`id="campaigns"`)
- `<h2>` heading "Campaigns"
- One subsection per version, **newest first**. Each has `id="campaign-[v]"` (e.g. `id="campaign-v2"`)

For each campaign version:
- Version header: `<span>` badge "Campaign [v]" — `background:#4f9cf9`, `color:#fff`, `border-radius:6px`, `padding:3px 10px`, `font-size:13px`, `font-weight:600` + date from the results file's "Last updated" line in `color:#70757a`, `font-size:13px`, `margin-left:10px`

- **Ad Previews** `<h3>` sub-heading
  - For each HTML asset in `campaigns/[v]/assets/`, embed as `<iframe>`:
    - `src="campaigns/[v]/assets/[filename]"`
    - `width="100%"`, height based on file: `google-ads.html` → `520px`, `social-media-*.html` → `620px`, `gbp-listing.html` → `580px`, `website-update.html` → `300px`, anything else → `480px`
    - `style="border:1px solid #e0e0e0;border-radius:8px;display:block;margin-bottom:16px;"`
    - Label above each iframe — bold, `14px`, `#444`:
      - `google-ads.html` → "Google Ad"
      - `social-media-facebook.html` → "Facebook Post"
      - `social-media-instagram.html` → "Instagram Post"
      - `social-media-linkedin.html` → "LinkedIn Post"
      - `gbp-listing.html` → "GBP Listing"
      - `website-update.html` → "Website Update"

- **Campaign Results** `<h3>` sub-heading
  - For each results `.md` that exists, render its content as HTML inside a light inner box: `background:#f7f8fc`, `border-radius:8px`, `padding:16px`, `margin-bottom:12px`
  - Small heading above each: "Google Business Profile", "Social Media", "Google Ads" — `font-size:13px`, `font-weight:600`, `color:#70757a`, `margin-bottom:8px`

---

### Markdown → HTML conversion rules

Apply these when rendering all `.md` files:
- `# text` → `<h2>`, `## text` → `<h3>`, `### text` → `<h4>`
- `**text**` → `<strong>`, `_text_` / `*text*` → `<em>`
- `- item` or `* item` → `<ul><li>…</li></ul>`
- `1. item` → `<ol><li>…</li></ol>`
- Code fences → `<pre style="background:#f4f4f4;padding:12px;border-radius:6px;overflow-x:auto;font-size:13px;">`
- `| col |` tables → `<table style="border-collapse:collapse;width:100%">` with `<th>` header row, `<td>` cells — `border:1px solid #e0e0e0`, `padding:8px 12px`
- Blank lines → `<p>` paragraph breaks
- `---` → `<hr style="border:none;border-top:1px solid #e8e8e8;margin:16px 0;">`
