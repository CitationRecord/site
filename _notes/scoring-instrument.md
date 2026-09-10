# The scoring instrument — scoping note

Status: scoping only, nothing implemented. Written 9 September 2026 against
rubric v0.1 and the demonstration at `/rubric/scoring/`.

The demonstration is a static page with sample data. This note covers what the
real instrument has to do that a static page cannot.

---

## Blinding

The client receives: an opaque item id, the proposition as stated, the citation
as emitted, the jurisdiction specified in the query, the corpus passage with its
context levels, and the mechanically settled findings. It receives no system
name, no model identifier, and no field from which one can be derived.

Two things that leak if unattended:

- **The item id.** It must be random, or a keyed hash whose key never leaves the
  server. An id derived from `(system, index)` is a blind in name only.
- **Prose style.** A model is often identifiable from its phrasing. This cannot
  be mitigated without editing the artifact under test, which would destroy it.
  Treat it as a residual risk, disclose it, and record scorer-guessed provenance
  as a separate optional field so its effect can be measured rather than assumed.

The item-to-system mapping lives in a server-side table the scoring client never
queries. It is revealed when scoring for that item closes: every assigned scorer
has submitted and any disagreement has been adjudicated. Reveal is an
edition-level event with its own timestamped record.

## Record integrity

Scoring answers are the report's raw material and need the properties
`citation-resolutions/lookups.jsonl` already has: append-only, timestamped,
version-stamped, no silent edits.

Proposed shape, one object per submitted answer set:

```
schema            citationrecord.score.v1
record_id         random, unique
item_id           opaque
scorer_id         pseudonymous, stable across the edition
answers           [{question, value, note, context_depth}]
context_final     {up, down} at submission
started_at_utc    when the item was first rendered
submitted_at_utc  when the set was committed
rubric_version    v0.1
provenance        {instrument_version, commit, dirty, api}
prev_hash         hash of the preceding record
supersedes        record_id, when correcting an earlier submission
```

Nothing is updated in place. A correction is a new record naming what it
supersedes, and both are retained. The `prev_hash` chain makes a silent edit
detectable rather than merely discouraged, which is the whole point of keeping
the journal at all.

## Context depth

The prototype tracks expansion depth once per item. That is not sufficient. Depth
is a variable in inter-rater analysis, and a scorer may expand further between
answering the first question and the third, so a single item-level figure
attributes the wrong context to at least some answers.

Record depth per answer, as it stood when that answer was committed, and keep the
final item-level figure alongside it. This is a change to the prototype's model,
not just its storage, and is the one substantive gap between the demonstration
and the instrument.

## Assignment

- Every item is scored independently by at least two scorers, with no visibility
  of each other's answers or notes until adjudication.
- Full double-scoring of roughly 300 items is about 600 scoring events. If that
  exceeds the judgment budget, double-score a random sample plus every item where
  the first scorer answered "Can't tell", since those are where disagreement
  concentrates.
- Each scorer's queue is shuffled across all three systems, so nobody scores one
  vendor in a block. Order is drawn per scorer and recorded.
- Assignment is computed server-side and fixed before scoring opens, then written
  to the journal so the allocation can be audited afterwards rather than taken on
  trust.
- Publisher recusal from Citation Firewall items is enforced in the assignment
  step. It is a disclosed commitment and should not depend on anyone remembering.

## Where it runs

Docker on the Iceland VPS is appropriate. The workload is negligible: a few
hundred items, a handful of concurrent scorers, no compute of any kind. One small
container and SQLite in WAL mode would carry it; Postgres if you want it. The
binding requirements are durability and integrity, not scale, so the things to
get right are off-box backups and mirroring the closed journal into the repo.

The question not asked: hosting scoring data outside the US.

The data is thin — pseudonymous ids and professional judgments about published
court opinions. No client matter, nothing privileged. Four things still worth
putting to counsel before scoring opens:

1. Iceland is in the EEA, so GDPR governs scorer identity data regardless of the
   publisher being US-based. That needs a lawful basis and a stated retention
   period, not an afterthought.
2. Participating attorneys are identifiable and their recorded judgments name
   vendors. If a vendor disputes a result, that record is discoverable, and where
   it sits affects how that process runs.
3. Contributors are credited by name, so consent framing has to cover both the
   credit and the underlying record.
4. None of this argues against Iceland. It argues for a one-page data policy
   published before the first item is scored.

The structural mitigation is cheap: keep scorer identity in a separate store from
the answers, joined only by pseudonymous id, so the journal that eventually gets
published carries no personal data at all.

## What it must not do

**The instrument assembles evidence and records human judgments. It does not
form any of its own.** This is a constraint on implementation, not a preference.

Specifically, no AI classification, ranking, flagging, or recommendation. No
pre-filled answers. No ordering of options by likelihood. No highlighting that
implies a conclusion. No surfacing of how other scorers answered, or how similar
items were scored, at any point before adjudication.

The settled-findings block marks the boundary. Deterministic resolution against
the corpus is permitted and shown, because it can be checked. Anything
inferential is not, whoever or whatever produces it.

Class assignment happens after scoring closes, by fixed rules applied to the
recorded answers. That is mechanical derivation, not judgment, and stays within
the constraint.
