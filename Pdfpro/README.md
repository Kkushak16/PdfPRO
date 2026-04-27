# DocLogic v2 — Deployment Guide

## Project Structure

```
doclogic/
├── index.html          ← Frontend (all UI + JS)
├── api/
│   └── claude.js       ← Vercel serverless proxy (keeps API key secure)
├── vercel.json         ← Vercel routing config
├── .env.example        ← Key template (copy → .env, never commit)
└── .gitignore
```

---

## Deploy to Vercel (Free — Recommended)

### Step 1 — Push to GitHub
```bash
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/YOUR_USERNAME/doclogic.git
git push -u origin main
```

### Step 2 — Import on Vercel
1. Go to https://vercel.com → "Add New Project"
2. Import your GitHub repo
3. Click **Deploy** (default settings are fine)

### Step 3 — Add your API Key
1. In Vercel dashboard → your project → **Settings → Environment Variables**
2. Add:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** `sk-ant-xxxxxxxxxxxxxxxx` (from https://console.anthropic.com)
3. Click **Save**
4. Go to **Deployments** → click the 3 dots → **Redeploy**

Your site is now live with a working API — key secured server-side. ✅

---

## Local Development

```bash
# Install Vercel CLI
npm i -g vercel

# Copy env template
cp .env.example .env
# → Fill in your real key in .env

# Run locally (simulates Vercel serverless functions)
vercel dev
# → Open http://localhost:3000
```

---

## .gitignore (create this file)

```
.env
node_modules/
.vercel/
```

---

## How the API proxy works

```
Browser (index.html)
    │
    │  POST /api/claude  (no key in request)
    ▼
Vercel Serverless Function (api/claude.js)
    │  adds x-api-key from env var (secure, server-side)
    ▼
Anthropic API  →  response back to browser
```

The API key **never touches the browser**. Users cannot extract it.
