# HANDOFF.md — endofdays-site

> Last updated: 2026-07-09 — Server

---

## Current state

Marketing page (`index.html`) has been through several conversion passes (3-tier
pricing, affiliates section, bigger hero, light theme). `welcome/index.html` handles
post-purchase onboarding. `app/index.html` is a real Supabase-backed member portal
(signup/login, demo Q&A, entitlement check) — functional for auth and the free demo, but
the paid-download gating is only half-built: the Cloudflare Worker it depends on has
never been deployed.

---

## In progress

- Nothing actively in flight. Last commit was a layout pass (bigger hero, light theme,
  tighter gap, simplified affiliates) on the marketing page.

---

## Next

- When you open this → deploy the Cloudflare Worker for `DOWNLOAD_ENDPOINT` (see commit
  `4b6b430`, "Gated download via Cloudflare Worker") and replace the
  `eod-download.CHANGE-ME.workers.dev` placeholder in `app/index.html` with the real URL
  — until then the paid download button always shows "Download isn't switched on yet."
- Separately, confirm with Adam whether the hardcoded Supabase URL + anon/publishable key
  in `app/index.html` are meant to be public (Supabase anon/publishable keys are
  generally safe client-side) or should be rotated/handled differently — see Blockers.

---

## Blockers

- **SECURITY (documented only, not fixed here):** `app/index.html` (lines ~115-116)
  hardcodes a live Supabase project URL (`https://hcxbuomybreemkndzxnd.supabase.co`) and
  a publishable/anon key (`sb_publishable_...`) directly inline. The gated-download flow
  also sends the logged-in user's live Supabase session bearer token
  (`Authorization: Bearer <access_token>`) to `DOWNLOAD_ENDPOINT` — that endpoint is
  currently an undeployed placeholder, so nothing is exposed yet, but once real it needs
  to be a trusted, HTTPS-only Worker. A separate pass handles rotation/remediation
  decisions; this note is just to keep it visible.

---

## Watch out

- `app/index.html` still contains a leftover UI string ("This portal isn't connected to
  its database yet...") that's stale — the portal *is* wired to Supabase now
  (`configured` check passes since the values aren't `YOUR_...` placeholders anymore).
- `R2-DELIVERY.md`, referenced in a comment in `app/index.html` as the doc for the
  Cloudflare Worker setup, does not exist anywhere in this repo — the Worker itself was
  never actually built, only stubbed for.

---

## Recently completed

- 3-tier pricing (Free / Membership DIY Build / The Box) + affiliates section wired to
  `/app/` signup CTAs.
- Post-purchase `welcome/` onboarding page.
- Supabase-wired member portal (`app/index.html`): signup/login, demo Q&A, entitlement
  check, profile upsert.
