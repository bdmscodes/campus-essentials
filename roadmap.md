# Campus Essentials — Gumroad Fork Roadmap

Rebrand the Gumroad monolith as **campusessentials.com.ng** — a marketplace for selling digital items (study notes, documents, templates, media).

**Stack:** Ruby on Rails 8 / Ruby 3.4.3 · React 18 + Inertia + Tailwind 4 · MySQL 8 · Redis · Elasticsearch · MongoDB · MinIO · Stripe/PayPal (at launch).

> **Approach: Path A (stay on the fork).** Keep the full Gumroad codebase, boot it unchanged first, then rebrand + trim for the campus marketplace use case.

---

## Phase 0 — Decisions
- [x] Approach confirmed: **Approach A — keep full fork.**
- [ ] Confirm whether Payments (Stripe/PayPal) use test keys now or are deferred until launch.
- [ ] Confirm the target visual identity (logo, palette, typography) for Campus Essentials.

## Phase 1 — Environment Setup (install dependencies)

> Windows dev is supported ONLY via **WSL + Ubuntu**. Work inside Ubuntu, never PowerShell.

- [ ] Move repo out of OneDrive to, e.g., `C:\dev\campus-essentials` (avoid sync / long-path / permission issues in WSL).
- [ ] Enable long git paths: `git config --system core.longpaths true` (PowerShell as admin).
- [ ] Install **Docker Desktop**; enable WSL integration for Ubuntu.
- [ ] `wsl --install`, launch Ubuntu, create a non-root user.
- [ ] In Ubuntu: install Ruby 3.4.3 via rbenv (per `.ruby-version`).
- [ ] In Ubuntu: install Node 22.22 via nvm (per `.node-version`).
- [ ] In Ubuntu: apt-install MySQL/Percona client libs, ImageMagick, libvips, FFmpeg, pdftk, libxslt, libxml2 (see `docs/development/windows.md`).
- [ ] `gem install bundler` then `bundle install`.
- [ ] `corepack enable` then `npm install`.
- [ ] `cp .env.example .env`; fill in secrets as needed.

## Phase 2 — Boot locally
- [ ] Start Docker services: `LOCAL_DETACHED=true make local`.
- [ ] `bin/rails db:prepare`.
- [ ] `bundle exec rails js:export` then `npm run build:pages-tailwind`.
- [ ] `bin/dev` → visit http://localhost:3000 (subdomains via `*.localhost:3000`).
- [ ] Reindex Elasticsearch (rails console): `DevTools.delete_all_indices_and_reindex_all`.
- [ ] Log in: `seller@gumroad.com` / `password` / 2FA `000000`.

## Phase 3 — Prove the engine (first checkout)
- [ ] Create a sample digital product (e.g., a PDF) as the seller.
- [ ] Complete a test purchase + file download flow as the buyer.
- [ ] Confirm dashboard, CSV payout, and analytics views render. This milestone makes the codebase legible.

## Phase 4 — Rebrand toward campusessentials.com.ng
- [ ] Set `CUSTOM_DOMAIN=campusessentials.com.ng` in dev; validate routing (`config/domain.rb` reads it).
- [ ] Audit + replace Gumroad brand strings: layouts, mailers, discover/storefront, auth pages, emails, logos.
- [ ] Redesign the storefront + seller dashboard visual language using shared views / Tailwind theme.
- [ ] Add a campus digital-items category set + seeded sample products for the notes / study-material use case.

## Phase 5 — Launch prep (only if going live)
- [ ] Wire real Stripe (Connect) + PayPal keys.
- [ ] Custom transactional email domain (e.g. `@campusessentials.com.ng`).
- [ ] HTTPS (TLS) + `assets.campusessentials.com.ng` asset domain.
- [ ] Choose hosting (VPS / Docker self-host); reference `.buildkite/` scripts for deploy patterns.

---

## Testing notes (don't chase the whole suite early)
- Full suite: `bin/rspec`.
- Integration specs need Chrome + Selenium; VCR cassettes are non-trivial.
- Early goal: **boot + change UI**, not a fully green suite.

## Docs worth reading in this repo
- `docs/development/windows.md` — the Windows/WSL walking guide.
- `CONTRIBUTING.md` — evidence bar for product changes (before/after visuals, AI disclosure).
- `docs/testing.md`, `docs/users.md` — auth roles + testing details.

---

*Status: roadmap saved; Approach A approved. Next: Phase 1 — environment setup.*

*Created 8/21/2026.*
