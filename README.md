# general-claude

Personal sandbox repo. Scratch space for one-off builds, experiments, and
anything that needs a repo attached to a Claude Code session.

## Layout

```
projects/
  _template/           copy this to start something new
  betterpros-email/    HTML email template (BetterPros newsletter)
```

One folder per project under `projects/`. Each folder is self-contained —
no shared build, no root-level tooling, no cross-project imports. Delete a
folder and nothing else breaks.

## Starting something new

```bash
cp -r projects/_template projects/my-thing
```

Then fill in `projects/my-thing/README.md` with what it is and how to run it,
and add a line to the layout table above.

## Conventions

- Project folder names are lowercase and hyphenated.
- Every project gets a `README.md` saying what it is and how to open/run it.
- Static things (HTML, emails, pages) use `index.html` as the entry point so
  they can be opened straight from disk.
- Secrets and API keys never get committed. Use a local `.env` — it's gitignored.
