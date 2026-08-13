# Portfolio Redesign — Design Spec

**Date:** 2026-08-13
**Owner:** Ibrahim Shaaban
**Goal:** Replace the current generic portfolio with a professional, senior-level single-page portfolio that reads credibly to recruiters and distinctly "software engineer" to fellow developers — the engineering taste expressed through *craft and detail*, not gimmicks.

## Positioning

- **Role:** Backend Engineer
- **Headline:** "Systems that scale. Products that last."
- **One-liner:** Takes backends from greenfield architecture to production — API design, database performance, infrastructure, and deploy pipelines end to end, across AI and SaaS products.
- **Location:** Cairo, Egypt

## Direction (approved)

Light, editorial, premium — "Direction A" from the visual brainstorm. Same design system throughout.

**Taste cues that signal engineering discipline:**
- Monospace section indices (`00`–`06`) and micro-labels
- Monospace metadata (dates, category tags)
- Hairline dividers, strict grid, generous whitespace
- Tight, precise display type; restrained motion

## Design tokens

- **Surface:** `#fbfbfa` (off-white)
- **Text:** `#16181d` primary, `#4a4d55` secondary
- **Hairline:** `#e6e6e2` / `#ededea`
- **Accent:** refined forest green `#2f6b46`
- **Chip bg:** `#f0f1ee`, border `#e2e3de`
- **Type:** Inter (or system sans) for UI/body; a monospace (SF Mono / ui-monospace stack) for labels, indices, dates, tags. Loaded via Google Fonts or system stack (no build step).
- **Radius:** ~6–14px. **Max content width:** ~ 1000–1100px, centered.

## Tech approach

- **Static, no build step.** Plain `index.html` + `style.css` + `script.js` in repo root (GitHub Pages `username.github.io`). Deploys on push.
- The old `index.html`, `style.css`, `mediaqueries.css`, `script.js` are **fully replaced** (media queries folded into `style.css` or a single stylesheet). Old generic assets (checkmark/arrow/experience/education icons, project-1/2/3 placeholders) removed or left unused.
- Vanilla JS only: mobile nav toggle, smooth-scroll (CSS), IntersectionObserver scroll-reveal.
- Responsive: desktop, tablet, mobile (single-column collapse; hamburger nav).
- Accessibility: semantic landmarks, alt text, keyboard-focus states, `prefers-reduced-motion` disables reveal animations, WCAG-AA contrast.

## Page structure (single scrolling page)

**Sticky nav** — "Ibrahim Shaaban" wordmark (left); `About · Experience · Projects · Stack · Contact` (right, monospace). Hairline bottom border appears on scroll. Collapses to hamburger on mobile.

**`00` Hero**
- Eyebrow: `Backend Engineer · Cairo` (mono, accent)
- H1: "Systems that scale. Products that last."
- Lead: the one-liner
- Tech tag chips: FastAPI, Django, PostgreSQL, Docker, Grafana (+ a few)
- Actions: **View work** (scrolls to Projects), **Download CV** (`assets/Ibrahim-Backend-CV.pdf`)
- Social icons: GitHub, LinkedIn, email

**`01` About**
- Condensed narrative from the professional summary (2 yrs, greenfield→production, AI + SaaS, resilient under load).
- Facts rail: Cairo · B.Sc. Computer Science, Assiut University (2020–2024) · GPA 3.46 / B+ · Arabic (native), English (professional).
- **Photo:** `assets/Ibrahim1.jpeg` cropped to a head-and-shoulders portrait with a subtle refined treatment (grayscale or faint green duotone) to fit the light palette; modest size. Flagged: a plain-background headshot would be a one-line future swap.

**`02` Experience** — structured rows, monospace dates, concise bullets:
- **Zedny Inc.** — Backend Engineer · Feb 2026–Present · Cairo (On-site). FastAPI + layered architecture; REST API design; caching + query optimization; Supabase/PostgreSQL + LangGraph for AI workflows; observability with Grafana/Loki/Promtail.
- **Nexxora Holding Ltd. (DigiTee AI)** — Backend Engineer · Sep 2024–Jan 2026 · Remote. Greenfield Django backend; secure partner/financial APIs; integrations: PayPal, QNB, Stripe, MoonPay, Twilio, X; NFT marketplace backend (MetaMask + email auth, fiat-to-crypto); GitLab CI/CD.

**`03` Projects** — cards with real screenshots (captured to `assets/projects/`), description, tech, live link + role:
- **Aleem Educational Platform** — `aleem.png` → aleemgames.com. Multi-tenant SaaS LMS, RBAC (admin/teacher/student), PayPal subscription billing, admin dashboards. Django · PostgreSQL · Docker · DigitalOcean · Nginx · CI/CD.
- **SHMAID — Shaalan's Humanitarian Medical Aid** — `shmaid.png` → shmaid.org. Full-stack donation + medical-aid platform, PayPal donation flows, aid-request workflows, role-based admin. React · Django · PostgreSQL · Contabo · CI/CD.
- **Dr. Zakaria Academy** — `zakaria.png` → dr-zakaria.com. Courses/learning platform (accounting, finance & statistics). (Role: backend/full-stack — confirm one-line description.)

**`04` Stack** — categorized matrix (mono category labels, not cards):
- Languages: Python, SQL
- Frameworks: Django, DRF, FastAPI, Flask, LangGraph, SQLAlchemy
- Databases: PostgreSQL, MySQL, MongoDB, Redis, Supabase
- Cloud/DevOps: Docker, Kubernetes, Git, GitLab CI/CD, AWS, DigitalOcean, Nginx, Linux
- Messaging/Caching: Redis, Celery, RabbitMQ
- Observability: Grafana, Prometheus, Loki, ELK, Sentry
- Testing/Quality: Pytest, Black, isort, Flake8, pre-commit

**`05` Community** — GDG Assiut, TECH Member (Jun 2022–Present): organized 5+ events reaching 1,500+ students/devs; delivered sessions on Python, Django, best practices.

**`06` Contact** + footer
- Email: ibrahimshaaban888@gmail.com
- LinkedIn: linkedin.com/in/ibrahim-shaaban11
- GitHub: github.com/ibrahimshaabann
- Phone: +20 100 255 5227
- Cairo, Egypt · Download CV
- Footer: wordmark + updated copyright (© 2026 Ibrahim Shaaban).

## Motion

- IntersectionObserver fade/translate reveal on section entry (respect `prefers-reduced-motion`)
- Smooth anchor scrolling
- Hairline/underline hover states on nav and links; subtle card hover lift on projects

## Out of scope (YAGNI)

- No framework/build tooling, no CMS, no dark-mode toggle (light only), no blog, no contact-form backend (mailto link only), no analytics.

## Open items to confirm during build

1. One-line description + your exact role on **Dr. Zakaria Academy**.
2. Photo treatment acceptable (cropped + grayscale/duotone), or go type-only after seeing it.
