# Validation status

Status snapshot: 2026-10-22. Private raw logs and profiles are not published.

Observed: native two-process connection on one PC, short guest squadmate movement, guest shoulder-camera correction, actual intro Matinee camera motion and reciprocal-registration cleanup, local player-context registration/cleanup, synthetic scene release/reacquire, and ordinary disconnect restoring AI. Earlier weapon input caused authoritative ammo consumption.

Not established: real full conversation control return, reliable equipment/firing/stop-firing, full combat, sustained mission travel, save/load, cross-PC Radmin, more than one guest, or end-user Mod Manager installation/disable/rollback.

One earlier run kept consuming ammunition after release; another had differing client/server counts. The latest extended equipment audit generated no eligible firing pulses due to inventory/action readiness. These are unresolved observations, not completed combat QA.

Real dialogue requires actual engine conversation state while a guest lease exists, followed by measured detachment and successful control/camera return. A synthetic gate and moving intro cameras do not satisfy this requirement.

Original-file protection in the audited environment covered 8,194 game files plus 16 profile files (8,210 total). Recent completed audit runs recorded zero native exceptions and unchanged original profiles. These observations apply only to those runs, not all possible future sessions.

Synthetic evaluators and their fixture tests are shipped so verification logic is reviewable. Private game-run evidence is required separately; a passed unit test or build does not mean the campaign works.

## Offline package evidence

`tools/melepkg/` reads the shipped `.pcc` packages without launching the game. It supports a
different class of claim from the rest of this document, and the boundary matters:

- **What it establishes**: what the shipped data declares and what a class default object is set to.
  Those are file facts with a closure check behind them.
- **What it does not establish**: anything about a running world. The reader is correct on
  `Default__X` exports (40/40 objects close exactly in `ME3/SFXGame.pcc`) and is **not** usable on an
  actor spawned in a level (9/40 in `ME3/BioA_Cat002.pcc`, 0/40 in `ME2/BioA_BchLmL.pcc`), because a
  spawned actor's serial block is not a property tag stream. A live read is still required for
  anything about a spawned object.

The measurements and their consequences are in [CAMPAIGN-INVARIANTS.md](CAMPAIGN-INVARIANTS.md). The
two that bear directly on claims made elsewhere in this document:

1. **No build enables seamless travel.** `bUseSeamlessTravel` is declared on `Engine.GameInfo` and set
   by 0 of 6,685 class default objects that close exactly. So `GetSeamlessTravelActorList` is never
   called, and the "sustained mission travel" gap above is not a matter of making that hook work - a
   level change destroys the guest's possessed pawn and it must be re-created.
2. **LE3 clamps capacity to one** on its campaign game class (`MaxPlayersAllowed=1`) while the base
   class allows 32, which names the field pair the `MaxPlayers`/`MaxAllowed` lease raises.

Offline checks run as `python -I -m unittest discover -s tests -p "test_mele_invariants.py" -v`.
They skip when no install is present, and the underlying commands exit 2 in that case rather than
printing zeroes, because a report full of zeroes from a tool that could not read the install is not
evidence of absence.

## Public source snapshot checks

The prepared public source copy built all three native outputs with Zig 0.15.2. Python unittest discovery ran 152 tests: OK, 2 skipped (platform/privilege-dependent fixtures). The separate Windows native harness passed memory/opt-in guards and temporary low-integrity write-denial fixtures. No game was launched for these publication checks.
