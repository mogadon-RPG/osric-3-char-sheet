# Backlog

Deliberately deferred work — not forgotten, not blocked, just not started.
Nothing here is a bug or a gap in what's already built; see the README's
"Not modelled" section for that.

## Open

### Printable character form + Mustache template merge
A button that fills out a template with the current character's data,
producing a print-ready sheet. Two previously separate ideas (Mark's
"print form" request and a Mustache-style template-merge feature) were
folded into one, since the generated template *is* what makes the form
ready for print — there's no separate print-specific code needed on top
of it.

**Status:** unblocked, waiting on Mark to say go. The export schema this
depends on (a nested, versioned JSON contract covering every character
field) shipped in v0.15.0 and has since been extended to v1.1.0 (owner
snapshot). Nothing else is known to stand in the way.

**Open question, noted for whoever builds it:** whether Owlbear's
extension sandbox allows a print dialog or a file download from inside
its iframe. This was never confirmed either way — it would need to be
tried in a real room. See the `downloads` runtime capability if one
turns out to be needed.

## Decided against (kept here so it isn't re-proposed)
- Enforcing the Druid's "wooden shields only" rule — shields are chosen
  by size, not material, so this can't be modelled with the current
  Armour/Shield picklists. Mark said skip it.
- A GM roster view and further JSON export/import work were held out of
  scope for any blanket "build the backlog" instruction, until named
  specifically. The roster view was later named and built (v0.20.0).
  Further export/import work — including the original GM
  pre-build-NPC-tokens use case — has not been.
