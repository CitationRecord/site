# Claims on the page with nothing producing them

A running list of statements on citationrecord.org that assert a fact no
artifact produces, or commit to a procedure no component performs. Not defects
in an instrument, which belong in [Known Weaknesses](https://citationrecord.org/weaknesses/), and
not statements that are wrong. These may all be true, or may yet become true.
The objection is narrower: nothing on the page or in any repository lets a
reader check them, and this project's whole argument is that an unbackable
claim should not travel.

An entry leaves this list when something produces the figure, when the
component exists and has run, or when the wording stops asserting more than
the evidence carries. Deleting the claim counts too, and is sometimes the
right answer.

Not yet published, like the rest of `_notes/`.

Three entries. The third has a deadline the other two do not.

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

## A prompt protocol and a hashed query set in components that do not exist

`unbuilt-components` · opened 10 September 2026 · open · **must close before
Edition One is queried, not before it publishes**

**Where.** "How it works", in two clauses: *the methodology and prompt
protocol are published before any results are*, and *the query set is
pre-registered rather than published: the queries and their expected answers
are fixed and hashed before any model is called, and that hash is written to
an append-only record before the first query*.

The second clause originally said the **test items** were fixed and hashed
before testing. That was impossible. An item is a proposition-and-citation
pair extracted from a model's response, so no item exists until after the
models have been queried. Only the queries and their recorded ground truth
exist beforehand, and only those can be hashed. Corrected on the page the day
after it went live; recorded here because the entry has to describe the
commitment that now stands.

**What performs them.** Nothing. The tooling README lists nine components. Five
exist.

| Component | Role | Status |
| --- | --- | --- |
| `archive/` | vendor claim snapshots | built |
| `bulk/` | bulk data loader | built |
| `census/` | reporter coverage | built |
| `parallel/` | per-cluster and per-volume counts | built |
| `resolve/` | citation resolution | built |
| `runner/` | **the frozen prompt protocol** | not written |
| `sample/` | **fixing and hashing the query set** | not written |
| `score/` | scoring interface | not written |
| `build/` | edition assembly | not written |

The two components the new copy depends on are both in the unwritten half.

**Why it is not the same as the other two entries.** Those are claims about
the past with no record behind them. This is a commitment about the future
with no mechanism behind it. All three fail the same test, in that nothing
produces them, but this one can still be made true, and there is a specific
window in which that has to happen.

**The deadline, which is the point of this entry.** Hashing has to occur
before any system is queried. A hash computed afterwards demonstrates nothing:
it is consistent with a query set assembled to suit the results, and no reader
can tell the two apart from outside. Pre-registration claimed after the fact
is worthless, and worse than making no claim, because it asserts a control
that was never applied.

So this entry does not wait on publication. It has to close before the first
query of Edition One, or the sentence on the page becomes unfixable rather
than merely unbacked. The ordering is the control; the hash is only its
record.

**What would settle it.** `sample/` built far enough to fix the query set and
its recorded ground truth and hash both, with the hash written somewhere
append-only before any model is called. The lookup journal in
`CitationRecord/resolutions` is the precedent and probably the venue:
timestamped, provenance-stamped, and reachable. Then
`runner/` far enough to freeze and version the protocol, since the provenance
requirement already names a prompt protocol version as a field a result
cannot be a result without.

**A note on how this got here.** The commitment was written to correct a worse
sentence, which bundled the protocol and the items together and, read one way,
promised to publish the queries. The correction is right. It also moved the
page from an unbacked description to an unbacked promise, which is an
improvement only if the promise is kept.

---

## What is deliberately not on this list

The page carries several present-tense descriptions of a benchmark that has not
run: that it scores all six classes and publishes the false-positive rate
alongside the catch rate, that each edition carries a permanent identifier, and
that vendors receive results twenty-one days before publication. A forthcoming
edition has to describe itself somehow, and these are commitments rather than
measurements.

They are a real question, but a different one: whether the page should mark the
line between what it does and what it intends to do. The restructure did not
settle it and it is still open.

## Closed

**The methodology, prompt set, and scoring rubric are published in full.**
Opened and closed 10 September 2026, and never on this list, because it was
false rather than unbacked: the rubric was published and the other two did not
exist as documents anywhere. The sentence was split into three commitments,
"in full" now applies only to the rubric, and the methodology became an
ordering commitment rather than a completion claim.
