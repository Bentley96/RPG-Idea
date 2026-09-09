# Replay Tax & Loop Acceleration

**The problem that kills regression games.** If loop 5 makes the player replay
two hours they have already seen, they stop playing. Every mechanic here exists
to attack that.

Per **D-004**: all three layers are adopted. Numeric tuning is deferred until
the core loop is proven — the targets below are placeholders to design against,
not committed values.

---

## The three layers

### Layer 1 — Acceleration (compress known content)

Known content must get faster to pass through. Mechanisms:

| Mechanism | Effect |
|---|---|
| **Dialogue compression** | Heard dialogue becomes skippable. Better: the protagonist *interrupts* — "I know. Save it." Turns a skip button into characterisation |
| **Cultivation acceleration** | Realms previously reached are re-climbed at a multiplier. The body remembers the path even though the core is gone |
| **Route knowledge** | Known locations open direct paths; no re-solving a solved traversal |
| **Trivialised encounters** | Fights the player has comfortably won become fast, not because of stats, but because their technique tier has outgrown the encounter |
| **Collapsed social arcs** | A ten-beat trust arc becomes two beats when the player knows what the NPC needs (see `docs/02-loop/persistence-matrix.md`) |
| **Solved puzzles** | Known answers are simply entered. Never re-gate on a puzzle the player has already solved |

> **Rule:** never make the player re-perform a *solved* problem. Re-fighting a
> fight can be satisfying if it now feels easy. Re-solving a puzzle is never
> satisfying.

### Layer 2 — Anchor advance (skip known content entirely)

Acceleration alone is not enough — compressing four acts by 70% is still a long
walk. The anchor advances at act boundaries, so old content leaves the loop
entirely rather than being replayed quickly.

Act 1's anchor is fixed and sacred. See `docs/02-loop/loop-architecture.md` §3,
including the open question of what moves the anchor in fiction.

### Layer 3 — Acknowledgment (make repetition mean something)

Repetition the game *notices* is thematic. Repetition it ignores is a bug.

- The protagonist's narration changes with the loop counter — the jadedness
  axis (`docs/01-narrative/character-axes.md`) is fed directly by this
- Re-treading an early scene late should read differently: the same words from
  the bullies, received by someone they cannot touch
- Occasional lines that only fire at high loop counts, rewarding the player who
  has genuinely lived it
- The mentor, met for the twentieth time, is a source of real feeling. Use it
  carefully and rarely

---

## Time budget

The measurable target. Tune once the loop is playable.

> **Target:** on loop N, reaching loop N−1's furthest point should take **≤40%**
> of the time it took the first time.

Corollary targets:
- Time from anchor to frontier should **never increase** loop over loop
- Time from anchor to frontier should trend **downward in absolute terms**
  across an act, even as the frontier moves further out
- The Recovery phase should not exceed roughly a third of a loop's runtime

Instrument these early. `TimeToFrontier` per loop is the single most important
telemetry value in the game — if it climbs, the design is failing regardless of
how good anything else is.

---

## Anti-patterns

Things that look like solutions and are not:

| Anti-pattern | Why it fails |
|---|---|
| **A "skip intro" button** | Admits the content is a chore. Fix the content, or advance the anchor |
| **Fully skippable known areas** | Throws away the acknowledgment layer, which is where the theme lives |
| **Persistent stat buffs to speed the climb** | Contradicts D-007. Power is technique and knowledge, not accumulating multipliers |
| **Randomising known content** | This is not a procedural roguelike (D-002). Randomising authored content destroys knowledge value — the player's foreknowledge stops being true |
| **Shorter content for later loops only** | Splits the content budget. Author once, compress dynamically |

---

## The tension to hold

Acceleration and acknowledgment pull against each other: the fastest possible
replay is one the player does not experience at all, and an unskippable
acknowledgment is a tax.

Resolution: **compress the mechanics, preserve the moments.** Fights get
shorter, traversal gets shorter, dialogue trees collapse — but the two or three
beats per act that carry the theme stay, and change with the loop count.

The player should never feel they are replaying content. They should feel they
are *moving through their own history*, faster each time.
