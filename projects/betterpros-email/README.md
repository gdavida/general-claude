# BetterPros — HTML email

Responsive HTML email announcing BetterPros' LatAm AEC staffing service.
Built from a design mock: header, hero, stats grid, 3-step "how it works",
two testimonials, footer.

Two CTAs, both pointing at the same destination: a left-aligned one directly
under the hero image so it lands above the fold, and the centered one on the
grey band at the bottom. Give the two links different UTM params so you can
tell which position actually converts.

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
- **The CTA keeps its VML fallback.** Outlook's Word engine ignores
  `border-radius`, so the pill is drawn with `<v:roundrect>` and the HTML
  button is hidden from Outlook by a `<!--[if !mso]><!-- -->` reverse
  conditional. That pattern is comment-delimited, so a comment-stripping
  importer can eat the button along with it — if you ever feed this file to
  Beefree, delete both branches and keep only the plain table button.
  Beefree regenerates its own Outlook-safe markup on export anyway.
- **Longhand padding** (`padding-top`/`padding-bottom`) and `bgcolor`
  attributes alongside `background-color`, since the importer reads
  attributes more consistently than shorthand.

Sending straight through HubSpot works too — its importer is far more
permissive than Beefree's, and it passes the HTML through close to as-is,
which is why the VML is worth keeping here.

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

Point the `https://betterpros.com` links at real destinations with UTM params
(different ones per CTA position).

## HubSpot tokens

The footer already carries the CAN-SPAM tokens HubSpot blocks publishing
without:

```
{{ site_settings.company_name }} · {{ site_settings.company_street_address_1 }},
{{ site_settings.company_city }}, {{ site_settings.company_state }}
{{ unsubscribe_link }}
```

Those `site_settings` values resolve from **Settings > Marketing > Email >
Configuration** in the HubSpot account, not from anything in this file — if
the rendered footer comes out blank, that's where to look.

The same tokens are in `email.txt`. They render as literal text anywhere
other than HubSpot, so swap them back to plain copy if you ever send this
through a different ESP.

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
