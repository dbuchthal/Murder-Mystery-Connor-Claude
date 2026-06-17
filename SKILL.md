---
name: murder-mystery-generator
description: Generate a complete, internally consistent murder mystery party game from a short brief. Use this whenever the user wants to create a murder mystery party, dinner-party whodunit, or roleplay mystery game — including when they mention players/guests/characters, a number of rounds, a designated killer and victim, a tone or voice, or want printable character backgrounds, per-round handouts, or a narrator/host script. Use it even when the user doesn't say "murder mystery" but describes wanting guests to play characters and solve a staged crime. Always use this skill for these tasks because the hard part is keeping clues, alibis, secrets, and the timeline mutually consistent across many separate files, and this skill enforces that with a build order and a validation pass.
---

# Murder Mystery Generator

Generate a full murder mystery party kit — a host-only story bible, one background per character, a handout for each character for each round, and a narrator script per round — all from a single brief.

## The one idea that makes this work

Everything must line up. The killer's alibi, the victim's last movements, the clues dropped in round 2, the red herring pointing at an innocent guest, what each character secretly knows and hides — it all has to be mutually consistent, and the mystery must be *fair* (solvable from the clues that actually surface, not arbitrary).

Generating each file independently guarantees contradictions: one sheet puts Alice in the library, a later narrator reveal puts her in the garden. So **never write a player-facing file before the ground truth exists.** Work in two phases:

1. **Build a host-only story bible first** — the single source of truth: the real timeline, method, who was where, the full clue list, what each character knows and hides, and which clues surface in which round.
2. **Derive every player-facing file as a *view* onto the bible.** Character backgrounds, per-round handouts, and narrator scripts only ever restate or reveal facts that already live in the bible. They never invent new ground truth.

If you catch yourself inventing a fact while writing a player file, stop and add it to the bible (re-checking consistency) before continuing.

## Inputs (the brief)

The user supplies a brief. `assets/brief-template.md` is the fill-in form. Required inputs:

- **Players** — each player's name and a short description (their character can be built from this).
- **Number of rounds.**
- **The killer** — which character did it.
- **The victim** — who is killed.
- **Tone & voice** — e.g. 1920s noir, gothic manor, campy comedy, hard-boiled, cozy. Applied uniformly to all text.

If any required input is missing, ask before generating. Also resolve one thing that's easy to miss:

- **Victim mode.** Does the victim play before dying, or are they dead at game start? Default for multi-round games where the victim is a named player: the victim plays round 1, is killed between rounds 1 and 2, then their player becomes a ghost/observer (give them a short "you are now dead" handout and let them help the host watch for lies). If the brief says otherwise, follow it. For a single-round game, the victim is dead at the start.

## Workflow

### Step 0 — Read the brief
Confirm the required inputs are present and resolve victim mode. Pick a short `slug` for the folder (e.g. `mystery-blackwood-manor`).

### Step 1 — Build the bible (no player files yet)
Follow `references/story-bible.md` for the full spec. The bible pins down setting, full cast (public persona + private secret + relationships), the crime (victim, method, weapon, place, time, motive), the true timeline, a means/motive/opportunity matrix for every suspect, the solution clue-chain, and a clue-distribution table mapping round × character → what surfaces.

### Step 2 — Validate the bible (the fairness gate)
Run the checklist below. Fix every gap before writing a single player file. This step is what prevents broken mysteries.

### Step 3 — Derive the player-facing files
Follow `references/output-files.md` for the exact templates. Produce, strictly as views on the bible: one character background per player, one handout per character per round, and one narrator script per round. Apply the requested tone consistently throughout.

### Step 4 — Write the host guide and assemble
Write `host-guide.md` (setup, printing, seating, timing, how to run the vote and reveal). Lay out the folder as below, then give the user a short summary and the file list.

## Design principles (what makes the mystery good)

Read `references/design-principles.md` for the full craft notes. The essentials:

- **Everyone hides something.** Give each non-killer suspect a secret that is *not* the murder (an affair, a debt, theft, a ruined reputation) plus apparent means, motive, and opportunity. This spreads suspicion and gives every player something to act, so the killer isn't the only one behaving oddly.
- **Make it solvable via a clue chain.** Define 3–5 key clues that *together* uniquely implicate the killer. Each must be planted somewhere (in a character's knowledge or a narrator reveal) and must surface by the final round. Don't let the solution hinge on one clue a single player could forget to mention — build in redundancy.
- **Decentralize information.** No one knows the whole picture. Distribute clues across characters and rounds so players must talk to each other.
- **Resolve every red herring.** Each false lead needs an innocent explanation that comes out by the end, or players feel cheated.
- **Let the killer lie consistently.** The killer's handouts must tell them what's been revealed and what to deny each round.
- **Escalate by round.** Round 1: meet the characters, the murder lands. Middle rounds: secrets and new evidence surface, suspicion shifts. Final round: accusation/vote, then the reveal.
- **Prevent metagaming.** Each character background ends with what that character does *not* know. Keep killer-only ground truth out of every sheet except the killer's own.

## Validation checklist (Step 2 gate)

Do not proceed to player files until all pass:

- [ ] Killer and victim match the brief exactly.
- [ ] Every suspect has apparent means, motive, and opportunity (so each is plausibly guilty at some point).
- [ ] 3–5 key clues form a chain that *uniquely* points to the killer; each is planted and surfaces by the final round.
- [ ] Each round surfaces at least one new clue and at least one secret or red herring.
- [ ] The timeline is physically consistent — no character is in two places at once, and the killer's movements actually allow the murder at the stated time/place.
- [ ] Every red herring has an innocent resolution that comes out during play.
- [ ] No player sheet contains killer-only ground truth (the killer's own sheet excepted).
- [ ] Each character background states what that character does not know.
- [ ] The requested tone is applied consistently across all files.

## Output layout

```
mystery-<slug>/
  brief.md            # the inputs used (copy of the filled brief)
  bible.md            # HOST ONLY — the full ground truth
  host-guide.md       # how to run it: setup, printing, timing, vote, reveal
  characters/
    <name>.md         # 1-page background, handed to that player
    ...
  rounds/
    round-1/
      narrator.md     # longer host script read at the top of the round
      <name>.md       # what that character reads before round 1
      ...
    round-2/
      ...
```

Expected file count: 1 bible + 1 host guide + 1 brief + N character backgrounds + rounds × (1 narrator + per-character handouts). (A dead victim gets a short ghost handout instead of a full one in later rounds.)

After assembling, present `bible.md` and `host-guide.md` first (the host reads those), then point the user to the per-character and per-round files. Remind them the bible and host guide are host-only.
