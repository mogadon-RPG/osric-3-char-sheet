# OSRIC 3.0 Character Sheet

An [Owlbear Rodeo](https://www.owlbear.rodeo/) extension that works out a
character's numbers from the OSRIC 3.0 rules — and also runs on its own in
any browser. It is an original, compact layout, not a replica of the printed
character sheet.

Status: pilot (0.x). Built by Anthropic Claude.

## Assumptions and decisions — read this first

**1. This is canonical OSRIC 3.0, and nothing else.**
The reference document is the OSRIC 3.0 Player Guide
(`OSRIC_3_0_Player_Guide_FINAL_v_7.pdf`). Every table and rule in the sheet
traces back to it, with section numbers cited in the sheet's own "Sources"
rollup. Nothing is drawn from other editions of OSRIC, from general AD&D 1e
knowledge, or from any house rules or custom settings. Where the book is
silent, the sheet says so rather than inventing a rule.

**2. Ascending AC only.**
OSRIC 3.0 supports both ascending and descending Armour Class. This sheet
deliberately supports **ascending only**: the AC readout and the Combat Rolls
matrix (`d20 + BTHB + ability To-Hit ≥ AC`, Player Guide 1.4.2.4A) are ascending.
Two details, so this isn't overstated: armour is stored internally in the
book's descending values (ascending = 20 − descending), and the DEX row's
"AC±" chip still shows the book's paired descending/ascending adjustment.
Nothing else in the UI shows descending AC.

**3. Options that are greyed out are OSRIC's own limits — not this tool's choices.**
Some choices are unavailable because the rules themselves forbid the
combination:

- **Ancestry → Class.** Each ancestry's "Character Classes" section
  (1.2.x.3) lists what it can be. A Halfling can only be a Fighter, Druid or
  Thief, for example — that is the book, not a transcription error.
- **Class → Armour and Shield.** Each class's own "Armour/Shield Allowed"
  line (1.3.x.1): Magic-Users, Illusionists and Monks may use none; Thieves
  are limited to padded, leather or studded leather and no shield; Assassins
  to leather or studded leather; Druids to leather. Fighters, Clerics,
  Paladins and Rangers may use any.

Unavailable options stay visible but italic and light, with no extra text.
If the current selection becomes illegal (change of ancestry or class, a
token loaded, a JSON import), it switches automatically to the first legal
option — so an import doesn't leave an illegal class/gear combination behind.

Ability scores are handled differently: a score outside its ancestry's
required range is flagged red with a warning, not blocked, because the book
leaves that to the GM.

**4. Level comes from XP, per class.** Level is derived from the class's own
XP table, not typed in. Assassin (15), Druid (14) and Monk (17) have hard
level caps — those higher levels don't exist for them. XP beyond the cap is
still kept, but shown in red because it no longer does anything.

**5. Current HP is a tracker, not a formula.** It never follows Max HP on its
own. A class change forces it to 0 (hit dice must be rerolled), and a button
in the Hit Points section sets it to Max HP.

## What it covers

- **Major values:** XP, Level (read-only), GP, Armour, Shield, AC (read-only).
- **Ancestry and Class** (gated as above); **Ability Scores** with base →
  effective (ancestry-adjusted) scores and every derived value from Player
  Guide tables 1.1.2–1.1.7.
- **Hit Points:** per-level hit-die rolls, CON bonus, fixed post-cap bonus,
  Current HP.
- **Combat Rolls:** the roll needed to hit each AC, melee and missile.
- **Saving Throws** and **Level Advancement** for all ten classes.
- **Class Specific** (shown only where they apply): Turning the Undead,
  Thief Skills, Spell Slots.
- **Notes** (level grants and cap notes gathered in one place) and a
  **Sources** rollup.

## Not modelled

- Multiclassing (an ancestry's multiclass combos aren't selectable — only
  which single classes are allowed).
- Level limits by ancestry, ancestral special abilities, languages, and
  age-based ability adjustments.
- Shield *material*: the Druid's "wooden shields only" rule can't be
  enforced because shields are chosen by size. Size sets only how many
  attacks per round a shield's flat +1 can cover; the per-round decision
  itself isn't tracked.
- A fractional exceptional-STR roll plus a flat ancestry adjustment
  (e.g. 18.50 + 1) can land on a value the table doesn't define. It resolves
  to the nearest row, but the book doesn't cover that case.
- Not built yet: a printable character form generated from a template.

## Using it

**Standalone:** open `index.html` in a browser. All calculations work; nothing
is saved except through Export/Import.

**In Owlbear Rodeo:** add it as a custom extension using
`https://mogadon-rpg.github.io/osric-3-char-sheet/manifest.json`. If Owlbear
shows an old version after an update, add a throwaway query string
(`...manifest.json?nocache=1`) to get past the cached manifest.

**Linking a sheet to a token.**
- *Automatic:* select exactly one token.
- *Manual:* click the avatar in the top-right. A GM sees every Character-layer
  token in the scene; a player sees the tokens they created. (A token a GM
  later reassigned to a player is not detected — Owlbear's "current owner"
  field isn't used yet.) "Use map selection instead" returns to automatic.
- *Upload and bind:* in the same picker, upload an exported character JSON,
  then choose the token to write it to.
- The data lives in the token's metadata under
  `com.mogadon.osric-3-char-sheet/state`. The text box under the avatar edits
  the token's own text label (Owlbear's "Edit text").
  Binding to a token with no saved data starts a blank default character, and
  nothing is written to a token until something on the sheet is edited.
  A small indicator beside the status line says whether the token holds your
  data: *on token ✓* (it already had data), *empty token* (nothing saved yet),
  *saved ✓* (the last change was written) or *NOT saved ⚠* (Owlbear refused the
  write — hover for the reason; in a room using Owner Only, only the token's
  owner or the GM can change it). Failures reading or listing tokens are shown
  in the panel too, not just in the console.

**Export / Import.** The exported JSON is a nested, versioned contract whose
key names are deliberately independent of the code's internal names:

```json
{
  "schemaVersion": "1.0.0",
  "character": { "ancestry": "Dwarf", "charClass": "Fighter", "level": 4, "xp": 8000 },
  "abilities": { "str": 16, "dex": 12, "con": 15, "int": 9, "wis": 10, "cha": 8 },
  "combat": { "armor": "Chain mail", "shield": "Small (1 foe)" },
  "hitPoints": { "current": 22, "rolls": [8, 6, 5, 3] },
  "wealth": { "gp": 75 }
}
```

Import checks types, ranges and the known ancestry, class, armour and shield
names, and rejects anything malformed; a file from a different schema version
is imported with a note.

**Token-data inspector (debug).** Right-click a token → *Show all data on
this token (debug)*. It shows who you're viewing as, whether you created the
token, this extension's saved data, every extension's metadata, and the
item's other properties. It is switched by one constant,
`ENABLE_TOKEN_DEBUG_MENU`, at the top of `background.html`: set it to `false`
and nothing is registered (the SDK isn't even loaded) while all the code stays
in place. After changing the manifest, re-add the extension so Owlbear picks
up the background page. The background page logs `[osric-3-char-sheet] ...`
lines to the browser console saying whether the menu item was registered, or why not.

## For whoever edits this

- Files: `index.html` (the whole app), `background.html` and `debug.html`
  (inspector), `manifest.json`, `icon.svg`.
- **The version lives in three places** and must stay in sync:
  `manifest.json` `version`; the `?v=` on all four manifest URLs (icon,
  background, action icon, popover); and `APP_VERSION` in `index.html`.
  The `?v=` values are what defeat Owlbear's caching.
- Bump versions with exact-string replacement, never a loose pattern: an
  unescaped-dot `sed` once rewrote part of an ability table and silently
  broke the whole script.
- **Never hand the SDK your own live objects.** `updateItems` runs through
  Immer, which freezes whatever you assign into an update. A shallow copy of
  `state` froze the live HD-rolls array and broke entering rolls after the
  first save. Deep-copy on the way out (`cloneJSON`) and don't mutate arrays
  in place.
- **Register SDK listeners before awaiting anything, and never let a failure
  be silent.** The selection listener used to be registered only after an
  awaited first refresh, so one failed refresh permanently stopped the sheet
  following the map (the status bar stuck on "Checking for Owlbear Rodeo…").
  Errors now surface in the panel.
- A syntax check alone is not enough — run the page. A `const` read before
  its declaration parses fine and only fails when the page actually executes.

## Legal

OSRIC is a trademark of Matthew Finch and Stuart Marshall, used with
permission. This work includes AELF Open Gaming Content, used under the AELF
Open License version 1.0a, and is not endorsed by Mythmere Games LLC or any
other contributor. This tool's own code is an unofficial fan work by Mogadon.
The full notice is in the sheet's footer.
