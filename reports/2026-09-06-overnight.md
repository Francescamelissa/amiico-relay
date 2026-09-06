# Overnight, 5–6 September

Written when I stopped, not as a stream. **Denominators on everything,
including the boring ones**, and where the answer is "no matches" the command
is given so it can be re-run.

**Nothing was published. No dashboard change, no DNS, nothing touching the
founder's personal domain.**

---

## The headline

**Build 17 is with Apple**, and the automatic update check is switched off *in
the binary she installs* — read out of the compiled artefact, not argued from a
config file.

**One finding cancels a piece of tomorrow's plan** and one **raises the urgency
of a reading**. Both are below under their numbers.

---

## 1 · Merged

Two changes landed: the gate that refuses a read of a write-only field before
its read path exists, and the documentation change that makes one file the
index of where everything is recorded.

The index property is the one that matters: **searching one file and finding
nothing now means it is not recorded anywhere.** That was false yesterday and
it produced a wrong finding.

## 2 · Build 17 — cut, verified, submitted

| | |
| --- | --- |
| **built from** | the commit that carries the update-check change |
| **the value read** | the update flag in the compiled property list: **false** |
| **where it was read** | `Payload/Amiico.app/Expo.plist` inside the delivered installer |
| **identity** | bundle id unchanged, version 0.1.0, build 17 |
| **submitted** | once, accepted by Apple, processing |

**What I am claiming and what I am not.** The flag being false is a property of
the binary a tester installs, and that is provable, and I proved it. It does
**not** establish that no request ever leaves the device — that would require a
traffic capture I have no means to make, and would only ever be ruling out a
defect in somebody else's library that we have no reason to suspect. **The
notice should say the automatic update check is off in the app she installs,
and should not say that nothing ever leaves the device.**

One honest wrinkle, recorded so nobody later reads the file and thinks I
misreported: the update service's address and a "check on launch" setting are
**still present** in that property list. They are inert while the flag is
false — the flag is the master switch — but they are there, and a reader
skimming the file could reasonably be alarmed by them.

## 3 · The sign-in redirect — a finding that raises tomorrow's reading

Yesterday's probe could not answer whether the allowlist matches exactly or by
prefix, and the control is what proved the probe useless. So I read the
authentication server's own source instead.

**Two answers, and the second is the one that matters.**

**(a)** Each allowlist entry is compiled as a literal pattern unless it contains
a wildcard. So an entry with no wildcard matches **only itself**. That makes one
of the two entries on the list **unreachable** — nothing ever requests exactly
that string — and it can be removed. A removal cannot break anything that
currently works.

**(b) When a requested destination is not on the list, the server does not
reject it. It silently substitutes the project's Site URL.**

That is why a request naming an obviously hostile destination came back
successful yesterday. And it means **the Site URL is where a sign-in link goes
whenever the requested destination is not allowed** — which makes it the single
most security-relevant value in the whole authentication configuration, and
**nobody has read it.** The screenshot showed the allowlist only.

Our own app cannot produce this situation: it filters every destination before
asking. The exposure is a request that never came from our app — which is the
whole reason a server-side list is the control and a client-side one is not.

**This does not need her tonight.** It moves that reading from routine to first
thing.

## 4 · The personal-data inventory, second half

The inventory that shipped covered one part of the database — twenty-three
tables — **because the brief said "all twenty-three" and that number got baked
into the check itself.** Personal data does not stop there.

Measured against a real instance in continuous integration, so the answer comes
from the actual versions in use rather than from reading a vendor's
documentation and hoping ours matches.

**Denominator: 33 tables. Derived three separate ways by three different
internal catalogues, printed side by side: 33, 33, 33.** Twenty-three in the
authentication schema, ten in the storage schema.

**What is there that the notice does not describe:**

- **Every email address lives here**, not in the part already inventoried,
  along with phone number, sign-in timestamps, and several single-use tokens.
