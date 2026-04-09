# Deploying Duda Site Scanner to Vercel

This guide gets the app live on a public URL in under 10 minutes.
The app will run in **demo mode** (no real data, no real scans) until you connect Supabase.
That's fine for sharing with the team to evaluate — add the backend later when you're ready.

---

## What you need

- A [Vercel account](https://vercel.com) — free tier is enough
- A [GitHub account](https://github.com) — free
- Node.js installed on your machine ([nodejs.org](https://nodejs.org) — LTS version)
- This `scanner-app` folder

---

## Step 1 — Push the code to GitHub

Open your terminal and run these commands from inside the `scanner-app` folder:

```bash
# Initialise a git repo (if not already)
git init
git add .
git commit -m "Initial commit — Duda Site Scanner"
```

Then go to [github.com/new](https://github.com/new) and create a **private** repository called `duda-site-scanner`. Copy the two commands GitHub shows you under "push an existing repository" — they look like this:

```bash
git remote add origin https://github.com/YOUR-USERNAME/duda-site-scanner.git
git push -u origin main
```

---

## Step 2 — Deploy to Vercel

**Option A — Vercel dashboard (recommended, no CLI needed):**

1. Go to [vercel.com/new](https://vercel.com/new)
2. Click **"Import Git Repository"**
3. Select the `duda-site-scanner` repo you just pushed
4. Vercel will detect Vite automatically — **no settings to change**
5. Click **Deploy**

Done. Vercel will give you a URL like `https://duda-site-scanner-xyz.vercel.app`

**Option B — Vercel CLI:**

```bash
# Install Vercel CLI (one-time)
npm install -g vercel

# Deploy from the scanner-app folder
vercel

# Follow the prompts:
# - Set up and deploy? Y
# - Which scope? (your account)
# - Link to existing project? N
# - Project name: duda-site-scanner
# - Directory: ./  (press Enter)
# - Override settings? N
```

---

## Step 3 — Share the URL

Once deployed, copy the URL from Vercel and share it with your team.

The app runs in **demo mode** by default:
- Login with any `@duda.co` email and password `demo`
- All data is local to each browser session — nothing is shared between users yet
- Scans return realistic mock data instantly — no API calls needed

This is enough for the team to evaluate the tool, run through the flows, and give feedback.

---

## Step 4 (optional) — Add a custom domain

In the Vercel dashboard → your project → Settings → Domains, add something like:
`scanner.duda.co` or `site-scanner.internal.duda.co`

You'll need to add a CNAME record in your DNS settings pointing to `cns1.vercel-dns.com`.

---

## Step 5 (when ready) — Connect real data

When you want real scans and shared team data, see `SETUP.md` for the full Supabase backend setup.
The switch from demo to live mode is just two environment variables — no code changes.

In Vercel dashboard → your project → Settings → Environment Variables, add:
```
VITE_SUPABASE_URL        = https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY   = your-anon-key
```

Then redeploy (Vercel does this automatically on the next git push).

---

## Troubleshooting

**Build fails on Vercel:**
- Make sure `package.json` has `"build": "vite build"` in scripts (it does by default)
- Node version: set to 18.x or 20.x in Vercel → Settings → General → Node.js Version

**White screen after deploy:**
- Check Vercel → your project → Deployments → click latest → Functions tab for errors
- Usually means a missing environment variable — the app handles this gracefully and falls back to demo mode

**Login not working:**
- In demo mode, use any `@duda.co` email and password `demo`
- Make sure you're not on a network that blocks the Google Fonts CDN (the font loads from fonts.googleapis.com)

**App shows "Demo mode" banner even after adding Supabase keys:**
- Make sure you redeployed after adding the env vars (push a new commit or click Redeploy in Vercel)
- Check that `VITE_` prefix is on both variable names — Vercel requires this for Vite apps
