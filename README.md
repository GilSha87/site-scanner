# Duda Site Scanner

Internal AM tool for monitoring client websites with AI-powered scan reports.

## Quick start (2 minutes)

```bash
npm install
npm run dev
```

Open http://localhost:5173 — the app runs in **demo mode** with realistic mock data. No keys needed.

**Demo login:** any `@duda.co` email + password `demo`
(e.g. `gil@duda.co` / `demo`)

---

## Going live (add real data)

### Step 1 — Connect Supabase

1. Create a project at [supabase.com](https://supabase.com)
2. Run `supabase/migrations/001_initial_schema.sql` in the SQL editor
3. Copy `.env.example` → `.env` and fill in:

```
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

### Step 2 — Deploy Edge Functions + Anthropic key

```bash
supabase link --project-ref YOUR_PROJECT_REF
supabase secrets set ANTHROPIC_KEY=sk-ant-...
supabase functions deploy run-scan
supabase functions deploy process-jobs
supabase functions deploy invite-member
```

### Step 3 — Enable emails (optional, add later)

```bash
supabase secrets set SENDGRID_KEY=SG.your_key
```

Then in `.env`:
```
VITE_EMAIL_ENABLED=true
```

See `SETUP.md` for the full guide.

---

## Project structure

```
src/
├── main.js              # App shell, router, scan runner
├── lib/
│   ├── data.js          # Supabase ↔ demo mode abstraction
│   ├── demo-data.js     # Seed data for demo mode
│   ├── store.js         # Global state
│   ├── utils.js         # Helpers, toast, modal
│   └── icons.js         # SVG icon strings
├── pages/
│   ├── login.js
│   ├── dashboard.js
│   ├── clients.js
│   ├── scans.js         # Scan list + full report viewer
│   ├── team.js          # Team management (admin only)
│   └── settings.js
└── styles/
    └── main.css
```

## Features

- **Login** — `@duda.co` domain enforced, suspended accounts blocked
- **Dashboard** — stats, recent scans, site status, due alerts
- **Clients** — all clients visible; staff edit own, admins edit all
- **Scan reports** — changes, new content, improvements, score breakdown, AM talking points, client-facing summary, email preview
- **Team** — invite with domain-locked email input, role picker (Admin/Staff), suspend/reactivate/remove
- **Settings** — email status, AI engine status, data export (admin only)
- **Demo mode** — full UI with realistic data, no keys required
# site-scanner2-
# site-scanner2-