- **Sessions record IP address and the device's browser identification.** This
  is more sensitive than most of what the shipped inventory covered.
- **The audit log records IP address** against each entry.
- **Sign-in provider records** hold the claims a provider returns, including an
  email address — this is where an Apple or Google identity would land.
- Multi-factor and passkey tables exist and are empty, but they exist.

**Storage: ten tables, protection enabled on all ten**, with rules defined on
the objects table only — which means everything else is closed to ordinary
users by default rather than left open.

**And a correction to my own earlier work: there are TWO storage buckets, not
one.** I reported one. The second is declared in the same file, whose opening
line says "buckets" in the plural. I read the statement that created the first
and stopped. Both are private, both capped at ten megabytes.

**What I could not measure: whether the file storage physically sits in the
same country as the database.** That is not visible from the code or from the
test instance. It needs the dashboard.

## 5 · The sweep for tooling that reports success it never checked

**Denominator: 21 scripts** — counted two ways, by a filesystem walk and by the
version-control index, agreeing at 21. Four defect classes searched.

**Result: zero real offenders.** Every reported outcome in every committed
script is derived from an actual result.

Two apparent hits were my own instrument being wrong:

- A missing safety setting in two shell scripts — **false**. I had only read
  the first five lines of each. One has it on line 95; the other omits it
  **deliberately**, and its own header explains why: it must run every check
  even after one fails, because the whole point is the full picture.
- Three suspicious pipeline constructs — **false**. All three were ordinary
  boolean logic, caught by a pattern of mine that was too loose.

**The honest conclusion, and it relocates the problem: every instance of this
defect in this project's history came from throwaway tooling written for a
single task, not from anything committed.** The committed scripts are
disciplined because each one was written *after* a defect taught the lesson.
So the remedy is not a fix to those files — there is nothing there to fix — it
is the discipline that landed yesterday, and I would like to make it mechanical
rather than remembered.

## 6 · Per-trip data separation — the reading phase, and a real finding

Three parts of the app already separate data by trip. **They do not agree, and
the disagreement is the answer to the question that was asked.**

Two of them hold **every** trip's data at once, indexed by trip. The third
holds **only the current trip's**, and is refilled from one of the other two
whenever the trip changes.

So the third is not separated-by-trip at all. It is a **view** of one of the
others.

**And it is worse than a clean view: it is also saved to the device
independently.** The same information is therefore stored twice, with a
one-directional refresh — the source updates the view, and nothing updates the
source. Two copies that agree on the day they were written and can drift apart
silently afterwards, where **the copy that gets displayed is the derived one.**

That is the same shape as a defect this project has already recorded twice in
other forms. It has to be settled *before* the remaining parts are separated,
because it decides what "separate this one" even means for each of them — a
source needs migrating, a view needs swapping and clearing, and they are not
the same job.

**I stopped there rather than starting the change.** The next step is the
failing test that proves the current shape is wrong, and writing it against a
question this size that is still open would have produced a test asserting
whichever answer I happened to assume.

## 7 · Drafted, not applied

The line for the sign-in emails naming a real contact address, so a person with
a question is told where to write **inside the message** rather than discovering
their reply bounced. Written for her to apply. **No infrastructure of any kind:
no address created, no mail routing, no domain change.**

---

## What is waiting for a person

- **Read the Site URL** in the authentication settings. Section 3 explains why
  this is now first rather than routine.
- Remove the redundant entry from the redirect allowlist — a removal, and it
  cannot break anything that currently works.
- Confirm one email-delivery setting is off. If it is ever on, it starts
  collecting IP address, location, operating system, browser and device — and
  **no code of ours can turn it on or off**, so this can only ever be a reading.
- Whether file storage sits in the same country as the database.
- Apply the drafted email line.

## What I would do next

Settle the view-versus-source question from section 6, then the failing test,
then the first part of the separation work.

*Nothing in this report is drawn from anyone's personal data. No unfixed,
exploitable defect is described here.*
