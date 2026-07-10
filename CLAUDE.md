# CLAUDE.md — endofdays-site

> Loads automatically every session in this project. Inherits global rules from
> `/workspace/_FOUNDATION.md`.
> Last updated: 2026-07-09

---

## What this project is

Marketing site + member portal for **EndOfDays** (an offline survival/prep platform
product — deployed at `endofdays.ai`, see `CNAME`). Three static pages, no build step:
root `index.html` (marketing/pricing — Free / Membership DIY Build / The Box tiers, plus
an affiliates section), `welcome/index.html` (post-purchase onboarding for new
members/box buyers), and `app/index.html` (the actual member portal — Supabase-backed
signup/login, a gated demo Q&A, entitlement checking, and a gated download for the
offline install package).

---

## Stack

- Plain HTML/CSS/JS, no framework, no build step, no bundler.
- **Supabase JS v2** via CDN (`app/index.html`) — auth, `profiles` upsert on login,
  `entitlements` table check for paid status.
- **Cloudflare Worker** (planned, not yet deployed) gates the actual installer download —
  `DOWNLOAD_ENDPOINT` in `app/index.html` is still a placeholder
  (`eod-download.CHANGE-ME.workers.dev`). Referenced doc `R2-DELIVERY.md` does not exist
  in this repo.
- Static hosting behind custom domain `endofdays.ai` (`CNAME` file → GitHub Pages style
  deploy).

---

## What Claude SHOULD do here

- Treat `app/index.html` as the only page wired to a real backend (Supabase) — the other
  two pages are static marketing/onboarding content.
- Before shipping the download-gating feature, actually deploy the Cloudflare Worker and
  replace the `CHANGE-ME` placeholder.

## What Claude SHOULD NOT do here

- Don't hardcode more secrets inline in `app/index.html` — it already has a live Supabase
  URL + anon/publishable key inline (see HANDOFF.md blockers); don't add to that pattern.
- Don't assume the gated download works — the Worker endpoint is unbuilt.

---

## State files

- `HANDOFF.md` — current state, blockers, next step
