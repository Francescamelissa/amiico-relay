# Day prompt — 22 September 2026

> Received 2026-09-22 07:47 BST (06:47:47 UTC), from the CTO, relayed by Francesca. Not numbered — a brief carrying rulings, filed dated like the 14 September brief. **Filed late, on 2026-09-24:** queue rows 74–77 cite it and it was never committed; the gap was found by an audit of the queue against `rulings/`. Text taken verbatim from the session transcript. Redactions per CONSTRAINTS.md: 1 email address (a synthetic probe address) replaced with `<probe address>`; 1 Supabase project reference replaced with `<project ref>` (when in doubt, it does not go here). Nothing else altered.

---

AMIICO — DAY PROMPT, the morning after the 22 September readings.
From the CTO, relayed by Francesca. She is at work today and unavailable.
Do not stop to ask her anything. Park it instead — §8 says how.

THE STANDING LINES. These do not relax because I am not watching.

- RED LANE = money · RLS · auth · migrations · a new sync entity · security ·
  personal data · any new external service · any new or upgraded native module.
  You may design it, write it, test it and open the PR. You may not merge it.
  HELD FOR CTO. Silence is never approval.
- A merge to main IS a deployment. The Supabase GitHub App applies migrations
  from main within a minute. Treat every migration PR as a deployment
  authorisation: client compatibility, what happens to the build already in
  her hands, and a rollback — or it does not go in the queue.
- No force-push. No history rewrite. No --no-verify, no substitute git, no
  core.hooksPath, no HUSKY=0. Branch protection is never disabled to unblock
  a merge — not once, not briefly.
- Never service_role in the client, in EAS, or in the repo. No secret value
  enters a chat, to me or to anyone, ever.
- No person's data other than Francesca's enters Claude. Counts are fine;
  records are not. NEW RULE from last night: never the contents of a sign-in
  email — the token in a magic link IS the account. Shape only.
- Money in integer minor units, display through formatMinor, exact-string
  assertions, toContain banned.
- npm ci first on every composite. Gates report their denominator. Floor the
  last number before the claim. Screens by NAME, never by number. The
  composite that gates a build runs alone, with its timing recorded.
- Anything that must reach a subagent lives in the repo, not in a message.

§1 — BUILD 19. THIS IS FIRST AND IT IS THE ONLY THING WITH A CLOCK ON IT.

The build in Francesca's hands has been unable to read trips from the server
since 19 September, because the shipped client selects home_currency and the
live column is now currency. The schema reading you were waiting on came back
on 22 September and it is authoritative: 227 columns in public, 44 migrations,
latest 20260916200000. The client-schema gate and the migrations-live-reading
gate now have their denominator.

Do: run both gates against that reading. Run the full composite alone, record
its wall-clock time, report the count and the denominator. Then cut build 19
and submit it. One submission per cut. No re-cut.

Before you submit, prove the fix rather than assert it: extract the Hermes
bundle from the build you are about to send and grep it for home_currency and
for the exact select string. Report the counts. Build 18's bundle contained
home_currency exactly once — build 19's must contain it zero times, and I want
to see the command and the number, not a summary. Report the IPA's sha256 and
CFBundleVersion.

If anything in the composite is red, stop at §1, report the red in full, and
move to §2. Do not work around it. A workaround hides a fault.

§2 — THE PERSONAL-DATA INVENTORY, DONE PROPERLY THIS TIME.

Check C8 of the privacy-notice verification was answered in September against
a tree that has since grown. I now hold the live schema and it contains things
the draft notice does not describe. Redo C8 against the 23 tables as they
actually are, one line per table, naming every column that holds or could hold
personal data, and for each one saying which of the notice's paragraphs covers
it — or that none does.

Pay particular attention to these, because none of them appears in the draft:
  inbound_emails (sender, subject, message_id, raw_source_ref, content_hash)
  trip_inbound_addresses (token, display_prefix)
  email_parse_log (model, input_tokens, output_tokens, cost_usd)
  assistant_usage (user_id)
  wallet_items (confirmation_number, key_detail, original_email_path,
                raw_source_ref, parse_confidence, parse_status)
  subscriptions (provider, product_id, raw jsonb)
  entitlements (key, value, source)
  trip_invites (invited_email, token)
  expenses (receipt_path)
  storage_purge_queue (bucket_id, object_path)
  profiles (avatar_url) and trips (cover_photo_url, email_slug)
  city_legs (lat, lng)
  settlements (from_user, to_user, note)

A schema is not a running feature. The existence of a table is not evidence
that anything writes to it. So I need the second half: a row count per table.

