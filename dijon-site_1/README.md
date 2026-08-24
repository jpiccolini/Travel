# Dijon — la recherche

A single-page site about the apartment search in Dijon, 25 June – 2 July 2026. One HTML
file, one images folder, no build step.

```
index.html
images/thumb/   36 photos, ~760px  — the contact sheets
images/full/    36 photos, ~1600px — the viewer
```

Total about 14 MB.

---

## Editing

**The story slots.** Gold dashed boxes marked *À compléter*, holding prompting questions
in italics. Replace the text inside `<div class="slot">…</div>` with your own, then delete
`class="slot"` so the box styling disappears. Or delete the whole `<div>` if a section
doesn't need narrative.

**Photo captions.** Near the bottom of the file there are two arrays. `SHOTS` is the two
apartment visits; `CITY` is the days either side.

```js
{ id:'img_7547', t:'12:01:00', cap:'Attic bedroom under the slope…' },
{ id:'img_5728', d:'1 July', t:'11:20', cap:'The owl on rue de la Chouette…', people:true },
```

`cap` is both the visible caption and the alt text. `t` is the camera's own timestamp;
`d` is the date, used only in `CITY`. A `gap:'…'` key inserts a labelled divider above
that photo — that's what produces "29 minutes later" and "The next day — 30 June".
Reorder or delete entries freely; both grids rebuild from the arrays, and the viewer
pages across all 36 in order.

Six entries carry `people:true`, flagging an identifiable face. Delete those entries if
you'd rather not publish them.

**Map pins.** The `PLACES` array, just below. `k` picks the colour: `home` oxblood,
`tram` green, `city` ochre.

```js
{ n:'Les Halles', s:'Market, Tue/Fri/Sat mornings', ll:[47.3231, 5.0392], k:'city' },
```

I only included places whose coordinates I could verify, so Les Halles, Notre-Dame and
the Cité de la Gastronomie aren't on there yet — grab their coordinates from
OpenStreetMap and add them.

**Colours and type** are CSS custom properties in the `:root` block at the top.

---

## Publishing it so other people can see it

Opening `index.html` from your own disk only works for you — the images live on your
machine and `file:///Users/jim/...` means nothing to anyone else. To share it, the files
have to live on a server. Three options, cheapest effort first.

### Render (what you already use)

1. New GitHub repo. Add `index.html` and `README.md`. The web editor won't take binaries,
   so use **Add file → Upload files** and drag the `images/` folder in.
2. Render → **New → Static Site**, point it at the repo.
3. Build command: leave blank. Publish directory: `.`
4. Auto-deploys on push, same as the tennis app. You get a `*.onrender.com` URL to send.

### GitHub Pages

Same repo, no second service: **Settings → Pages → Deploy from a branch → main / root**.
Gives you `username.github.io/repo-name`. Fine for something this size.

### Netlify Drop

`netlify.com/drop` — drag the whole folder onto the page, get a URL in about ten seconds.
No repo, no account needed to start. Good for showing someone tonight.

### Before you publish

The site is currently `noindex`, and the images are already stripped of GPS. Decide about
the six photos with faces in them and the `noindex` tag, then push. If you want it seen by
family but not the world, GitHub Pages on a private repo needs a paid plan — the simpler
route is to leave `noindex` on and just send the link to people directly.

---

## Privacy and rights

- **EXIF is gone.** The originals carried GPS accurate to a few metres, plus timestamps
  and the phone model. The web copies have all of it stripped — I read the timestamps out
  first and hardcoded them, so the page still shows the real times without shipping the
  metadata. The HEIC files were converted to JPEG in the same pass.
- **The map pin is approximate**, placed to the block rather than the door.
- **`noindex` is on.** Line 8 of `index.html` keeps this out of search results. Delete
  that `<meta>` tag when you want it findable.
- **Six photos show faces.** Flagged in the arrays as described above.
- **One photo was left out on purpose.** `IMG_0029_2.WEBP`, the bathroom shot, carries a
  Century 21 watermark — it's the agency's listing photograph, not yours, and their
  copyright. Fine to keep for your own reference; risky to republish on a public site.
  Your own bathroom photos from 29 June are already in the gallery and cover the same
  room, so nothing is lost.
- **`IMG_7560_4.JPG` was also skipped** — it's the same terrace frame as `img_7560`,
  already in the set at 12:02:39.

---

## The two external dependencies

Google Fonts and Leaflet/OpenStreetMap both load from CDNs. If the school's HTTPS
inspection interferes:

- **The map already degrades.** If Leaflet doesn't load, the page swaps in a self-contained
  list of the same places, each linking out to OpenStreetMap. No blank grey box.
- **Fonts fall back** to Georgia and a system sans. The layout doesn't shift much.
- **To remove the dependency entirely**, download `leaflet.js`, `leaflet.css` and the
  woff2 files from a machine that can reach them, drop them in a `vendor/` folder, and
  point the `<link>` and `<script>` tags at the local copies.

---

## Notes on the build, if this ends up in front of a class

A few things here are worth stealing for Modern Web Design:

- **The metadata is the structure.** The photo section isn't ordered by taste, it's
  ordered by the camera's clock, and the 29-minute gap between the last interior shot and
  the exterior one became a design element. Reading real data out of real files and
  letting it drive the layout is a better lesson than a lorem-ipsum grid.
- **Cropping vs. cropping-out.** The contact sheet is square with `object-fit: cover`,
  which keeps the chronological left-to-right reading order that a masonry column layout
  would destroy. The full frame lives in the viewer. Grid order matters when the content
  is a sequence.
- **Graceful degradation you can actually see.** The map fallback is a good demo because
  you can trigger it by blocking one domain in devtools.
- **Stripping EXIF is a privacy lesson with a visible artifact.** Students can open a
  phone photo in an EXIF viewer, find their own house, and then fix it.

No frameworks, no build tooling, ~950 lines including the CSS. It's readable top to bottom.
