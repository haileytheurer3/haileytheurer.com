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
| `index.html` | The homepage: the photo portfolio. Self-contained - all markup, CSS and JS in one file. |
| `data/photos.json` | The photo list: id, dimensions, group, title, note, date added, pick flag. This is what the homepage reads to build the galleries. |
| `photos/` | The images, one `<id>.jpg` per entry in `photos.json`, resized to 1600px on the long edge. |
| `robots.txt` | Currently blocks all search engines. See "Search visibility" below. |
| `404.html` | Shown for any URL that does not exist. |
| `CNAME` | Tells GitHub Pages the site lives at `haileytheurer.com`. **Do not delete or rename this** - the custom domain stops working. |
| `.nojekyll` | Stops Pages from running Jekyll. Without it, any file or folder whose name starts with `_` is silently dropped. **Leave it alone.** |

## Adding or changing photos

Two files move together, and they must stay in sync:

1. Drop the image into `photos/` named `<id>.jpg`, where `<id>` is any short unique string
2. Add a matching entry to the `photos` array in `data/photos.json`

The fields are `id`, `w`, `h` (pixel dimensions), `title`, `note`, `added` (`YYYY-MM-DD`),
`group` (`people`, `wildlife`, or `place`), and optionally `pick: true` with a `pickAt`
number to feature it in the Selected Work row at the top.

A photo listed in `photos.json` with no matching file shows as a broken image. A file in
`photos/` with no entry in `photos.json` simply never appears.

Keep images at roughly 1600px on the long edge. Full-size originals are archived outside
this repo, in `~/obsession/projects/photography-portfolio/photos/`.

## Search visibility

The site is currently **hidden from Google**, by two things that have to agree:

- `robots.txt` contains `Disallow: /`
- `index.html` has `<meta name="robots" content="noindex, nofollow">`

This came from when the portfolio was an unlisted link. To become findable by name,
both have to change: `robots.txt` to `Allow: /`, and the meta tag to `index, follow`
(or just deleted). Changing only one of them does nothing.

## Adding other pages

Create an HTML file and link to it. `about.html` is served at
`haileytheurer.com/about.html`. For a cleaner URL, make a folder with an
`index.html` inside it: `work/index.html` is served at `haileytheurer.com/work/`.

Use root-relative paths in links and assets (`/styles.css`, not `styles.css`) so
they resolve the same from any depth.

## History

The portfolio began as a Claude artifact and was moved onto this domain on 2026-08-30.
It previously lived at `haileytheurer3/photographs`, which is now retired. The local
working copy is `~/obsession/projects/haileytheurer-com/`.
