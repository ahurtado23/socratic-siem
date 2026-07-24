# SOCratic Method — SIEM Selection Console

A single page, interactive console that walks through how to choose the right SIEM for your org — built as a live talk track companion, not a slide deck.

**Live demo:** https://ahurtado23.github.io/socratic-siem/ (password protected — ask Alex for access)

## What this is

Most "how to pick a SIEM" content is a checklist in a blog post. This project turns that decision process into something you can click through like a real SOC console: connectors, a rule library, access control, live findings, and a guided recommender that asks the same questions you'd actually ask a vendor.

It's a presentation tool first — every "connector," "rule," and "user" on screen is illustrative sample data, not a live security tool.

## Features

- **Overview** — session summary and content covered
- **Dashboard** — sample telemetry (event volume, detections, identity signal)
- **Connectors** — a mock data source inventory (Application / Identity / SIEM / Cloud / Endpoint / Network) with an "Add a Source" flow
- **Rule Library** — sample detection rules with playbook, rule logic, and change history per rule
- **Findings** — trends shaping SIEM buying decisions right now
- **Access Control** — role based access control (RBAC) example: roles, a permissions matrix, and a user list
- **Cases** — a sample investigation case study
- **Insights** — how to actually measure SIEM effectiveness
- **Recommender** — a guided Q&A that recommends a SIEM approach based on your answers (see [docs/RECOMMENDER-LOGIC.md](docs/RECOMMENDER-LOGIC.md) for the decision tree behind it)
- **Contact** — takeaways (slides, infographic) and links

## Tech stack

Single self contained `index.html` — no build step, no dependencies, no framework. Inline CSS and vanilla JS render everything client side (including small inline SVG charts). This is intentional: it means the whole project can be opened directly in a browser or dropped onto any static host with zero setup.

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for how the page/nav/modal system is put together.

### Mobile and PWA

The layout is responsive down to phone width (the sidebar collapses into a slide out drawer under 880px), and the site is installable as a Progressive Web App — `manifest.json` and `service-worker.js` sit alongside `index.html` to enable "Add to Home Screen" and offline caching once served over HTTPS. Both are ignored on a plain `file://` open, service worker registration only runs on HTTPS or localhost, so nothing changes about opening the file directly.

## Running locally

```bash
git clone git@github.com:ahurtado23/socratic-siem.git
cd socratic-siem
open index.html
```

No server, no `npm install` — it's just HTML.

## Access

The live site is gated behind a simple client side password prompt (see the `PASSWORD` variable near the top of `<body>` in `index.html`). This is a casual deterrent, not real access control — the source (and the password) is visible to anyone who reads the repo. Treat it accordingly.

## Docs

- [Architecture](docs/ARCHITECTURE.md) — how the single file app is structured
- [Recommender Logic](docs/RECOMMENDER-LOGIC.md) — the checkpoint by checkpoint decision tree behind the Recommender page
