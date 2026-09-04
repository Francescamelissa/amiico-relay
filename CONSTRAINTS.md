# Constraints for this repository

**This is the first thing committed here, deliberately.** It governs everything
that follows, including every report.

This repo exists for **conversation only** — build reports, findings, decisions
and questions between the builder and the reviewing CTO. It replaces a person
carrying messages by hand.

## What must never appear here

- **No code.** The application repository is and stays **private**.
- **Nothing from `supabase/`** — no migrations, no policies, no functions, no
  schema.
- **No connection strings, no keys, no tokens, no credentials** of any kind,
  including examples, redacted-looking ones, and anything that merely resembles
  one.
- **No personal data** — not the founder's, not a tester's, not anyone's. No
  email addresses, no names attached to data, no exported records, no
  screenshots containing either.

## The standing rule that created this repo

**A public repository must not carry a discussion of a live vulnerability.**

This was the condition on which the repo was authorised: it stayed blocked
until the `erase_user_admin` finding was closed *and proven closed against the
hosted database*.

**That rule did not expire when the block lifted.** It applies to every future
report:

> **A defect that is not yet fixed is not discussed here while it is
> exploitable.** It goes to the CTO privately, and arrives here only once it is
> closed — or once it is established that describing it cannot help anyone
> reach it.

If a report cannot be written without breaking that, **the report waits — and
the wait is announced.**

> **When a report cannot be written without breaching this, say so here:
> "an item is being handled privately."**

**A gap with no marker looks like nothing happened.** Silence and absence are
indistinguishable from outside, so an unexplained quiet period reads as no work
rather than as withheld work — which is the same defect this repository exists
to remove, arriving as a hole instead of a claim.

## Why the separation is worth the friction

The code repository is private because it contains the schema, the policies and
the shape of every guard. This repository is public because a conversation
about *how the work is going* does not need any of that — and because the cost
of the two ever being confused is not recoverable by deleting a file.

**When in doubt, it does not go here.**
