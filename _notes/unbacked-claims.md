# Claims on the page with nothing producing them

A running list of statements on citationrecord.org that assert a fact no
artifact produces. Not defects in an instrument, which belong in
[Known Weaknesses](known-weaknesses.md), and not statements that are wrong.
These may all be true. The objection is narrower: nothing on the page or in
any repository lets a reader check them, and this project's whole argument is
that an unbackable number should not travel.

An entry leaves this list when something produces the figure, or when the
wording stops asserting more than the evidence carries. Deleting the claim
counts too, and is sometimes the right answer.

Not yet published, like the rest of `_notes/`.

Two entries.

---

## Ninety minutes per quarter

`ninety-minutes` · opened 10 September 2026 · open

**Where.** "Taking part": *The commitment is roughly ninety minutes per
quarter.*

**What produces it.** Nothing. No panel has been seated, no item has been
scored, and no scoring session has been timed. The figure is an estimate
presented in the same register as the credit line and the no-payment
commitment beside it, both of which are decisions rather than measurements.

**Why it matters more than it looks.** It is the only number a prospective
scorer weighs before writing. Understating it recruits people who then leave;
overstating it turns away people who would have stayed. It is also load-bearing
for the claim that participation is light enough not to need payment.

**What would settle it.** Timing the first edition's scoring and replacing the
estimate with what it actually took, or marking it as an estimate until then.
The scoping note in `_notes/scoring-instrument.md` already proposes recording
`started_at_utc` and `submitted_at_utc` per answer set, which would produce
this figure as a by-product rather than as a separate exercise.

**Left in place deliberately** while the surrounding copy was corrected, so
the correction and the estimate are decided separately.

---

## Every vendor claim we surveyed

`every-surveyed` · opened 10 September 2026 · open

**Where.** "What this measures": *Every vendor claim we surveyed reports
existence, or reports nothing measurable at all.*

**What produces it.** Partly something, and less than the sentence needs. The
claim archive at `CitationRecord/claim-archive` carries a tracked claim list of
seven claims, and the archiver in `CitationRecord/tooling` under `archive/`
captures an HTML snapshot, a screenshot, a SHA-256, a Wayback copy and an
append-only manifest line for each one. The manifest currently holds one
entry, and `snapshots/` holds one claim.

**The gap.** *Every* quantifies over a survey whose extent is recorded nowhere.
A reader cannot tell how many claims were looked at, which vendors, on what
dates, or by what criterion a claim counted as reporting nothing measurable.
Six of the seven tracked claims have no captured evidence, and the tracked list
is not reachable from the page at all.

Two separate problems sit inside one sentence. The universal quantifier needs a
denominator. The characterisation of what those claims report is an attorney
judgment that no artifact records.

**What would settle it.** Capturing the tracked claims so the manifest covers
the set, and either citing the archive from the page or narrowing the sentence
to what the evidence carries. A sentence naming how many claims were surveyed
and over what window would be stronger than *every*, and would survive a vendor
editing the page it came from, which is the reason the archiver exists.

---

## What is deliberately not on this list

The page carries several present-tense descriptions of a benchmark that has not
run: that it scores all six classes and publishes the false-positive rate
alongside the catch rate, that test items are held out and rotated, that each
edition carries a permanent identifier, and that vendors receive results
twenty-one days before publication. A forthcoming edition has to describe
itself somehow, and these are commitments rather than measurements.

They are a real question, but a different one: whether the page should mark the
line between what it does and what it intends to do. That belongs in the
restructure, not here.

One statement is neither on this list nor a commitment. "How it works" says the
methodology, prompt set, and scoring rubric are published in full. The rubric
is published. The other two do not exist as documents in any repository. That
is not an unbacked claim, it is a false one, and it needs correcting rather
than recording.
