# OSRIC 3.0 Character Manager for Owlbear Rodeo

**Reference document:** OSRIC 3.0 Player Guide (`OSRIC_3_0_Player_Guide_FINAL_v_7.pdf`).
All rules content, tables, and section numbers cited below trace back to
this document; nothing here is drawn from any other edition of OSRIC or
from general AD&D 1e knowledge.

Ability-score-driven calculations only, per your v1 scope: enter the six
ability scores, get every derived stat the OSRIC 3.0 Player Guide's ability
tables (1.1.2–1.1.7) define — no visual replica of the printed sheet, an
original compact layout instead.

## What's covered

- **STR** — To Hit, Damage, Encumbrance, Minor Test, Major Test, including
  the fractional exceptional-strength rows (18.01–18.99), gated behind the
  "Fighter-type" checkbox since only fighters/paladins/rangers roll the d100.
- **DEX** — Surprise, Missile To Hit, Initiative Effect, AC Adjustment (shown
  as descending / ascending, matching the sheet's own bracket notation),
  Agility Save.
- **CON** — HP Modifier (also gated by Fighter-type, since 17–19 differ),
  Resurrection Success, System Shock.
- **INT** — Max. Additional Languages.
- **WIS** — Mental Save Modifier.
- **CHA** — Max. Henchmen, Loyalty, Reaction.

**Also covered, added in a later pass** (not ability-driven, but now in the
same tool): full Saving Throw tables for all ten classes (Aimed Magic Items,
Breath Weapons, Death/Paralysis/Poison, Petrification/Polymorph, Spells),
and Level Advancement tables (XP thresholds, hit dice, named level benefits,
and spell-slot progressions for casting classes) — both driven by the
Class/Level fields below, sourced from Player Guide 1.3.x.4A/4B.

## Class restrictions by ancestry

Each ancestry's 1.2.x.3 section lists which classes it can take (as itself
or as a component of an allowed multiclass combo — this sheet doesn't model
multiclassing yet, just which single classes are on the table). Changing
Ancestry now filters the Class dropdown: disallowed classes show greyed out
and labeled "(not available to X)" rather than just being flagged after the
fact, and if the currently-selected class becomes disallowed, it switches
automatically to the first class that ancestry permits.

Halfling is genuinely this restrictive in the book — Fighter, Druid, or
Thief only, no Cleric or Assassin — that's not a transcription error.

## Ancestry

Adds Human plus the six demi-human ancestries (Dwarf, Elf, Gnome, Half-Elf,
Half-Orc, Halfling), each with its Table 1.2.0A / 1.2.x.1 ability adjustment
and required range. The interdependency this resolves: the number in each
ability's input box is your **base** (rolled) score; the arrow badge next to
it shows the **effective** score (base + ancestry adjustment), and every
derived stat and combat calculation uses the effective score, not the base
one — matching the book's own order of operations (roll, then apply
ancestral bonus, then check the range). An effective score outside that
ancestry's allowed range turns red with a ⚠, rather than being silently
clamped or blocked — you're still free to enter it, same as the book leaves
the GM to arbitrate.

**Not implemented** (deliberately, scope-limited to ability scores):
level limits by class, ancestral special abilities (infravision, Stalwart
saves, Stone-Kenning, etc.), languages, multiclass rules, or the age-based
ability adjustments — those all live elsewhere in Chapter Two and are a
different slice of work from "ancestry's effect on ability scores."

One rough edge worth knowing: a fractional exceptional-Strength roll
(18.01–18.99) plus a flat ancestry adjustment (Half-Orc +1, Halfling -1)
can land on a value the table doesn't cleanly define (e.g. 18.50 + 1 =
19.50). The lookup still resolves to the nearest defined row rather than
erroring, but that combination isn't something the book itself walks
through, so treat it as an edge case, not confirmed-correct behavior.

## Class, level, and Combat Rolls

A Class field (all ten OSRIC 3.0 classes) and a Level field now drive:

- **Fighter-type gating** — Fighter/Paladin/Ranger get STR's exceptional
  percentile rows and CON's higher HP bonus at 17–19; every other class is
  capped at plain 18 STR and the lower CON bonus, per the book's notes.
- **Combat Rolls** — a live matrix, target AC 10 upward (extendable), showing
  the natural roll needed to hit under the Ascending AC formula (1.4.2.4A):
  `d20 + BTHB + ability To-Hit ≥ AC`. Melee uses STR's To Hit; Missile uses
  DEX's Missile To Hit. BTHB comes straight from the Player Guide's Base to
  Hit Bonus table (levels 1–20, all ten classes).

Two things worth knowing about that table:
- Several classes (Assassin, Druid, Monk) show "N/A" past a certain level in
  the book. I've treated that as "stops advancing, holds at the last defined
  value" rather than some other rule — that's my assumption, not something
  the excerpt confirmed, so worth double-checking against the full combat
  chapter if it matters at high levels.
- "nat 20 only" in the matrix means the straight math needs more than 20,
  but the book's own special rule (a natural 20 always gets +5 added) still
  clears that AC. Genuinely impossible hits (even with the +5) show as "—".

## Layout

Reworked for density after seeing another Owlbear extension's compact style
(collapsible sections, tight rows): Ability Scores and Combat Rolls are now
`<details>` accordions (Ability Scores open by default, Combat collapsed),
each ability is a single dense row (abbreviation, score input, small chip
badges for every derived value — hover a chip for its full label), and the
whole thing targets ~420px wide to sit comfortably in an Owlbear side panel.

## Testing it right now

Just open `index.html` in a browser — no server, no Owlbear needed. The
calculations all run standalone; you'll see a "Standalone mode" notice at
the top instead of a token link.

## Testing inside Owlbear Rodeo

Hosted on GitHub Pages at `mogadon-RPG/osric-3-char-sheet` — add it via
its manifest URL:
`https://mogadon-rpg.github.io/osric-3-char-sheet/manifest.json`.
Once added, opening it with exactly one token selected links the sheet to
that token: it reads and writes its ability scores to the token's metadata
under the key `com.mogadon.osric-ability-reckoner/state` (kept as-is
despite the rename, so any already-bound tokens don't lose their saved
data), so scores persist with the token across sessions.

## Known rough edges (pilot, not final)

- No input validation beyond clamping to 3–19 — no UI polish on bad input.
- The Fighter-type flag is a single checkbox, not tied to an actual class
  field (there isn't one yet — this is ability scores only, remember).
- Styling is a first pass, not run through a full design review.
