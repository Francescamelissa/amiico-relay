# Overnight, 9–10 September

**Head commit: `c79033f`.**

**Nothing was published. No dashboard change, no DNS, nothing touching the
founder's personal domain. No force-push.**

---

## The first line, because it was asked for first

**The inbound-email endpoint authenticates.** It is not an open writer. Detail
in section 1 — but the answer to the question that would have stopped
everything is no.

## And the one that stops something else

**A ruling made tonight would, if carried out, delete every city she has ever
added to a trip.** Section 5. It is returned rather than built on, and the
reconciliation that was ordered first is exactly what surfaced it.

---

## 1 · The inbound email receiver

Read end to end, 310 lines.

| question asked | answer |
| --- | --- |
| what it verifies before any work | **HTTP Basic auth**, checked before the request body is even parsed |
| how it handles missing configuration | **fails closed** — an unset or too-short secret authenticates nothing |
| how it compares the secret | hashes both sides and compares in constant time |
| does it run with elevated privilege | **yes** |
| what it accepts | a mail-webhook payload; rejects anything of the wrong shape |
| is there a second gate | **yes** — a per-trip token that must exist and be marked active. Unknown token writes nothing |
| does any part of the storage path come from the request | **yes, and both parts are sanitised** — the file's own comment names what it is defending against: traversal, separators, null bytes, control characters, absurd lengths, unicode tricks |
| size and type limits | **10 MB per attachment, 5 attachments**, and file types decided by **content inspection, never by the declared type** |
| rate limit | 500 per hour, globally, checked before the token lookup |

**Reading it did not lower my estimate of it.** The order of the checks is the
part that matters: authentication happens before parsing, and the token gate
before any write. It is the most carefully built thing I have read in this
project.

## 2 · Companies that receive anything — and the list was wrong

**Denominator: 10 files in the server directory, 7 of them code. Distinct
hosts found: 3.**

**Two server functions send data to a third party that was on nobody's list.**
One sends **the contents of forwarded emails**; the other sends **screenshots**.

Neither can run today, and I checked each gate rather than assuming:

- the screenshot feature's switch is **off in the code**, and consent is
  separately required;
- the email one is woken only by the inbound receiver, which needs an active
  address token — and **no code path anywhere issues one**;
- both additionally need a key that is not visible from here.

**The gate that backs the recipient list read 197 files and none of them were
server code.** It now reads 204. On its first widened run it went red on all
three server hosts, which is the gap closing in front of us.

**This is the third time "the app's two folders" was treated as "the whole
tree".** It hid a second private storage area, then the only thing able to
write to it, and now this. Server code is excluded from every other check in
this project because it runs on a different platform — **which is precisely why
nothing had ever read it.**

The host is now registered as **reachable-but-switched-off**, with a check that
goes red the day the switch moves — so turning that feature on cannot happen
without the notice being updated in the same change.

**One correction to a rule, recorded where it will be read:** the existing
ruling that this company "is not a processor and does not appear in the notice"
was written about **the assistant conversation on consumer terms**. It says
nothing about **the same company's API called from server code**, which is a
different product on different terms. The two were being read as one.

## 3 · The holds are named

**`HOLD-TRANSFER`** — lifted.
**`HOLD-INVENTORY`** — standing, with its lifting conditions written out
rather than described: the notice must account for sign-in address and device
records, both private storage areas and what can write to them, and the
third-party recipient found tonight.

## 4 · Retention on the sign-in audit records

**The documentation is silent.** It says nothing about how long entries are
kept, nothing about rotation, nothing about deletion, and nothing about
configuring any of it. That is the answer rather than a gap in the reading, and
it makes the fallback wording correct: sign-in records are kept for as long as
the account exists.

## 5 · THE CONTRADICTION — returned, not built on

The ruling was: the second copy of the city list is a projection, so stop
persisting it and rebuild it from the authoritative one.

**It is the other way round.**

- The list she edits has **six of its own write paths**, and **not one of them
  writes to the "authoritative" copy.**
- The authoritative copy is filled from **the server**, and the sync engine in
  this build **has no network at all** — so nothing she has ever created has
  reached it.
- The merge between them already knows this: it **deliberately prefers the
  local list** when local edits exist, and the comment above that line says
  dropping them "would have silently dropped every local-only one."

**So the copy proposed for deletion is the only one holding her data, and the
one proposed as the source is empty.** Carrying out the ruling deletes every
city she has added to every trip.

Containment does not hold, and it does not hold in the direction that makes the
change safe. **Reported before touching the persistence, as instructed.**

**And the ordering is what saved it.** The reconciliation was ordered ahead of
the deletion, and the reconciliation's answer is "do not proceed". That is the
instruction working exactly as intended — the finding is not that the ruling
was wrong, it is that the safeguard placed in front of it caught something a
careful reading of the same files had already missed twice.

**One thing that cannot be built as specified, and it is worth naming.** The
reconciliation was to "fail loudly". On device-local data, with no telemetry
and one user, **a loud failure reaches nobody** — the only observer is the
founder, and it would surface as an alarm about something she cannot act on. A
reconciliation here has to either repair silently or be checked before the
build ships. I would build the second.

## 6 · The tooling rule

Not yet written into the rulebook. **It earned a fresh receipt tonight**, which
belongs in it: a commit message written with backticks inside a shell string
had three words **executed as commands and deleted from the message** before it
was recorded. It cannot be corrected, because correcting it would mean
rewriting history. The full text went into the request instead.

Twice more tonight, the same rule's other half **worked**: two edit scripts
stopped before writing because their anchors did not match, leaving the files
untouched instead of half-changed.

## 7 · Per-trip separation

**Not started, and section 5 is why.** The next step was to write a failing
test for the current shape — and the shape is not what the ruling assumed, so
that test would have pinned the wrong answer.

---

## What is waiting for a person

- Which of the two copies is authoritative, given section 5.
- Paste build 17's notes into the store console — they are written and ready.
- The sign-in destination reading, still first.
- Confirm one email-delivery setting is off.
- The server-function list, which decides whether tonight's third-party finding
  is live or merely written down.

*Nothing in this report is drawn from anyone's personal data. No unfixed,
exploitable defect is described here.*
