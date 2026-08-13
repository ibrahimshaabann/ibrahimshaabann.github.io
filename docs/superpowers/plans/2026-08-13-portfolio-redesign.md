# Portfolio Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the current generic portfolio with a professional, senior-level single-page site (Light / Direction A) driven by Ibrahim's real resume content and live-project screenshots.

**Architecture:** Static site, no build step. One `index.html`, one `style.css` (tokens + all sections + responsive), one `script.js` (nav toggle + IntersectionObserver reveal). Deployed via GitHub Pages on push to `main`. Verification is visual: render with headless Chrome to a screenshot and inspect.

**Tech Stack:** HTML5, modern CSS (custom properties, grid/flex, `@media`), vanilla JS (IntersectionObserver), Google Fonts (Inter + a monospace) with system fallbacks.

**Reference:** Spec at `docs/superpowers/specs/2026-08-13-portfolio-redesign-design.md`. Read it before starting.

**Verification helper (used in most tasks):**
```bash
google-chrome-stable --headless=new --disable-gpu --no-sandbox --hide-scrollbars \
  --virtual-time-budget=4000 --window-size=1280,1600 \
  --screenshot=/tmp/pf.png "file://$PWD/index.html"
```
Then Read `/tmp/pf.png` to inspect. For mobile, use `--window-size=390,1800`.

---

### Task 1: HTML skeleton + design tokens + resets

**Files:**
- Modify (replace): `index.html`
- Modify (replace): `style.css`
- Delete later (Task 11): `mediaqueries.css`

- [ ] **Step 1: Replace `index.html` with the document shell**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="Ibrahim Shaaban — Backend Engineer. Systems that scale, products that last. FastAPI, Django, PostgreSQL, observability." />
  <title>Ibrahim Shaaban — Backend Engineer</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;680&family=JetBrains+Mono:wght@400;500&display=swap" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <!-- nav (Task 2) -->
  <main>
    <!-- sections (Tasks 3–9) -->
  </main>
  <!-- footer (Task 9) -->
  <script src="script.js"></script>
</body>
</html>
```

- [ ] **Step 2: Replace `style.css` with reset + tokens + base type**

```css
/* ===== Tokens ===== */
:root {
  --surface: #fbfbfa;
  --surface-2: #f4f4f2;
  --text: #16181d;
  --text-2: #4a4d55;
  --hairline: #e6e6e2;
  --hairline-2: #ededea;
  --accent: #2f6b46;
  --chip-bg: #f0f1ee;
  --chip-border: #e2e3de;
  --sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --mono: 'JetBrains Mono', ui-monospace, 'SF Mono', Menlo, Consolas, monospace;
  --maxw: 1080px;
  --pad: clamp(20px, 5vw, 40px);
}

/* ===== Reset ===== */
* { margin: 0; padding: 0; box-sizing: border-box; }
html { scroll-behavior: smooth; -webkit-text-size-adjust: 100%; }
body {
  font-family: var(--sans);
  background: var(--surface);
  color: var(--text);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}
img { max-width: 100%; display: block; }
a { color: inherit; text-decoration: none; }

/* ===== Layout helpers ===== */
.wrap { max-width: var(--maxw); margin: 0 auto; padding: 0 var(--pad); }
.section { padding: clamp(56px, 9vw, 104px) 0; border-top: 1px solid var(--hairline); }
.section:first-of-type { border-top: none; }

/* ===== Shared taste cues ===== */
.idx { font-family: var(--mono); font-size: 11px; letter-spacing: .18em; text-transform: uppercase; color: var(--accent); }
.label { font-family: var(--mono); font-size: 11px; letter-spacing: .12em; text-transform: uppercase; color: var(--text-2); }
.sec-head { display: flex; align-items: baseline; gap: 12px; margin-bottom: clamp(24px, 4vw, 40px); }
.sec-head h2 { font-size: clamp(22px, 3vw, 30px); letter-spacing: -.6px; font-weight: 680; }
```

- [ ] **Step 3: Verify it renders (blank but styled) with no console errors**

Run the verification helper; Read `/tmp/pf.png`. Expected: off-white blank page, no layout errors.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: portfolio skeleton with design tokens (light/direction A)"
```

---

### Task 2: Sticky navigation (desktop + mobile toggle)

