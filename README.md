# Homeward

A private move tracker for our household — packing, decisions, and follow-ups in one place, synced across our phones and desktops. Easy.

**Live:** https://nerdlykyle.github.io/homeward/

Sign in with a magic link (email), pick your name, and you're in. Everything is shared — we both see and edit the same data.

---

## What's inside

- **Packing** — rooms with a percent slider and box count. A room can hold **sections** (Kyle's closet, Andrea's chest); once it has any, the room's % becomes the average of its sections. Boxes track contents, source room, destination room, and open-first / fragile flags.
- **Goals** — assign a one-off task, a whole room, or a single section to either of us, with progress.
- **Sort** — the decision flow:
  - **Questions** — snap or type an item, tag the other person with a "?", they answer Keep / Toss / Donate / Sell. A badge shows what's waiting on you.
  - **Toss / Donate** — haul checklists; check one off when it's gone.
  - **Sell** — asking price, To list → Listed → Sold, with a running total of what we've made.
- **Tasks** — Isla's Nipher registration paperwork, and Vic follow-ups (painter & carpet).
- **Archive** — everything finished and cleared.

## Stack

- Single static `index.html` — plain JavaScript, no framework, no build step.
- [Appwrite Cloud](https://cloud.appwrite.io) backend: Auth (magic link), Databases, Storage (photos).
- Hosted on GitHub Pages.

Photos are shrunk to JPEG in the browser before upload. The photo bucket is read-any (so images load on Pages) but unlisted — a photo is only viewable by someone holding its exact random link.

---

## Backend setup

Only needed when the database *shape* changes (new tables or fields). Day-to-day app edits are just a new `index.html` — no key required.

The script is idempotent: it skips anything that already exists, so re-running is safe.

```
cd '/Users/KyleParsons/APPS/Homeward'
npm install node-appwrite          # first time only
APPWRITE_API_KEY=your_key node homeward-setup.mjs
```

Make a temporary API key in the Appwrite console (Project → Overview → Integrations → API Keys) with **Database** and **Storage** scopes. **Delete it when the run finishes** — no standing admin key on disk.

## Deploy

1. Push `index.html` to the repo root on the `main` branch.
2. GitHub Pages auto-deploys (Settings → Pages → Deploy from a branch, `main`, `/root`).
3. New host or address? Add it as a Web platform in Appwrite (Project → Platforms), or logins silently fail.

Cache note: GitHub's CDN can serve an old copy for a few minutes after a push. Append `?v=2` (bump the number) to force a fresh load.

## Config

Top of `index.html`:

- `ENDPOINT` / `PROJECT` — Appwrite project (safe to be public).
- `MEMBERS` — the two names used for assigning and tagging.
- `ALLOWED_EMAILS` — leave `[]` to allow anyone who logs in, or list our two emails to lock it down.
