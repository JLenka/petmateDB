# PetMate — deploy to Vercel

This folder is the complete app. There is no build step: Vercel serves the files as they are.

| File | What it is |
|---|---|
| `index.html` | The whole app. Connects to Supabase project `PetMateDBProject`. |
| `manifest.webmanifest` + `icon-*.png` | Makes PetMate installable ("Add to Home screen" / "Install app"). |
| `favicon.png`, `apple-touch-icon.png` | Browser tab and iPhone home-screen icons. |
| `vercel.json` | Security headers (only Supabase, jsDelivr and Google Fonts are allowed) and caching. |

## Deploy

**From GitHub (recommended, redeploys on every change)**
1. Create a GitHub repository, e.g. `petmate`, and upload every file in this folder to the top level.
2. In Vercel: Add New → Project → import the repository.
3. Set Framework Preset to **Other**. Leave Build Command, Output Directory and Install Command empty.
4. Deploy. Your address will look like `https://petmate.vercel.app`.

**From your computer (no GitHub)**
1. Open a terminal in this folder.
2. Run `npx vercel` and accept the defaults, then run `npx vercel --prod`.

## Then, in Supabase (required)

Authentication → URL Configuration:
- **Site URL:** your Vercel address, e.g. `https://petmate.vercel.app`
- **Redirect URLs:** `https://petmate.vercel.app/**` and, for preview deploys, `https://*.vercel.app/**`

Authentication → Emails → Templates → **Magic Link** and **Confirm signup**: add
`<p>Your PetMate sign-in code: <strong>{{ .Token }}</strong></p>`

Authentication → Emails → SMTP Settings: connect Resend or Brevo before inviting other people.
The built-in sender only emails members of your Supabase team, 2 emails an hour.

## If you use a custom domain or another Supabase project

- New domain: add it to the Supabase Site URL and Redirect URLs.
- Different Supabase project: change `CFG.url` and `CFG.key` in `index.html`, and replace
  `tkptnmtevjutowndtqar.supabase.co` in `vercel.json` with the new project address,
  otherwise the browser blocks the connection.

## Updating

Replace `index.html` with the new version and push (GitHub) or run `npx vercel --prod` again.
`index.html` is served with `no-cache`, so people get the new version on their next visit.