**Files:**
- Modify: `index.html` (insert nav before `<main>`)
- Modify: `style.css` (append nav styles)
- Modify (replace): `script.js`

- [ ] **Step 1: Add nav markup** (insert after `<body>`)

```html
<header id="nav">
  <div class="wrap nav-inner">
    <a href="#top" class="wordmark">Ibrahim Shaaban</a>
    <nav class="nav-links" aria-label="Primary">
      <a href="#about">About</a>
      <a href="#experience">Experience</a>
      <a href="#projects">Projects</a>
      <a href="#stack">Stack</a>
      <a href="#contact">Contact</a>
    </nav>
    <button class="nav-toggle" aria-label="Menu" aria-expanded="false"><span></span><span></span></button>
  </div>
</header>
```

- [ ] **Step 2: Append nav styles to `style.css`**

```css
#nav { position: sticky; top: 0; z-index: 50; background: color-mix(in srgb, var(--surface) 88%, transparent); backdrop-filter: blur(8px); transition: border-color .3s; border-bottom: 1px solid transparent; }
#nav.scrolled { border-bottom-color: var(--hairline); }
.nav-inner { display: flex; align-items: center; justify-content: space-between; height: 64px; }
.wordmark { font-weight: 600; letter-spacing: -.3px; }
.nav-links { display: flex; gap: 26px; font-family: var(--mono); font-size: 13px; }
.nav-links a { color: var(--text-2); transition: color .2s; }
.nav-links a:hover { color: var(--text); }
.nav-toggle { display: none; flex-direction: column; gap: 5px; background: none; border: 0; cursor: pointer; padding: 8px; }
.nav-toggle span { width: 22px; height: 2px; background: var(--text); transition: .3s; }
@media (max-width: 720px) {
  .nav-links { position: fixed; inset: 64px 0 auto 0; flex-direction: column; gap: 0; background: var(--surface); border-bottom: 1px solid var(--hairline); max-height: 0; overflow: hidden; transition: max-height .3s; }
  .nav-links.open { max-height: 320px; }
  .nav-links a { padding: 16px var(--pad); border-top: 1px solid var(--hairline-2); }
  .nav-toggle { display: flex; }
}
```

- [ ] **Step 3: Replace `script.js`**

```js
// Mobile nav toggle
const toggle = document.querySelector('.nav-toggle');
const links = document.querySelector('.nav-links');
toggle.addEventListener('click', () => {
  const open = links.classList.toggle('open');
  toggle.setAttribute('aria-expanded', open);
});
links.querySelectorAll('a').forEach(a => a.addEventListener('click', () => links.classList.remove('open')));

// Hairline border on scroll
const nav = document.getElementById('nav');
const onScroll = () => nav.classList.toggle('scrolled', window.scrollY > 8);
onScroll();
window.addEventListener('scroll', onScroll, { passive: true });
```

- [ ] **Step 4: Add `id="top"` anchor** — change `<body>` opening content so the wordmark link target exists: add `<span id="top"></span>` right after `<body>`.

- [ ] **Step 5: Verify** — render desktop (1280px) and mobile (390px). Read both screenshots. Expected: nav bar with wordmark + links (desktop), hamburger visible (mobile).

- [ ] **Step 6: Commit**

```bash
git add index.html style.css script.js
git commit -m "feat: sticky nav with mobile toggle and scroll border"
```

---

### Task 3: Hero (`00`)

**Files:** Modify `index.html` (inside `<main>`), `style.css`.

- [ ] **Step 1: Add hero markup** (first child of `<main>`)

