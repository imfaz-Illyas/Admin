# Deploying PULSE to Vercel

## Why you saw `404 NOT_FOUND`

Vercel serves a file called **`index.html`** at the root URL by default. Your project's homepage is named `pulse.html`, so when someone visits `your-project.vercel.app/` Vercel can't find a matching file → it falls through to its 404 page.

The fix is two new files in this folder (already created for you):

- **`vercel.json`** — config that maps `/` → `pulse.html`, `/admin` → `pulse-admin.html`, `/email` → `email-preview.html`
- **`index.html`** — a tiny redirect to `pulse.html` (insurance — works even if `vercel.json` is ignored)

## Step-by-step fix

1. Make sure these files are all in the **same folder** (the one you're deploying):

   ```
   pulse-deploy/
   ├── index.html              ← new (redirect)
   ├── vercel.json             ← new (config)
   ├── pulse.html              ← public site
   ├── pulse-admin.html        ← admin dashboard
   ├── email-preview.html      ← email mockup
   └── (anything else)
   ```

2. **Push or redeploy** to Vercel. Two ways:

   **Option A — Vercel CLI (fastest)**

   ```bash
   cd pulse-deploy
   npx vercel --prod
   ```

   **Option B — Git**

   Commit the new files (`index.html`, `vercel.json`) and push to the branch connected to your Vercel project. Vercel will auto-deploy.

   **Option C — Drag & drop**

   In the Vercel dashboard → your project → **Deployments** tab → **Redeploy** → drag the whole folder in.

3. Visit `your-project.vercel.app/` — it should now load PULSE.

## Vercel project settings

In the Vercel dashboard → your project → **Settings → Build & Development Settings**:

| Setting | Value |
|--|--|
| **Framework Preset** | Other (or "No framework") |
| **Build Command** | _(leave empty)_ |
| **Output Directory** | _(leave empty — defaults to project root)_ |
| **Install Command** | _(leave empty)_ |
| **Development Command** | _(leave empty)_ |

If any of these are filled in (e.g. you set "Next.js" earlier), clear them and redeploy.

## URLs after deployment

- `your-project.vercel.app/` → public site (homepage)
- `your-project.vercel.app/admin` → admin dashboard
- `your-project.vercel.app/email` → email mockup
- `your-project.vercel.app/pulse.html` → also works (direct file access)
- `your-project.vercel.app/pulse-admin.html` → also works

## Important — about data sharing

The public site and the admin share data via `localStorage` and `BroadcastChannel`, which **only sync between tabs of the same origin**. On Vercel they're served from the same domain, so this works perfectly. You don't need a backend just to make admin edits show up on the public site — but **for production you absolutely should add one** so:

- Data persists for ALL visitors (not just one browser)
- Admin changes are visible to everyone, not just the admin's own browser
- Bookings and pay-per-inquiry charges actually go through Stripe
- Inquiry emails actually send via Resend

The Next.js scaffold I built earlier under `pulse/` (a separate folder) is the production path — it has API routes, Sanity CMS hooks, NextAuth, Resend integration, all wired up.

## If you still see 404 after these steps

1. Check that `index.html` is in the **root** of what you deployed (not a subfolder).
2. Open Vercel dashboard → **Deployments** → click the latest deployment → **Source** tab → confirm you can see `index.html` and `vercel.json` listed there. If not, they didn't get uploaded.
3. Hard-refresh the deployed URL: `Ctrl/Cmd + Shift + R`.
4. Open Vercel dashboard → **Logs** tab and look for "404 NOT_FOUND" entries — the path it's looking for tells you what's missing.
