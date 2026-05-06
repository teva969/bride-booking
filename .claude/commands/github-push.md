# GitHub Push & Deploy

Push this project to GitHub, generate a README with a live screenshot, deploy via GitHub Pages, and update the repo's About section.

**Usage:** `/github-push <github-repo-url>`

The user must pass their GitHub repo URL as `$ARGUMENTS` (e.g. `https://github.com/username/repo`). If no argument is given, stop and ask for it before continuing.

---

## Step 1 — Set up the remote

Parse `OWNER` and `REPO` from `$ARGUMENTS`.

Check whether `origin` is already set:
```bash
git remote get-url origin 2>/dev/null
```

- If missing → `git remote add origin $ARGUMENTS`
- If pointing elsewhere → `git remote set-url origin $ARGUMENTS`

---

## Step 2 — Push to GitHub

Stage all changes, commit, and push:

```bash
git add -A
git commit -m "Initial commit" --allow-empty
git push -u origin main
```

If `gh` CLI is not authenticated, stop and tell the user to run `gh auth login`.

---

## Step 3 — Take a screenshot with Playwright MCP

Start the local server if it is not already running:
```bash
python3 -m http.server 8080 &
sleep 2
```

Use the **Playwright MCP** tool to:
1. Navigate to `http://localhost:8080`
2. Take a full-page screenshot
3. Save it as `screenshot.png` in the repo root

---

## Step 4 — Write README.md

Create `README.md` in the repo root with **all** of the following sections in order:

### Badges
Shields.io badges for every technology used, e.g.:

```markdown
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=for-the-badge&logo=github&logoColor=white)
```

### About the Project
What the site is, who it is for, and its key features. Write 3–5 sentences specific to this codebase.

### Screenshot
```markdown
![Screenshot](screenshot.png)
```

### File Structure
Run `find . -not -path '*/.git/*' -not -name '.DS_Store' | sort` and render the output as a code block.

### How to Use
Step-by-step instructions:
1. Clone the repo
2. `python3 -m http.server 8080` then open `http://localhost:8080`
3. How to change the contact email (update the `fetch` URL in `index.html`)
4. How to swap photos (replace Unsplash URLs)
5. How to change colours (edit CSS custom properties at the top of `<style>`)
6. Note about formsubmit.co activation email on first submission

### Live Site
```markdown
[View Live Site](https://<OWNER>.github.io/<REPO>/)
```

---

## Step 5 — Commit and push README + screenshot

```bash
git add README.md screenshot.png
git commit -m "Add README with screenshot"
git push origin main
```

---

## Step 6 — Create GitHub Actions workflow

Check whether `.github/workflows/deploy.yml` already exists. If it does, skip this step.

If not, create it:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: .
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Commit and push if newly created:
```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions workflow for GitHub Pages"
git push origin main
```

Tell the user to enable GitHub Pages at:
`https://github.com/<OWNER>/<REPO>/settings/pages` → Source → **GitHub Actions**

---

## Step 7 — Update repo About

```bash
gh repo edit <OWNER>/<REPO> \
  --description "<one-line description written from the README About section>" \
  --homepage "https://<OWNER>.github.io/<REPO>/"
```

---

## Step 8 — Confirm live site

Wait ~60 seconds, then check:
```bash
curl -sI "https://<OWNER>.github.io/<REPO>/" | head -1
```

Report the result and print the live URL for the user.
