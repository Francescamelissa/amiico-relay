# The queue — one file, worked top-down (ruling 034 §2, 039 §2)

Priorities are not issued in prose. Each row carries who raised it, what
closes it stated as an **observable** (a file, a number, a read-back — never
an intention), and its state: **ready** · **in progress** · **held by CTO**
(waiting on a ruling or on text reaching the CTO) · **held by founder**
(waiting on an action only she can take) · **done** (with the observable that
closed it). The builder edits this file; the CTO amends by text and the file
decides (039 §2). Rows are worked from the top. A row that cannot be worked is
moved, not skipped silently.

| # | item | raised by | closes when (observable) | state |
| --- | --- | --- | --- | --- |
| 1 | The taint window: every reading, count and gate result recorded between 20:24:45 and the toolchain being set is enumerated and re-taken, with exit status asserted and a positive control beside each gh reading | CTO (040 §1, §3) | the enumeration is in the builder's reply with each row marked re-taken or superseded; `scripts/lib/gh-read.js` is on `main` | **done** — reply of 2026-09-15 21:05; PR #254 |
| 2 | Empty-corpus check: did any composite gate pass inside the window on an input set it never had | CTO (040 §2) | each gate that enumerates through git/gh is named with what it did on the night; every counting gate asserts a floor | **done** — reply of 21:05; `rulings-sequence`, `unseen-branches`, `tap-target-audit` reds shown; PR #254 |
| 3 | The three `git reset --hard` losses: recovered work verified against the pre-reset copy and the transcript; the probe procedure moved to a second tree | builder (found 20:52) | the five files' content matches copy + replayed edits (composite 18/0); the rule is in ENGINEERING parent 4 with the instance | **done** for recovery and rule (PR #254); **ready** for the scratch-tree helper (`scripts/lib/scratch-tree.sh`) |
| 4 | PR #254 review | builder | merged, or returned with sections | **held by CTO** |
| 5 | Erasure runbook `docs/erasure-rehearsal.md` (#252): point of no return marked; trips rows captured before (ids + owner ids saved durably outside the repo); trips prediction before N / after N−1 or N; "not exercised" for zero-before rows; entity_id row 41 + erase statement; audit-log delete step 6; invited_email erase; then its TEXT pasted to the CTO | CTO (029 §2–§4, 031 §B–§D) | the CTO has the runbook text in a message (they cannot read repo paths); #250/#251 unblock on it | **in progress** — next after this reply |
| 6 | Article 12(3) erasure of USER-B, due 2026-10-14 | CTO (031 §A) | the after-state query returns the predicted counts; residuals listed; the subject's identifiers never in rulings/, runbook, commit or fixture | **held by CTO** — on row 5's runbook being read |
| 7 | Ruling 029 §2: `activity_log.entity_id` can hold a user id — query row 41 and an erase statement (migration text, red lane) | CTO (029 §2) | the migration text is in a PR body for red-lane review; the runbook's row count is read from the file | **ready** |
| 8 | Ruling 029 §3: the FK map derived from the catalog (pgTAP), not regex over migrations | CTO (029 §3) | a pgTAP assertion listing the 21 FK columns from `pg_constraint` is green, and a red is shown by dropping one in a copy | **ready** |
| 9 | Ruling 029 §4 / 030 §D: CI seeded cascade fixture — one row in every place a user id can live, including an `activity_log` row per `entity_type` value | CTO | the before-state query on the fixture returns non-zero on every row that can be non-zero; "not exercised" rows are named | **ready** |
| 10 | Ruling 029 §5: residuals — audit-log delete (self-asserting count/delete/count/raise, privilege check), `trip_invites.invited_email` erase, summaries must not store names as prose (or the DPIA comment corrected) | CTO | each residual has either an erase step in the runbook or a sentence saying why it survives, and the DPIA comment matches the code | **ready** |
| 11 | Ruling 029 §6: trip survival — what writes trips to the server (`create-trip.tsx` insert), the delete path (`tripLifecycle`), the RLS delete policy on `trips` | CTO | three file:line citations in a reply, each read from the tree | **ready** |
| 12 | Ruling 029 §7: the code-entry contradiction — the app has NO code entry; the dead copy is in the template, not the app | builder (returning the contradiction) | the CTO rules which side changes | **held by CTO** |
| 13 | Ledger entries: hook not a control; `HUSKY=0` observed both arms; ruleset before 0 / after 1; the toolchain incident (shim set: git, python3, clang, make, swift); the three resets; the snapshot and its note | CTO (037 §3), builder | the lines are in `docs/MASTER-LEDGER.md` on `main` | **ready** — next PR |
| 14 | Build-19 notes amendment (PDF / itinerary / expenses checks); build 18's `## What shipped` recovered verbatim from App Store Connect | CTO (029 §8) | the record block is present under `## What shipped` with the paste comment; the notes gate is green | **held by founder** — the ASC text must be pasted by her |
| 15 | Activity keying: `ActivityEntry` has no `tripId` — the full §4 migration | CTO (023) | activity entries are keyed by trip in memory and on disk, with the lock's protection proven by a test that reds when two trips share a list | **ready** |
| 16 | The lost six-section reply, resent in parts each naming its sections | CTO (033) | the CTO confirms receipt of every section by name | **in progress** — superseded in substance by 040/041 and this reply; the parts are sent with this reply's manifest |
| 17 | `unseen-branches` reads 165 remote-tracking refs while the API lists 120 branches — 45 stale refs on this machine | builder (found 21:00) | `git remote prune origin` run and the two counts agree, or the difference is explained by name | **ready** |
| 18 | The edit helper reports `bytesBefore/After` as character counts (UTF-8 multibyte differs); rename or count bytes | builder | the field name matches what it measures; the fixture still passes | **ready** |
| 19 | Founder: `sudo xcodebuild -license accept` (lifts the shim taint for every tool); export run on build 18; Resend page readings (#20/#21) | builder / CTO | the composite's toolchain block reads `xcode-select: /Applications/Xcode.app/Contents/Developer` with `git --version` succeeding and `DEVELOPER_DIR` unset | **held by founder** |
