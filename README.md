# Kazakhstan trip — Aug 1–7, 2026

Static one-page itinerary, deployed to GitHub Pages. Edit `index.html`, push to `main`, the live site updates in ~30–90 seconds. Share one URL with your travel companion; they refresh to see changes.

## One-time setup

1. **Create the repo on GitHub.** Name it whatever you like (e.g. `kz-trip`). Public is fine; if you go private, you'll need GitHub Pro for Pages — public is simpler.

2. **Push this folder to it.** From inside this `kz-trip/` folder:

   ```bash
   git init -b main
   git add .
   git commit -m "Initial itinerary"
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```

3. **Enable Pages.** On GitHub: **Settings → Pages → Build and deployment → Source: GitHub Actions.** That's it — no branch picker, the included workflow handles deploys.

4. **Wait for the first deploy.** Open the **Actions** tab. The "Deploy to GitHub Pages" run takes ~30–60 seconds. When it's green, your site is live at:

   ```
   https://<your-username>.github.io/<repo-name>/
   ```

5. **Share that URL with your travel companion.** Tell them to bookmark it.

## Pushing updates

Edit `index.html`, then:

```bash
git add index.html
git commit -m "Swap Day 5 lunch spot"
git push
```

The Action re-runs automatically. Your companion's bookmark refreshes to the new version.

## Optional: custom domain

If you own a domain, add a `CNAME` file at the repo root containing just your domain (e.g. `kz.malcolm.travel`), and configure DNS per [GitHub's docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site). The Action will pick it up.

## Optional: ping your companion when you push

Add a second job to `.github/workflows/pages.yml` that fires a webhook on push — e.g. a Telegram bot, Discord webhook, or Slack incoming webhook. A minimal Telegram example:

```yaml
  notify:
    needs: deploy
    runs-on: ubuntu-latest
    steps:
      - name: Telegram ping
        run: |
          curl -s -X POST "https://api.telegram.org/bot${{ secrets.TG_TOKEN }}/sendMessage" \
            -d chat_id=${{ secrets.TG_CHAT }} \
            -d text="Itinerary updated: ${{ github.event.head_commit.message }}"
```

(Add `TG_TOKEN` and `TG_CHAT` under repo **Settings → Secrets and variables → Actions**.)

## Files

- `index.html` — the itinerary. Single file, all CSS/JS inline, no external assets.
- `404.html` — light-touch fallback for typo'd URLs.
- `.github/workflows/pages.yml` — auto-deploys on push to `main`.
- `.gitignore` — typical OS/editor noise.

## Notes

- The page is a single file by design — easy to edit on a phone via the GitHub web editor while you're on the road.
- All styling is light-mode and self-contained; nothing breaks if the device is offline once loaded.