State plainly whether you have any production read path of your own. If you do
not — and I believe you do not — do NOT guess. Write the query instead, as a
single self-verifying block Francesca can paste when she is back, built the way
the last one was: the counts computed in the same result set that lists them,
so a truncated export cannot read as a complete one. That pattern is what
caught the SQL editor silently stopping at 100 rows last time. Put the query in
the repo, not only in your report.

Separately, from the code rather than the database: for each of those tables,
name the call site that writes to it, or state that none exists. wallet_items
and email_parse_log are the two I care most about — risk 04 on the founder page
says the AI email path is dormant and console-switchable, and I want that
claim re-established from the tree today rather than quoted from a week ago.

§3 — THE ERASURE KEYING GAP. THIS ONE HAS A LEGAL DEADLINE BEHIND IT.

The erasure rehearsal walks the FK map from auth.users.id. Two columns hold an
email address as plain text and are not reachable that way:

  trip_invites.invited_email
  inbound_emails.sender

Question, answered with a citation to the runbook rather than from memory: does
erasure-rehearsal-runbook.md delete or redact those two, keyed on the address
string? Yes or no, with the line. If no, that is a defect in the runbook and
the erasure would leave a named person's email address behind after we had told
them it was gone. Write the fix as a PR. Do not run it. RED LANE — HELD.

While you are in there, do the same check for every other text column that can
hold a person's identifier rather than their id: activity_log.summary,
expenses.note, settlements.note, emergency_contacts (name, phone), profiles
(display_name, avatar_url). Report each as covered / not covered / needs a
decision. "Needs a decision" is a legitimate answer and I would rather have it
than a guess.

§4 — THE PROBE'S FULL BLAST RADIUS.

The OTP probe to <probe address> left more behind than
your original report accounted for. Known so far: a Resend send record, roughly
twelve hours of retry attempts across 6–7 September, and a bounce. Likely and
unconfirmed: a suppression entry in Resend, and an unconfirmed row in
auth.users.

Produce the full accounting, then a cleanup proposal. Do not execute any part
of it — auth.users is red lane and service_role is not ambient. The proposal is
the deliverable.

Then write the rule this should have had: a probe against a live external
service states its expected blast radius BEFORE it runs, and its report
accounts for every artefact it predicted plus any it did not. Put it where the
probes live, not in a message.

§5 — THE THREE STRINGS THE PRIVACY NOTICE IS WAITING ON.

You have these written down; I do not have them in front of me. Copy them out
of your own records, with the source of each:

  (a) The region hosting Supabase project <project ref>, exactly as the
      dashboard prints it, plus whether file storage sits in the same region.
  (b) The two outside companies the widened C10 re-run found the shipped app
      talking to — names, what each receives, and the call site.
  (c) The weather provider and the exact wording of its free-use licence
      restriction, since a build gate already depends on it.

Settled and needing no further work, for your records: Resend open tracking and
click tracking are both OFF. Established 22 September from the artefact — a
sent email's own HTML — and not from the settings page. That email contains no
img element, no url() reference, no remote stylesheet and no web font; its only
URL is the verify link, unrewritten, pointing at supabase.co. File that in the
transfer risk assessment with the date and the method. Webhooks is the one
sub-item not yet read and it is Francesca's click, not yours.

§6 — READ PACKETS FOR #280 AND #286.

Both are waiting on me and I will read them next. Prepare each as a packet in
the repo: what changes, what it touches in the red lane, what breaks if the
shipped client does not update in lockstep, and the rollback.

For #286 specifically, the schema already contains a second conversion concept
— expenses.converted_amount bigint alongside expenses.fx_rate numeric — which
I do not believe matches #286's design. Reconcile the two in writing before I
read it. Two meanings in one column is not ambiguity; it is two columns sharing
a name, and we have paid for that once already this month.

§7 — THE PARKED QUESTIONS AND THE ABSENCE-PINNING SWEEP.

Finish the sweep from the overnight report: every place we have concluded
something from an absence, pinned to the command that produced the absence and
its denominator. An argument that rests on an absence must pin the absence.
Rows 8 and 10 are erasure-adjacent and go with §3.

§8 — HOW TO REPORT, AND WHAT TO DO WHEN YOU FINISH.

Manifest at the top: the section numbers you received, so a truncated relay is
visible immediately. Then one block per section, in order, including the boring
ones — a missing section reads as "nothing there" and that has bitten us twice.
Where the answer is "no matches", give the command that produced it.

If you find you were wrong about something earlier, say so plainly and give the
correction. That has been the most valuable thing in every report so far.

Anything needing Francesca goes in a single PARKED list at the end, one line
each, no more than five. Not in the body. She is at work.

Do not stop when you finish §7. Go back to the per-trip keying work — the
change that unlocks a second trip, and behind it companions, sharing and
splitting. It is the biggest thing standing between the app she has and the app
she designed. Open PRs, hold the red lane ones, and keep going.
