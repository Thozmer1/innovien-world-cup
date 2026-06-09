# World Cup Standings — share via Vercel

This folder is a complete static site (`index.html`). Anyone with the link can view it; no login needed.

## One-time setup (≈2 min)
**Easiest — drag & drop:**
1. Go to **https://vercel.com/new** and sign in (free; use your Innovien Google/email).
2. Click **"Deploy"** → choose the **"Upload"** / drag-and-drop option.
3. Drag this **`worldcup-dashboard`** folder onto the page.
4. Vercel gives you a public URL like `https://innovien-world-cup.vercel.app` — share that company-wide.

**Or via command line** (if you have Node installed):
```
cd worldcup-dashboard
npx vercel --prod
```

## Updating the standings
The dashboard reads from **World Cup Contest Data Points.xlsx**. To refresh:
1. Enter the latest counts in the yellow columns on the **Manual Inputs** tab and **save** the file.
2. From the `Leadership Reporting` folder run: `python build_dashboard.py`
   (this rewrites `worldcup-dashboard/index.html`).
3. Re-deploy: drag the folder onto vercel.com again, or run `npx vercel --prod`.
   The public URL stays the same.

> Tip: if you connect the folder to a GitHub repo when deploying, Vercel auto-updates
> every time you push the regenerated `index.html` — no re-dragging needed.

## Notes
- Pure static HTML — works on any phone or laptop, no data is sent anywhere.
- The page does **not** expose the raw spreadsheet; only names, roles, groups, and point totals appear.
