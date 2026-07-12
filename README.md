# SurgeLog — Verified Surgical Logbook

Free, globally scoped surgical logbook. Log procedures, get them counter-signed, share a verified portfolio.

## Files

| File | Purpose |
|------|---------|
| `index.html` | Landing page (public) |
| `app.html` | Main logbook app (login required) |
| `portfolio.html` | Public surgeon portfolio (shareable URL) |
| `verify.html` | Supervisor counter-sign page (no login needed) |

## Deploy to GitHub Pages — Step by Step

### 1. Create GitHub repo

1. Go to github.com → New repository
2. Name it `surgelog` (or anything you want)
3. Set to **Public** (required for GitHub Pages free tier)
4. Do NOT initialise with README — you already have files

### 2. Push these files

Open terminal in the folder containing these files:

```bash
git init
git add .
git commit -m "Initial launch"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/surgelog.git
git push -u origin main
```

### 3. Enable GitHub Pages

1. Go to your repo on GitHub
2. Settings → Pages (left sidebar)
3. Source: **Deploy from a branch**
4. Branch: **main** → **/ (root)**
5. Click Save

Your site will be live at:
`https://YOUR_USERNAME.github.io/surgelog/`

Takes ~2 minutes to go live after first push.

### 4. One thing to fix after you know your URL

In `app.html`, the portfolio link generation uses `location.pathname`. This works automatically — no changes needed.

The verification email link also builds itself from the current URL — works automatically on GitHub Pages.

### 5. Custom domain (optional, ~$12/yr)

1. Buy domain on Namecheap or Porkbun
2. In GitHub repo → Settings → Pages → Custom domain → enter your domain
3. At your domain registrar, add a CNAME record pointing to `YOUR_USERNAME.github.io`
4. Enable "Enforce HTTPS" in GitHub Pages settings

## Supabase — One extra step needed

Because the app writes emails via the Resend API from the browser, you need to allow that in Supabase:

1. Go to Supabase → Authentication → URL Configuration
2. Add your GitHub Pages URL to **Site URL**: `https://YOUR_USERNAME.github.io/surgelog/app.html`
3. Add to **Redirect URLs**: `https://YOUR_USERNAME.github.io/surgelog/app.html`

This lets Supabase Auth redirect back to your app correctly after email confirmation.

## Resend — Domain verification

Currently the from address is `noreply@surgelog.app`. Until you have a custom domain and verify it in Resend, emails will send from Resend's shared domain.

To use your own domain:
1. Resend → Domains → Add Domain
2. Add the DNS records they give you to your domain registrar
3. Update the `from` field in `app.html` line ~410 to `noreply@yourdomain.com`

## Tech stack

- **Database + Auth**: Supabase (free tier)
- **Hosting**: GitHub Pages (free)
- **Email**: Resend (free tier, 3000 emails/month)
- **No build step**: pure HTML/JS, push and it's live
