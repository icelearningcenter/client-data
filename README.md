# client-data

The simulated client records behind the ICE video library's **Client Record**
pages — one medical-chart fragment per teaching case — served from GitHub Pages
at **https://icelearningcenter.github.io/client-data/** and loaded by the
`/client-viewer` page on the ICE site.

These are the case subjects the library's videos follow (chart, diagnosis,
insurance, referring physician, age), not records of real patients.

## One file is generated. The rest are yours

| File | Generated? | Notes |
|---|---|---|
| `client-list.js` | **yes** | Rebuilt daily at 07:00 UTC and pushed over. **Any hand-edit is lost.** |
| `client-note-*.html` | no | Hand-maintained. Edit these directly. |

`client-list.js` is written by `generate_client_content.py` in
[`brightcove-tools`](https://github.com/icelearningcenter/brightcove-tools/blob/main/.github/workflows/update-content.yml)
from the client Google Sheet. It is the index `/client-viewer` reads to turn a
`?client=<slug>` parameter into a name and a note file:

```js
const clientData = [
  { name: "001 Ben", slug: "client-note-ben" },
  ...
];
```

**To add, rename, reorder or retire a client**, edit the Google Sheet — the
record ID, display name, slug, diagnosis, setting and `Status` all come from
there. A client is listed only while its `Status` reads `Active`. Editing
`client-list.js` here instead looks like it worked and is reverted within a day.

**To change what a client's chart says**, edit that `client-note-*.html`
directly. Nothing regenerates the notes.

The two must agree: a Sheet row whose slug has no matching
`client-note-<slug>.html` produces a client-viewer link that resolves to
nothing, and the daily run will not warn about it.

## Naming

Note files are `client-note-<slug>.html`, where the slug matches the Sheet's
`client_url` column. A client with more than one case carries a distinguishing
suffix rather than a number — `client-note-alice-cva.html` and
`client-note-alice-rotator-cuff.html` are two different cases, as are
`client-note-tom-inpatient.html` and `client-note-tom-outpatient.html`. The
numeric Client Record ID lives in the Sheet and in the chart, not in the
filename.

## Related

- [`header-data`](https://github.com/icelearningcenter/header-data) — the
  `*-clients.html` tables that link into these pages, generated from the same
  Sheet.
- [`brightcove-tools`](https://github.com/icelearningcenter/brightcove-tools) —
  the generator and its schedule.
