# Deploying to GitHub Pages - Agent Tutorial

## Overview

GitHub Pages hosts static websites directly from a GitHub repository. This tutorial covers deploying Vite/React apps (like night-moves) to GitHub Pages.

## Prerequisites

- A GitHub repository under `xmrtdao` org or your personal account
- Node.js and npm installed
- A Vite/React project ready to build

## Step 1: Configure Base Path

In `vite.config.ts`, add the `base` property. For a project site under an organization, use the repo name:

```ts
export default defineConfig({
  base: "/your-repo-name/",  // <-- ADD THIS
  plugins: [react()],
  // ...
});
```

For a user/org site (your-repo-name.github.io), use `base: "/"` instead.

## Step 2: Set Homepage in package.json

```json
{
  "homepage": "https://xmrtdao.github.io/your-repo-name/"
}
```

## Step 3: Build the Project

```bash
npm install
npm run build
```

This generates a `dist/` folder with the static site.

## Step 4: Handle SPA Routing

Single Page Apps need client-side routing. Copy `index.html` as `404.html` so GitHub Pages serves it for unknown paths:

```bash
cp dist/index.html dist/404.html
```

## Step 5: Deploy to gh-pages Branch

Create an orphan branch with just the built files:

```bash
# Create a temp directory
mkdir -p _deploy
cp -r dist/* _deploy/
cp dist/index.html _deploy/404.html

# Init a new git repo with just the deploy files
cd _deploy
git init
git checkout -b gh-pages
git add -A
git commit -m "Deploy to GitHub Pages"
git remote add origin https://github.com/ORG/REPO.git
git push origin gh-pages --force

# Clean up
cd .. && rm -rf _deploy
```

## Step 6: Enable GitHub Pages

In the repo settings on GitHub.com:
1. Go to Settings > Pages
2. Source: Deploy from a branch
3. Branch: `gh-pages` / `/` (root)
4. Save

Your site will be live at `https://ORG.github.io/REPO/` within 2-3 minutes.

## Alternative: Using GitHub Actions

For automatic deployment on every push, add `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build
      - run: cp dist/index.html dist/404.html
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

## Common Issues

| Issue | Fix |
|-------|-----|
| White screen after deploy | Check the `base` path in vite.config.ts - must match the repo name |
| Assets returning 404 | Wrong base path - assets are looking at wrong URL |
| SPA routes return 404 | Missing `404.html` - copy index.html as fallback |
| Build fails on CI | Ensure `npm ci` uses package-lock.json, not `npm install` |

## Real Example: night-moves

- Repo: github.com/xmrtdao/night-moves
- Live: https://xmrtdao.github.io/night-moves/
- Config: vite.config.ts with `base: "/night-moves/"`

## Agent Task

When deploying a Vite/React app to GitHub Pages:
1. Add `base` to vite.config.ts
2. Set `homepage` in package.json
3. Build the project
4. Push `dist/` to `gh-pages` branch
5. Enable Pages in repo settings
6. Verify the site loads at the github.io URL
