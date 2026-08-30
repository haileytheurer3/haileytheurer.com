# haileytheurer.com

The source for [haileytheurer.com](https://haileytheurer.com). Plain static HTML
and CSS - no build step, no framework, no dependencies. What is in this repo is
what gets served.

## Working on the site

```bash
git clone https://github.com/haileytheurer3/haileytheurer.com.git
cd haileytheurer.com
python3 -m http.server 5055
```

Then open <http://127.0.0.1:5055/>. Edit files, refresh the browser, repeat.
Stop the server with Ctrl-C.

To publish:

```bash
git add -A
git commit -m "describe what you changed"
git push
```

GitHub Pages rebuilds automatically. The change is live in one to two minutes.

## The caching trap

**A normal browser reload can show you a stale page for up to 10 minutes after a
deploy.** GitHub Pages serves everything with `Cache-Control: max-age=600`, so
both the CDN edge and your browser will happily hand back an old copy. This makes
a perfectly good deploy look like it silently failed.

If a change does not appear:

1. Hard-refresh: **Cmd-Shift-R** (Mac) or **Ctrl-Shift-R** (Windows)
2. Still nothing? Add a cache-buster: `https://haileytheurer.com/?cb=123`
3. Check the build actually ran: repo → **Actions** tab

Do not start editing files again until you have ruled out cache. It is almost
always cache.

## Layout

| Path | What it is |
|---|---|
| `index.html` | The homepage. Currently a placeholder. |
| `404.html` | Shown for any URL that does not exist. |
| `CNAME` | Tells GitHub Pages the site lives at `haileytheurer.com`. **Do not delete or rename this** - the custom domain stops working. |
| `.nojekyll` | Stops Pages from running Jekyll. Without it, any file or folder whose name starts with `_` is silently dropped. **Leave it alone.** |

## Adding pages

Create an HTML file and link to it. `about.html` is served at
`haileytheurer.com/about.html`. For a cleaner URL, make a folder with an
`index.html` inside it: `work/index.html` is served at `haileytheurer.com/work/`.

Use root-relative paths in links and assets (`/styles.css`, not `styles.css`) so
they resolve the same from any depth.

## Notes

- The site is public and so is this repo. Do not commit anything you would not
  want read: credentials, API keys, private notes, unreleased work. There is a
  `/private/` folder in `.gitignore` if you need somewhere local to stash things.
- Keep an unpublished draft ready without shipping it by leaving it at its final
  path and adding that path to `.gitignore`. Publish later by deleting the line.
