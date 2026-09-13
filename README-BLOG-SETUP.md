# Your Blog — Setup Checklist

Everything is scaffolded. You need to do 5 things with your own GitHub account.

## 1. Create the GitHub repo
- Go to github.com/new
- Name it `yourusername.github.io` (replace with your actual username) for the
  simplest free URL, OR any name (e.g. `myblog`) if you're fine with
  `yourusername.github.io/myblog/`
- Make it Public (required for free Pages on personal accounts)
- Do NOT initialize with a README (we already have files)

## 2. Push this project
From inside this `myblog/` folder:
```
git add -A
git commit -m "Initial blog setup"
git branch -M main
git remote add origin https://github.com/yourusername/yourrepo.git
git push -u origin main
```

## 3. Update your real details
Replace placeholders in these files:
- `hugo.yaml` → `baseURL` (your real Pages URL) and `socialIcons` github url
- `static/admin/config.yml` → `repo: yourusername/yourrepo`

If you used a project repo (not `username.github.io`), also update
`.github/workflows/hugo.yml` — no change needed there, it auto-detects
baseURL from GitHub Pages settings.

## 4. Enable GitHub Pages
- Repo → Settings → Pages
- Under "Build and deployment", set Source to **GitHub Actions**
- Push to `main` (or re-run the workflow manually from the Actions tab)
- First build takes ~1-2 minutes. Your site will be live at the Pages URL
  shown in Settings → Pages.

## 5. Enable the CMS dashboard (/admin)
Decap CMS needs OAuth to let you log in with GitHub and commit posts.
Easiest free option — Netlify's Git Gateway:
1. Create a free Netlify account, "Add new site" → "Import an existing
   project" → point it at the same GitHub repo (Netlify will build it too,
   but you can ignore/disable that — you only need it for the Identity +
   Git Gateway service).
2. In the Netlify site dashboard: Site configuration → Identity → Enable
   Identity.
3. Identity → Registration → set to "Invite only" (so only you can log in).
4. Identity → Services → Git Gateway → Enable.
5. Invite yourself as a user under Identity → Invite users.
6. In `static/admin/config.yml`, change the backend to:
   ```yaml
   backend:
     name: git-gateway
     branch: main
   ```
7. Add this script tag to `static/admin/index.html` right before the
   decap-cms script tag:
   ```html
   <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
   ```
8. Visit `yoursite.com/admin`, log in via the invite email, and you're in.

Once logged in: New Post → fill in title/date/category (space,
cybersecurity, or random) → write in the markdown editor → Publish.
Decap commits the Markdown file to your repo, GitHub Actions rebuilds,
and the post goes live automatically within ~1-2 minutes.

## Writing without the CMS (alternative)
You can always skip the dashboard and just add a file directly to
`content/posts/your-post-slug.md` with frontmatter like:
```yaml
---
title: "My Post"
date: 2026-09-13T10:00:00+06:00
draft: false
categories: ["cybersecurity"]
tags: ["security"]
summary: "One line summary"
---
Your content here.
```
Then `git add -A && git commit -m "new post" && git push` — Actions
handles the rest.

## Local preview
```
hugo server -D
```
Visit http://localhost:1313
