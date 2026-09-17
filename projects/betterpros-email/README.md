# BetterPros — HTML email

Responsive HTML email announcing BetterPros' LatAm AEC staffing service.
Built from a design mock: header, hero, stats grid, 3-step "how it works",
two testimonials, CTA, footer.

## Open

```bash
open index.html
```

## Build notes

Written to survive **Beefree's HTML importer**, which is stricter than an
inbox. That drives most of the structural choices here:

- **Horizontal padding is physical spacer cells, not CSS.** Every section is
  a 3-column row: `<td width="32">` gutter, `<td width="536">` content,
  `<td width="32">` gutter. Beefree does not reliably carry
  `padding-left`/`padding-right` from a content `<td>` through an import, so
  side padding declared that way disappears. A real cell with a `width`
  attribute cannot be dropped.
- **One `<table>` per section**, stacked, rather than one table with many
  `<tr>`s. That maps 1:1 onto Beefree's row model, so each section lands as
  its own editable row instead of collapsing into a single block.
- **Dividers are 1px `<td>` rows** with a `bgcolor` attribute, not
  `border-top` on a table. Borders on `<table>` elements get dropped on
  import; a filled cell survives.
- **No VML, no `<!--[if !mso]><!-- -->` reverse conditionals.** Importers
  strip comments, and that pattern's opening token is itself a comment — the
  block it wraps can get eaten along with it, which would have taken the CTA
  button with it. Beefree generates its own Outlook-safe markup on export,
  so the VML is redundant in this pipeline anyway.
- **Longhand padding** (`padding-top`/`padding-bottom`) and `bgcolor`
  attributes alongside `background-color`, since the importer reads
  attributes more consistently than shorthand.

The trade-off from dropping VML: if you send this file *directly* rather
than through Beefree, Outlook desktop renders the CTA as a green rectangle
instead of a pill. Everything else is unaffected. Export from Beefree and
that goes away.

Other notes:

- 600px fixed shell, inlined styles, `Inter` with a system-sans fallback.
  Mono labels use `Consolas, Menlo, Courier, monospace`.
- One `@media (max-width:620px)` block: the 2x2 stats grid stacks by
  flipping cells to `display:block` and collapsing the spacer cells, and
  gutters narrow to 22px. Beefree replaces this with its own responsive
  handling on import — it only matters if you send the file as-is.
- Hidden preheader at the top of `<body>` so the inbox preview doesn't leak
  the first line of body copy.

Verified rendering at 640px and 390px viewports. The Beefree import itself
is untested — I have no way to run it from here, so the structure follows
what their importer documents as supported rather than a confirmed round
trip.

## Placeholders to swap before sending

Everything under `assets/` is a generated stand-in, sized at 2x so it stays
sharp on retina. Swap the file, keep the filename, and nothing else changes.

| File | Size | Replace with |
|---|---|---|
| `assets/hero.png` | 1072x512 | Real photo (renders at 536px wide) |
| `assets/step-1..3.png` | 80x80 | Icons, green `#10B981` on `#E6F7F0` |
| `assets/linkedin/twitter/instagram.png` | 80x80 | Social glyphs in `#98A2B3` |

Also replace `{{unsubscribe_url}}` with your ESP's merge tag, and point the
`https://betterpros.com` links at real destinations with UTM params.

Before sending, re-host `assets/` on a CDN and switch the `src` attributes to
absolute HTTPS URLs — relative paths don't resolve in an inbox.

**This matters for the Beefree import too.** Beefree fetches images over the
network, so the relative `assets/...` paths come in broken. Either upload the
files to Beefree's image manager and repoint the blocks after importing, or
swap the `src` attributes for absolute URLs *before* you import.

`email.txt` is the plain-text alternative part. Send it alongside the HTML;
an HTML-only message is a deliverability penalty at most providers.

## Palette

| Token | Hex |
|---|---|
| Accent green | `#10B981` |
| Accent tint | `#E6F7F0` |
| Ink | `#101828` |
| Body text | `#5A6472` |
| Muted | `#8A94A3` |
| Border | `#E7EAEE` |
| Card bg | `#F6F7F9` |
| Page bg | `#EFF1F3` |
