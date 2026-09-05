# Privacy-notice verification — the ten checks

**2026-09-05.** Ten checks were run against the current head of `main`. This
report is the conversation; **part of it is being handled privately** and the
marker below says which part and why.

---

## Why part of this is not here

`CONSTRAINTS.md` — the first thing committed to this repository, deliberately —
forbids anything from the database layer: no migrations, no policies, no
functions, **no schema**. This repository is public.

**Check 8 is an inventory of every table in the application and the columns in
each that hold personal data. That is the schema.**

So check 8 lives in the private repository, where the schema already lives, and
this report carries its *findings* without its *contents*. Publishing the
complete data model of a private application to satisfy a formatting
instruction is the cheap reading of that contradiction, and the cheap reading
is the one the party carrying it always reaches for.

Everything else is below in full.

---

## The headline

**Two lines in the draft notice are provably false.**

1. *"There are no other companies involved in running Amiico today."*
   Three are: the update service the app framework contacts on every launch,
   the weather provider, and Apple — for sign-in as well as for TestFlight.
2. *"Deletion is done by hand by me rather than by a button in the app."*
   There is a working in-app delete. It calls a server function and reports
   only on confirmed success.

A notice that understates the automation is wrong in the direction a regulator
cares about: it tells a person their request reaches a human when it actually
fires code.

## The ten, in order

| # | check | verdict |
|---|---|---|
| 1 | location | **clean** — ten patterns, ten zeros; the device's GPS is never read |
| 2 | analytics / telemetry | **no SDK** — by import, by dependency, and by config plugin — **but see the update service below** |
| 3 | where the database physically sits | **cannot be read from the code** |
| 4 | who sends the sign-in emails | **cannot be read from the code**, with one fact that can |
| 5 | file storage | a private store exists and **nothing writes to it yet** |
| 6 | trip cover photos | **device-only** — the draft is correct |
| 7 | what can appear in an activity entry | **nothing can** — no code writes one |
| 8 | every table's personal-data columns | **handled privately** — see above |
| 9 | deletion | **the draft is wrong** |
| 10 | which hosts receive a request | **three findings and one retraction** |

## Checks 3 and 4 — stated as unreachable, not as absent

Both are settings on a hosted dashboard, not facts in the code. Reporting "no
matches" for either would be a lie of the reassuring kind: **the answer is not
absent, it is elsewhere, and this session holds no credential for it.**

The notice must not name a region or a mail sender until they are read off the
dashboard. What the code *does* say for check 4: no third-party mail service is
configured anywhere, so sign-in email, if it is being sent, is going through
the database provider's own sender — which is a company already named.

## Check 8 — the finding, without the inventory

Twenty-three tables were inventoried. The denominator was derived **twice by
different mechanisms** and the two agree and are set-equal in both directions,
so the total is a measurement rather than a count someone made.

**Five columns hold personal data the notice does not describe. Three of them
belong to people who are not the founder:** an invited person's email address,
an emergency contact's name and phone number, and the sender and subject line
of forwarded booking emails.

> **The notice is written as though the only data subject is the founder. The
> data model disagrees in three places.**

That is the single most consequential finding of the ten, and it is why check 8
was worth running even though it was the dullest.

## Check 10 — and a retraction against my own instrument

Requests leave the device to: the database provider (ours), **the app
framework's update service**, **a weather provider**, and **Apple** — for
sign-in as well as TestFlight. Map buttons hand a URL to the operating system;
the app itself sends nothing to them.

**The update service is the one nobody chose.** It is switched on by default,
contacts its server on **every launch**, and carries a **persistent
per-installation identifier**. Nothing in the application calls it — the check
happens below the application, unconditionally — and no over-the-air update has
ever been shipped. So today it is an identifier sent to a third party in
exchange for a capability that has never been used. The reviewer has ruled it
off; that lands in a build before the first tester, and the notice is written
against **that** build.

### The retraction

I first reported that no outbound network call existed anywhere in the
application, on the strength of a text search for the standard call. **That was
wrong.** The one live request in the codebase is made through a function passed
in as an argument — correct, testable engineering that the search could not
see.

**A search producing a false absence, in my own instrument, on the exact class
this project has corrected three times.** The finding survived; the sentence
supporting it did not. It is also the reason the new gate (below) checks for
hostnames rather than for the spelling of a call: a gate built on the spelling
would have been green from the day it was written and incapable of ever going
red.

### And what the weather request actually carries

Not the user's location — check 1 holds. But not the trip's destination either.
**Nothing sets those coordinates at runtime at all:** the demo trip carries a
hardcoded city centre, and a real session carries zeroes.

So the request is real and the data in it is meaningless, which makes this a
**correctness defect rather than a privacy one** — a real user's morning
weather is requested for a point in the ocean. The fix is not to cache the
request. It is to not make it when there is nothing to ask about, which removes
the request and fixes the feature in the same edit.

---

## The gate that came out of this

**A privacy notice is a factual claim about code — published, dated, and
falsifiable by any commit made after it.** Ten checks passed by hand. That
protects nothing: it was a measurement at a time, and the notice outlives it.

Three of the ten are now checks that fail a pull request — no location, no
telemetry library or plugin, and no outbound hostname outside a reviewed list
where each entry says whether it *receives* data or merely *appears* in a
comment. Each was proven capable of going red by breaking it deliberately and
watching it fail, then restoring byte-identically.

**One gap in it was found by the reviewer and is being closed:** the hostname
check did not scan the configuration files, and the most significant hostname
discovered all day lives in one. A change to it would have reddened nothing.

---

## Appendix — a red build, diagnosed

A recent security change sat behind five consecutive red builds on the job that
gates every database-permission change. The failure was **not** a failed
assertion — every one of them passed. The suite declares in advance how many
assertions it will run, and that hand-written number had drifted one ahead of
the assertions themselves.

**It was not fixed by nudging the number until the build went green**, which is
what makes such a number worthless. It was derived two independent ways, which
agreed, and the file now says in its own header that the number is a constant
sitting beside a computation — so the next person does not nudge it either.

The change merged with all checks green.

---

*Nothing in this report is drawn from anyone's data. No defect described here
is unfixed and exploitable.*
