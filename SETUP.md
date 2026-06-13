# Editing your GitHub-hosted pages from a friendly admin

This kit gives you a content editor at `your-site/admin/` where you log in and edit
pages in a rich-text UI. Saving commits to your repo, GitHub Pages rebuilds, and the
page your Google Site embeds updates — no more hand-editing HTML.

It uses **Sveltia CMS** with **no build step**: content pages are Markdown files
rendered in the browser by `page.html`. Your interactive/app pages are untouched.

---

## What's in the kit

```
admin/index.html      The editor (loads Sveltia from a CDN)
admin/config.yml       What the editor can edit  (you edit 2 lines here)
page.html              Renders a content page in the browser, no build needed
content/welcome.md     An example page to confirm it works
```

Copy all of these into the **root** of your GitHub Pages repo, keeping the folders.

---

## One-time setup (about 10 minutes)

### 1. Point the config at your repo
Open `admin/config.yml` and edit the two marked lines:

```yaml
repo: YOUR-USERNAME/YOUR-REPO     # e.g. janedoe/my-site
branch: main                      # your default branch
```

### 2. Commit the kit to your repo
Add the files (GitHub web UI → "Add file" → "Upload files" works fine) and commit.

### 3. Confirm GitHub Pages is on
Repo → **Settings → Pages**. If it isn't already serving your site, set the source to
your branch (root) and save. Note your site URL, e.g. `https://YOUR-USERNAME.github.io/`.

### 4. Create a personal access token (your login key)
Because you're the only editor, you sign in with a token instead of setting up an OAuth
server.

- GitHub → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**
- **Repository access:** Only select repositories → pick your site repo
- **Permissions → Repository permissions → Contents:** **Read and write**
  (Metadata read-only is added automatically — that's fine)
- Generate, then **copy the token** (you won't see it again)

### 5. Open the editor and log in
Go to `https://YOUR-USERNAME.github.io/admin/`. Choose the **personal access token**
sign-in option and paste your token. You're in.

---

## Daily use

- **Edit a page:** open `/admin/`, pick a page under *Content Pages*, edit, **Save**.
- **New page:** click *New Content Page*, give it a title, write the body, save. The
  filename (slug) comes from the title — e.g. *About Us* → `about-us.md`.
- A minute or so after saving, GitHub Pages finishes rebuilding and the live page updates.

### Show a page on your Google Site
Each content page is viewable at:

```
https://YOUR-USERNAME.github.io/page.html?slug=THE-SLUG
```

So *About Us* (`about-us.md`) is `…/page.html?slug=about-us`.

In Google Sites: **Insert → Embed → By URL**, paste that link, choose full-page embed
as you already do. From now on you edit that page in `/admin/` — never in Sites or raw
HTML again.

---

## Your interactive / app pages

Leave them exactly as they are — standalone HTML files embedded directly. They don't go
through the Markdown renderer (that would flatten them). Keep editing those in GitHub or
with Claude. The CMS handles your **content** pages; your **apps** stay independent. Both
live happily in the same repo.

---

## Later: adding other editors

The token login is per-person and great for a solo owner. If you ever want teammates to
edit without handing out tokens, deploy the free **Sveltia CMS Authenticator** on
Cloudflare Workers and add one line to `config.yml`:

```yaml
backend:
  name: github
  repo: YOUR-USERNAME/YOUR-REPO
  branch: main
  base_url: https://your-worker.workers.dev
```

Then editors click "Sign in with GitHub" instead of pasting a token. Setup instructions:
https://github.com/sveltia/sveltia-cms-auth

---

## Quick troubleshooting

- **`/admin/` is blank:** confirm `admin/index.html` and `admin/config.yml` are both in
  the `admin` folder and Pages has finished building.
- **Login fails:** the token needs **Contents: Read and write** on *this* repo, and the
  `repo:` line must match `owner/name` exactly.
- **Page shows "not found":** the `slug` in the URL must match the filename in `content/`
  (without `.md`).
- **Edits don't appear:** give Pages a minute, then hard-refresh. Check the repo's commit
  history to confirm the save landed.