```html
<section id="hero" class="section">
  <div class="wrap hero">
    <p class="idx">Backend Engineer · Cairo</p>
    <h1 class="hero-title">Systems that scale.<br>Products that last.</h1>
    <p class="hero-lead">I take backends from greenfield architecture to production — API design, database performance, infrastructure and deploys, end to end, across AI and SaaS products.</p>
    <div class="tags">
      <span class="tag">FastAPI</span><span class="tag">Django</span><span class="tag">PostgreSQL</span>
      <span class="tag">Docker</span><span class="tag">Redis</span><span class="tag">Grafana</span>
    </div>
    <div class="hero-actions">
      <a class="btn btn-primary" href="#projects">View work</a>
      <a class="btn btn-ghost" href="./assets/Ibrahim-Backend-CV.pdf" target="_blank" rel="noopener">Download CV</a>
    </div>
    <div class="socials">
      <a href="https://github.com/ibrahimshaabann" target="_blank" rel="noopener" aria-label="GitHub">GitHub ↗</a>
      <a href="https://www.linkedin.com/in/ibrahim-shaaban11" target="_blank" rel="noopener" aria-label="LinkedIn">LinkedIn ↗</a>
      <a href="mailto:ibrahimshaaban888@gmail.com" aria-label="Email">Email ↗</a>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append hero + shared button/tag styles**

```css
.hero { padding-top: clamp(40px, 8vw, 90px); }
.hero-title { font-size: clamp(38px, 7vw, 68px); line-height: 1.03; letter-spacing: -2px; font-weight: 680; margin: 16px 0 20px; }
.hero-lead { font-size: clamp(15px, 2vw, 18px); color: var(--text-2); max-width: 52ch; }
.tags { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 26px; }
.tag { font-family: var(--mono); font-size: 12px; padding: 6px 11px; border-radius: 6px; background: var(--chip-bg); border: 1px solid var(--chip-border); color: var(--text-2); }
.hero-actions { display: flex; flex-wrap: wrap; gap: 12px; margin-top: 32px; }
.btn { font-weight: 600; font-size: 14px; padding: 12px 22px; border-radius: 8px; transition: .2s; border: 1px solid transparent; }
.btn-primary { background: var(--accent); color: #fff; }
.btn-primary:hover { background: #245537; }
.btn-ghost { border-color: var(--hairline); color: var(--text); }
.btn-ghost:hover { border-color: var(--text); }
.socials { display: flex; gap: 22px; margin-top: 30px; font-family: var(--mono); font-size: 13px; color: var(--text-2); }
.socials a:hover { color: var(--accent); }
```

- [ ] **Step 3: Verify** — render desktop; Read screenshot. Expected: large headline, lead, chips, two buttons, social row. Confirm CV link path resolves (file exists at `assets/Ibrahim-Backend-CV.pdf` — note the current filename has a trailing space; rename in this step).

```bash
git mv "assets/Ibrahim-Backend-CV .pdf" "assets/Ibrahim-Backend-CV.pdf"
```

- [ ] **Step 4: Commit**

```bash
git add index.html style.css "assets/Ibrahim-Backend-CV.pdf"
git commit -m "feat: hero section + shared buttons/tags; fix CV filename"
```

---

### Task 4: About (`01`) with temp photo

**Files:** Modify `index.html`, `style.css`.

- [ ] **Step 1: Add about markup**

```html
<section id="about" class="section">
  <div class="wrap sec-head"><span class="idx">01</span><h2>About</h2></div>
  <div class="wrap about">
    <div class="about-photo">
      <img src="./assets/Ibrahim1.jpeg" alt="Ibrahim Shaaban" />
    </div>
    <div class="about-body">
      <p>Backend engineer with 2 years taking systems from greenfield architecture to production. I own API design, database performance, infrastructure, and deployment pipelines end to end — always focused on the business problem behind the code: cutting operational errors, accelerating workflows, and keeping systems resilient under real-world load across AI and SaaS products.</p>
      <dl class="facts">
        <div><dt class="label">Location</dt><dd>Cairo, Egypt</dd></div>
        <div><dt class="label">Education</dt><dd>B.Sc. Computer Science — Assiut University (2020–2024)</dd></div>
        <div><dt class="label">GPA</dt><dd>3.46 / B+ · Very Good</dd></div>
        <div><dt class="label">Languages</dt><dd>Arabic (native) · English (professional)</dd></div>
      </dl>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Append about styles (photo cropped + toned via CSS — no file processing)**

```css
.about { display: grid; grid-template-columns: 260px 1fr; gap: clamp(28px, 5vw, 56px); align-items: start; }
.about-photo { border-radius: 12px; overflow: hidden; border: 1px solid var(--hairline); background: var(--surface-2); }
.about-photo img { width: 100%; height: 320px; object-fit: cover; object-position: 50% 22%; filter: grayscale(1) contrast(1.03); transition: filter .4s; }
.about-photo:hover img { filter: grayscale(0); }
.about-body p { font-size: clamp(15px, 1.8vw, 17px); color: var(--text-2); max-width: 60ch; }
.facts { margin-top: 28px; display: grid; grid-template-columns: 1fr 1fr; gap: 18px 32px; }
.facts dt { margin-bottom: 4px; }
.facts dd { font-size: 14px; }
@media (max-width: 720px) { .about { grid-template-columns: 1fr; } .about-photo img { height: 280px; } .facts { grid-template-columns: 1fr; } }
```

- [ ] **Step 3: Verify** — render desktop + mobile; Read screenshots. Confirm photo is cropped to portrait, grayscale, and the busy background is de-emphasized. If the crop shows too much background, adjust `object-position` / `height`.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: about section with cropped/toned temp photo and facts"
```

---

### Task 5: Experience (`02`)

**Files:** Modify `index.html`, `style.css`.

- [ ] **Step 1: Add experience markup**

```html
<section id="experience" class="section">
  <div class="wrap sec-head"><span class="idx">02</span><h2>Experience</h2></div>
  <div class="wrap xp">
    <article class="xp-row">
      <div class="xp-meta"><span class="xp-date">2026 →</span></div>
      <div class="xp-main">
        <h3>Backend Engineer — Zedny Inc.</h3>
        <p class="label">Cairo, Egypt · On-site</p>
        <ul>
          <li>Scalable FastAPI services in a structured layered architecture.</li>
          <li>REST API design for maintainability, modularity, separation of concerns.</li>
          <li>Performance via caching, query optimization, high-performance endpoints.</li>
          <li>Supabase/PostgreSQL + LangGraph for data-driven, AI-powered workflows.</li>
          <li>Observability with Grafana, Loki, Promtail — log aggregation, latency tracking, anomaly detection.</li>
        </ul>
      </div>
    </article>
    <article class="xp-row">
      <div class="xp-meta"><span class="xp-date">2024 – 26</span></div>
      <div class="xp-main">
        <h3>Backend Engineer — Nexxora Holding Ltd. (DigiTee AI)</h3>
        <p class="label">Remote</p>
        <ul>
          <li>Owned greenfield backend architecture for DigiTee AI (Python/Django).</li>
          <li>Secure, extensible APIs for partner and financial integrations.</li>
          <li>Integrated PayPal, QNB, Stripe, MoonPay, Twilio, and X APIs.</li>
          <li>Led the NFT marketplace backend — MetaMask + email auth, fiat-to-crypto payments.</li>
          <li>Managed GitLab CI/CD; collaborated with AI, cybersecurity, and animation teams.</li>
        </ul>
      </div>
    </article>
  </div>
</section>
```

- [ ] **Step 2: Append experience styles**

```css
.xp-row { display: grid; grid-template-columns: 120px 1fr; gap: 24px; padding: 26px 0; border-top: 1px solid var(--hairline-2); }
.xp-row:first-child { border-top: none; }
.xp-date { font-family: var(--mono); font-size: 12px; color: var(--text-2); }
.xp-main h3 { font-size: 18px; font-weight: 600; letter-spacing: -.3px; }
.xp-main .label { margin: 4px 0 12px; }
.xp-main ul { list-style: none; display: grid; gap: 7px; }
.xp-main li { position: relative; padding-left: 18px; font-size: 14.5px; color: var(--text-2); }
.xp-main li::before { content: '—'; position: absolute; left: 0; color: var(--accent); }
@media (max-width: 720px) { .xp-row { grid-template-columns: 1fr; gap: 10px; } }
```

- [ ] **Step 3: Verify** — render; Read screenshot. Expected: two clean rows with mono dates and dashed bullets.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: experience section (Zedny, Nexxora)"
```

---

### Task 6: Projects (`03`) with real screenshots

**Files:** Modify `index.html`, `style.css`. Uses `assets/projects/{aleem,shmaid,zakaria}.png` (already committed).

- [ ] **Step 1: Add projects markup**

```html
<section id="projects" class="section">
  <div class="wrap sec-head"><span class="idx">03</span><h2>Projects</h2></div>
  <div class="wrap projects">
    <article class="proj">
      <a class="proj-shot" href="https://aleemgames.com" target="_blank" rel="noopener"><img src="./assets/projects/aleem.png" alt="Aleem Educational Platform" loading="lazy" /></a>
      <div class="proj-body">
        <div class="proj-top"><h3>Aleem Educational Platform</h3><a class="proj-link" href="https://aleemgames.com" target="_blank" rel="noopener">aleemgames.com ↗</a></div>
        <p>Multi-tenant SaaS learning platform with RBAC for admins, teachers, and students. PayPal subscription billing and admin dashboards; production infra on DigitalOcean with Docker, Nginx, PostgreSQL, SSL, and CI/CD.</p>
        <div class="tags"><span class="tag">Django</span><span class="tag">PostgreSQL</span><span class="tag">Docker</span><span class="tag">DigitalOcean</span><span class="tag">CI/CD</span></div>
      </div>
    </article>
    <article class="proj">
      <a class="proj-shot" href="https://shmaid.org" target="_blank" rel="noopener"><img src="./assets/projects/shmaid.png" alt="SHMAID — Shaalan's Humanitarian Medical Aid" loading="lazy" /></a>
      <div class="proj-body">
        <div class="proj-top"><h3>SHMAID — Humanitarian Medical Aid</h3><a class="proj-link" href="https://shmaid.org" target="_blank" rel="noopener">shmaid.org ↗</a></div>
        <p>Full-stack donation and medical-aid platform. PayPal donation flows, medical-aid request workflows, and role-based admin features; production deployment on Contabo with CI/CD and secure data handling.</p>
        <div class="tags"><span class="tag">React</span><span class="tag">Django</span><span class="tag">PostgreSQL</span><span class="tag">Contabo</span></div>
      </div>
    </article>
    <article class="proj">
      <a class="proj-shot" href="https://dr-zakaria.com" target="_blank" rel="noopener"><img src="./assets/projects/zakaria.png" alt="Dr. Zakaria Academy" loading="lazy" /></a>
      <div class="proj-body">
        <div class="proj-top"><h3>Dr. Zakaria Academy</h3><a class="proj-link" href="https://dr-zakaria.com" target="_blank" rel="noopener">dr-zakaria.com ↗</a></div>
        <p>Full-stack: architected the platform directly with the business owner and built self-hosted video streaming from our own server to cut costs and deliver performance-optimized playback. A courses platform for accounting, finance, and statistics.</p>
        <div class="tags"><span class="tag">Full-Stack</span><span class="tag">Video Streaming</span><span class="tag">Performance</span><span class="tag">Architecture</span></div>
      </div>
    </article>
  </div>
</section>
```

- [ ] **Step 2: Append project styles**

```css
.projects { display: grid; gap: 28px; }
.proj { display: grid; grid-template-columns: 400px 1fr; gap: 28px; align-items: center; padding: 22px; border: 1px solid var(--hairline); border-radius: 14px; background: #fff; transition: box-shadow .25s, transform .25s; }
.proj:hover { box-shadow: 0 10px 40px -20px rgba(0,0,0,.25); transform: translateY(-2px); }
.proj-shot { border-radius: 10px; overflow: hidden; border: 1px solid var(--hairline); display: block; }
.proj-shot img { width: 100%; height: 240px; object-fit: cover; object-position: top; }
.proj-top { display: flex; flex-wrap: wrap; align-items: baseline; justify-content: space-between; gap: 8px; }
.proj-top h3 { font-size: 19px; font-weight: 600; letter-spacing: -.3px; }
.proj-link { font-family: var(--mono); font-size: 12px; color: var(--accent); }
.proj-body p { font-size: 14.5px; color: var(--text-2); margin: 10px 0 14px; }
@media (max-width: 820px) { .proj { grid-template-columns: 1fr; } .proj-shot img { height: 200px; } }
```

- [ ] **Step 3: Verify** — render desktop + mobile; Read screenshots. Confirm all three screenshots load and cards read cleanly. Check images aren't stretched.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: projects section with live screenshots (Aleem, SHMAID, Zakaria)"
```

---

### Task 7: Stack (`04`) matrix

**Files:** Modify `index.html`, `style.css`.

- [ ] **Step 1: Add stack markup**

```html
<section id="stack" class="section">
  <div class="wrap sec-head"><span class="idx">04</span><h2>Stack</h2></div>
  <div class="wrap stack">
    <div class="stack-row"><span class="label">Languages</span><p>Python · SQL</p></div>
    <div class="stack-row"><span class="label">Frameworks</span><p>Django · Django REST Framework · FastAPI · Flask · LangGraph · SQLAlchemy</p></div>
    <div class="stack-row"><span class="label">Databases</span><p>PostgreSQL · MySQL · MongoDB · Redis · Supabase</p></div>
    <div class="stack-row"><span class="label">Cloud / DevOps</span><p>Docker · Kubernetes · Git · GitLab CI/CD · AWS · DigitalOcean · Nginx · Linux</p></div>
    <div class="stack-row"><span class="label">Messaging / Caching</span><p>Redis · Celery · RabbitMQ</p></div>
    <div class="stack-row"><span class="label">Observability</span><p>Grafana · Prometheus · Loki · ELK Stack · Sentry</p></div>
    <div class="stack-row"><span class="label">Testing / Quality</span><p>Pytest · Black · isort · Flake8 · pre-commit</p></div>
  </div>
</section>
```

- [ ] **Step 2: Append stack styles**

```css
.stack { display: grid; gap: 0; }
.stack-row { display: grid; grid-template-columns: 200px 1fr; gap: 24px; align-items: baseline; padding: 16px 0; border-top: 1px solid var(--hairline-2); }
.stack-row:first-child { border-top: none; }
.stack-row p { font-size: 15px; }
@media (max-width: 620px) { .stack-row { grid-template-columns: 1fr; gap: 6px; } }
```

- [ ] **Step 3: Verify** — render; Read screenshot. Expected: tidy label/value matrix, mono labels.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: stack matrix section"
```

---

### Task 8: Community (`05`)

**Files:** Modify `index.html`, `style.css`.

- [ ] **Step 1: Add community markup**

```html
<section id="community" class="section">
  <div class="wrap sec-head"><span class="idx">05</span><h2>Community</h2></div>
  <div class="wrap xp">
    <article class="xp-row">
      <div class="xp-meta"><span class="xp-date">2022 →</span></div>
      <div class="xp-main">
        <h3>TECH Member — Google Developer Group, Assiut</h3>
        <ul>
          <li>Organized 5+ community events reaching 1,500+ students and developers.</li>
          <li>Delivered technical sessions on Python, Django, and software best practices.</li>
        </ul>
      </div>
    </article>
  </div>
</section>
```

- [ ] **Step 2: Verify** — reuses `.xp` styles; render, Read screenshot.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: community section (GDG Assiut)"
```

---

### Task 9: Contact (`06`) + footer

**Files:** Modify `index.html`, `style.css`.

- [ ] **Step 1: Add contact + footer markup** (contact inside `<main>`, footer after `</main>`)

```html
<section id="contact" class="section">
  <div class="wrap sec-head"><span class="idx">06</span><h2>Contact</h2></div>
  <div class="wrap contact">
    <p class="contact-lead">Open to backend and full-stack engineering roles. The fastest way to reach me:</p>
    <div class="contact-grid">
      <a class="contact-card" href="mailto:ibrahimshaaban888@gmail.com"><span class="label">Email</span><span>ibrahimshaaban888@gmail.com</span></a>
      <a class="contact-card" href="https://www.linkedin.com/in/ibrahim-shaaban11" target="_blank" rel="noopener"><span class="label">LinkedIn</span><span>ibrahim-shaaban11 ↗</span></a>
      <a class="contact-card" href="https://github.com/ibrahimshaabann" target="_blank" rel="noopener"><span class="label">GitHub</span><span>ibrahimshaabann ↗</span></a>
      <a class="contact-card" href="tel:+201002555227"><span class="label">Phone</span><span>+20 100 255 5227</span></a>
    </div>
    <a class="btn btn-primary" href="./assets/Ibrahim-Backend-CV.pdf" target="_blank" rel="noopener" style="margin-top:28px;display:inline-block">Download CV</a>
  </div>
</section>
```
```html
<footer class="site-footer">
  <div class="wrap footer-inner">
    <span class="wordmark">Ibrahim Shaaban</span>
    <span class="label">© 2026 · Cairo, Egypt</span>
  </div>
</footer>
```

- [ ] **Step 2: Append contact + footer styles**

```css
.contact-lead { font-size: clamp(16px,2vw,19px); max-width: 46ch; margin-bottom: 26px; }
.contact-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; max-width: 640px; }
.contact-card { display: flex; flex-direction: column; gap: 6px; padding: 16px 18px; border: 1px solid var(--hairline); border-radius: 10px; font-size: 14px; transition: border-color .2s; }
.contact-card:hover { border-color: var(--accent); }
.site-footer { border-top: 1px solid var(--hairline); padding: 28px 0; }
.footer-inner { display: flex; align-items: center; justify-content: space-between; }
@media (max-width: 620px) { .contact-grid { grid-template-columns: 1fr; } }
```

- [ ] **Step 3: Verify** — render; Read screenshot. Confirm all links present, mailto/tel correct.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: contact section and footer"
```

---

### Task 10: Scroll-reveal motion (accessible)

**Files:** Modify `script.js`, `style.css`.

- [ ] **Step 1: Append reveal styles**

```css
@media (prefers-reduced-motion: no-preference) {
  .reveal { opacity: 0; transform: translateY(16px); transition: opacity .6s ease, transform .6s ease; }
  .reveal.in { opacity: 1; transform: none; }
}
```

- [ ] **Step 2: Append reveal JS to `script.js`**

```js
// Scroll reveal
const reveals = document.querySelectorAll('.section > .wrap, .proj');
reveals.forEach(el => el.classList.add('reveal'));
if (window.matchMedia('(prefers-reduced-motion: no-preference)').matches) {
  const io = new IntersectionObserver((entries) => {
    entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target); } });
  }, { threshold: 0.12 });
  reveals.forEach(el => io.observe(el));
} else {
  reveals.forEach(el => el.classList.add('in'));
}
```

- [ ] **Step 3: Verify** — render (headless renders final state; reveal elements should end visible). Read screenshot to confirm nothing is stuck at opacity 0. Also load `file://.../index.html` mentally: with virtual-time-budget the observer fires. If content appears blank, ensure the fallback `.in` is applied.

- [ ] **Step 4: Commit**

```bash
git add script.js style.css
git commit -m "feat: accessible scroll-reveal animations"
```

---

### Task 11: Responsive pass, cleanup, and final verification

**Files:** Delete `mediaqueries.css`; prune unused assets; final `index.html`/`style.css` review.

- [ ] **Step 1: Remove the now-unused old stylesheet and stale assets**

```bash
git rm mediaqueries.css
git rm assets/arrow.png assets/checkmark.png assets/education.png assets/experience.png \
       assets/project-1.png assets/project-2.png assets/project-3.png \
       assets/IbrahimSoftwareEngineer1.pdf assets/IbrahimWedding.jpeg
```
Keep: `assets/Ibrahim1.jpeg`, `assets/Ibrahim2.jpeg`, `assets/Ibrahim-Backend-CV.pdf`, `assets/projects/*`, `assets/github.png`, `assets/linkedin.png`, `assets/email.png` (only if referenced — with text links they may be unused; remove if so).

- [ ] **Step 2: Confirm no dangling references**

Run: `grep -oE '(src|href)="\./assets/[^"]+"' index.html | sort -u` and verify every referenced file exists (`ls` each). Expected: only `Ibrahim1.jpeg`, `Ibrahim-Backend-CV.pdf`, `projects/*.png`.

- [ ] **Step 3: Full-page verification at three widths**

Render at `1280x1600`, `820x1800`, `390x2000`; Read all three screenshots. Checklist:
- Nav collapses to hamburger < 720px
- No horizontal scroll / overflow at any width
- Hero, About photo, Experience, all 3 project cards, Stack, Community, Contact all present and legible
- Accent green consistent; hairlines crisp

Fix any overflow/spacing issues in `style.css` inline, re-render, re-check.

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "chore: responsive pass, remove old stylesheet and unused assets"
```

---

### Task 12: Deploy verification

- [ ] **Step 1:** Confirm on `main` (this is a `username.github.io` repo — root serves directly). Push:

```bash
git push origin main
```

- [ ] **Step 2:** Wait ~1 min, open https://ibrahimshaabann.github.io in a browser (or headless screenshot the live URL) and confirm the new site is live, CV downloads, and all three project links open.

- [ ] **Step 3:** Report the live URL to Ibrahim for final review.

---

## Notes for the implementer

- **No test framework** — verification is visual via headless Chrome screenshots. Inspect each after building.
- Keep everything in the three root files; do not introduce a build step.
- Content is authoritative in the spec — don't invent facts. If something's missing, ask rather than guess.
- The photo is temporary; keep the swap to a single `<img src>` change.
