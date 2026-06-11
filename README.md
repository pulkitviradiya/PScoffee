# P.S. Coffee — Website

A pre-launch marketing site for **P.S. Coffee** — an instant-coffee brand without the instant coffee. 100% Arabica specialty espresso, ready before you arrive, from ₹89.

This is a **static site**: plain HTML, CSS, and JavaScript. **No build step, no framework, no dependencies.** It can be deployed as-is to any static host. These are production-ready files, not a prototype to re-implement.

---

## 🚀 Deploy to GitHub + Vercel

### What you're deploying
A static multi-page site. Everything in this folder ships exactly as-is. There is nothing to compile or transpile.

### Step 1 — Push to GitHub
From inside this folder:

```bash
git init
git add .
git commit -m "P.S. Coffee website — initial deploy"
git branch -M main
git remote add origin https://github.com/<your-username>/ps-coffee-site.git
git push -u origin main
```

### Step 2 — Deploy to Vercel

**Option A — Vercel dashboard (easiest)**
1. Go to https://vercel.com/new
2. Import the GitHub repo you just pushed.
3. **Framework Preset:** select **Other** (or "No Framework").
4. **Build Command:** leave empty.
5. **Output Directory:** leave as the root (`./`).
6. Click **Deploy**. Done.

**Option B — Vercel CLI**
```bash
npm i -g vercel
vercel        # preview deploy
vercel --prod # production deploy
```
When prompted, accept the defaults — there is no build command and the output directory is the project root.

`vercel.json` is already included. It enables clean URLs (so `/menu` works as well as `/menu.html`) and sets long-cache headers on the `/assets` folder.

---

## 📁 Project structure

```
ps-coffee-site/
├── index.html          # Home — dark editorial hero, P.S. Pack block, full scroll
├── pack.html           # The P.S. Pack — subscription, 3-tier pricing wall
├── matcha.html         # Matcha — standalone green hero, grades table
├── menu.html           # Full menu — coffee, matcha & food, with filters
├── app.html            # The app — 3-tap ordering, phone mockups
├── about.html          # About / brand philosophy
├── story.html          # Founding story
├── partnership.html    # B2B — Host a Pod
├── assets/
│   ├── ps.css          # Shared design system (all tokens, components, layout)
│   ├── ps.js           # Shared behaviour (nav, footer, loader, forms, tweaks)
│   └── image-slot.js   # Drag-and-drop image placeholder web component
├── vercel.json         # Vercel config (clean URLs + asset caching)
└── .gitignore
```

Every page links to the two shared files in `assets/`. The navigation bar and footer are **injected by `ps.js`** — to add/remove/reorder nav items or footer links, edit the `PAGES` array and footer HTML near the top of `assets/ps.js` (it propagates to every page automatically).

---

## 🖼️ Images — IMPORTANT (read before launch)

The site currently uses a custom `<image-slot>` web component for **every photo**. These render as labelled placeholders (e.g. "Hero — signature cup", "Iced Matcha Latte") and let you drag an image in **locally in the browser** — but that image is stored in the visitor's own browser storage (`localStorage`), **not on the server**. This is intentional for the design/review phase.

**Before going live, replace the placeholders with real images.** For each `<image-slot ...></image-slot>`:

1. Add the real photo to an `images/` folder in this project.
2. Replace the slot with a standard tag, e.g.:
   ```html
   <!-- from -->
   <image-slot id="hero-1" placeholder="Hero — signature cup" fit="cover"></image-slot>
   <!-- to -->
   <img src="images/hero-cup.jpg" alt="P.S. Coffee signature cup" style="width:100%;height:100%;object-fit:cover" />
   ```
3. Once **all** slots are replaced, you can remove the `<script src="assets/image-slot.js"></script>` line from each page.

### Photo shot list (what to supply)
**Hero / brand:** home hero (signature cup, dark, portrait 4:5), matcha hero (iced matcha, green, 4:5), Pack hero.
**Home category cards:** Coffee · Matcha · Food (square).
**Products (square, consistent lighting):** Cappuccino, Cold Brew, Iced Matcha Latte, Butter Croissant; matcha line-up — Hot Matcha, Iced Matcha, Ceremonial Usucha, Matcha Cold Foam, Strawberry Matcha, Matcha Tonic.
**Editorial:** "meet the cup" close-up on cream, a Pod / compact counter in a real building, matcha-band lifestyle shot.
**Other pages:** menu drink grid, app phone screens, about/story founder & space shots, partnership space shots.

---

## 📝 Forms

The feedback and waitlist forms (on `index.html`) currently **validate and show a success state on the client only** — they do not POST anywhere yet. To collect real submissions, wire the `data-ps-form` handler in `assets/ps.js` (search for `data-ps-form`) to a backend or form service (Formspree, Vercel serverless function, Google Form endpoint, etc.).

---

## 🎨 Design system at a glance

- **Canvas:** Steam Cream `#F4EEE3` · raised surfaces `#FCFAF5` · warm sand panels `#EBE3D4`
- **Ink:** warm espresso near-black `#16140F`
- **Accent:** Electric Terracotta `#E8400C`
- **Matcha sub-palette:** `#2F6B43` / `#4E8C5C` / tint `#E4EBDB`
- **Type:** Archivo (display / headings) + Hanken Grotesk (body), loaded from Google Fonts
- All tokens are CSS custom properties at the top of `assets/ps.css` (`:root { … }`) — change them in one place to retheme the whole site.

A built-in **Tweaks** panel (toggle in `ps.js`) lets you live-preview accent colour, canvas tone, type scale, and button radius; chosen values persist in `localStorage`.

---

## Local preview

No server strictly required, but for clean URLs and correct relative paths:

```bash
npx serve .
# or
python3 -m http.server 8000
```

Then open http://localhost:8000.
