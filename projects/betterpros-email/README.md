# BetterPros — HTML email

Responsive HTML email announcing BetterPros' LatAm AEC staffing service.
Built from a design mock: header, hero, stats grid, 3-step "how it works",
two testimonials, CTA, footer.

## Open

```bash
open index.html
```

## Build notes

- **Table-based, 600px** fixed shell, inlined styles. Renders in Outlook
  (Word engine), Gmail, Apple Mail, Outlook.com.
- **Outlook specifics:** MSO conditional wrapper table for the 600px shell,
  `<v:roundrect>` VML fallback for the pill CTA (Word ignores
  `border-radius`), `AllowPNG` + 96 PPI in `OfficeDocumentSettings`.
- **Responsive:** one `@media (max-width:620px)` block. The 2x2 stats grid
  stacks to one column via `.col { display:block; width:100% }` with the
  `.gut` spacer cells collapsing. Gmail app on Android ignores media
  queries — it degrades to a scaled-down 600px layout, which is fine here.
- **Fonts:** Inter with a system-sans fallback stack; mono labels use the
  `SFMono-Regular, Consolas, Menlo` stack. No webfont `@import` — Outlook
  and Gmail strip it, and the fallbacks are close enough.
- **Preheader** text is set at the top of `<body>` (hidden div + zero-width
  padding chars so the inbox preview doesn't leak body copy).
- Both gutters and vertical spacers are real `<td>`/`<div>` elements rather
  than margins, since Outlook drops margins on block elements.

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
