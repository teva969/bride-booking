# GitHub Push & Deploy

Complete GitHub deployment workflow. The user will provide the GitHub repo URL/code if needed.

## Steps

### 1. Push to GitHub

Check git status. If there are uncommitted changes, stage and commit them with a descriptive message, then push to `origin main`. If already up to date, make an empty commit to trigger the workflow:

```bash
git add -A
git commit -m "<descriptive message>"
git push origin main
```

### 2. Create / Update README

Generate a comprehensive `README.md` for the project with the following sections — do NOT skip any:

- **Badges** — tech stack badges using shields.io (HTML5, CSS3, JavaScript, GitHub Pages, etc.)
- **About the Project** — what the site is, who it's for, key features
- **Screenshot** — use the Playwright MCP tool to take a full-page screenshot of the live site (`https://<username>.github.io/<repo>/`) and embed it in the README as `![Screenshot](screenshot.png)`. Save the screenshot as `screenshot.png` in the repo root.
- **File Structure** — directory tree of the repo
- **How to Use** — local dev instructions (python3 -m http.server 8080), form submission notes, how to customise images/colours
- **Live Site** — link to the GitHub Pages URL

Commit and push the README (and screenshot) after writing it.

### 3. Create GitHub Actions Workflow for GitHub Pages

Check if `.github/workflows/deploy.yml` already exists. If not, create it:

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

Commit and push if newly created. Remind the user to enable GitHub Pages under **Settings → Pages → Source → GitHub Actions** if not already done.

### 4. Update GitHub Repo About & Add Live Site URL

Use the GitHub CLI to update the repository description and homepage URL:

```bash
gh repo edit <owner>/<repo> \
  --description "<short description of the project>" \
  --homepage "https://<owner>.github.io/<repo>/"
```

Derive the owner and repo from `git remote get-url origin`.

### 5. Verify Live Site

Fetch the live GitHub Pages URL and confirm it returns the expected content (not a 404).

## Notes

- Always derive the GitHub username and repo name from `git remote get-url origin` — never hardcode them.
- If `gh` CLI is not authenticated, prompt the user to run `! gh auth login`.
- After all steps complete, report the live URL and confirm everything is deployed.
