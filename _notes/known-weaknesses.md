# Known Weaknesses

The register of defects found in this project's own instruments, each under
its own name, stated before anyone else has to find it.

Not yet published. It lives under `_notes/` until there is a methodology
document to carry it, and promoting it to a public page is a separate
decision. Entries are written as though they were already public, because
they will be.

Fixed entries stay. A register that dropped a defect once it was repaired
would be a changelog, and would leave a reader unable to tell an instrument
that has never failed from one whose failures were tidied away.

An entry stays open until the defect is fixed or accepted. A defect that is
accepted rather than fixed says so, and says why.

Four entries: two fixed, two open.

---

## A sparse volume read as full coverage

`sparse-volume-read-as-full` · found 3 September 2026 · **fixed**

**The defect.** The coverage check treated any volume CourtListener held at
all as fully indexed. A citation to a page the corpus had never ingested
therefore resolved as *not found* rather than *not covered*. Scored, that is a
false positive for fabrication recorded against a real case.

**Evidence.** It reported *Mata v. Avianca*, 678 F. Supp. 3d 443, as not
found. That is the most cited real case in this entire field, and the reason
the field exists. CourtListener holds exactly two opinions in volume 678 of
F. Supp. 3d, at pages 1369 and 1371: *SGS Sports Inc. v. United States* and
*Fraserview Remanufacturing Inc. v. United States*, both United States Court
of International Trade, both 2024. Page 443 is nowhere near the ingested
region.

Records, in `CitationRecord/resolutions` at `20f3823`:

| Lookup | Retrieved | Outcome |
| --- | --- | --- |
| 678 F. Supp. 3d 443 | 2026-09-08T05:11:46Z | `not_covered`, `volume_only_partially_held` |
| 678 F. Supp. 3d 443 | 2026-09-08T18:47:29Z | `not_covered`, `volume_only_partially_held` |
| 678 F. Supp. 3d 1369 | 2026-09-10T03:14:33Z | cluster 9467618, court `cit` |
| 678 F. Supp. 3d 1371 | 2026-09-10T03:15:08Z | cluster 9468905, court `cit` |

Both surviving lookups of page 443 already show the corrected verdict, so the
misclassification itself predates the journal and is attested by the commit
that repaired it rather than by a record of it happening.

**What fixed it.** `CitationRecord/tooling` at `9276206`, which states it as
rejecting a sparse volume as evidence of absence. The resolver now probes what
the volume actually holds and returns `not_covered` with the coverage counts
attached.

**Residual, accepted.** The resolver still cannot separate a page CourtListener
never ingested from a page that was invented. Nothing available from
CourtListener can. Such a citation goes to the unscorable bucket with its
coverage counts and an attorney decides. That is the direction the methodology
requires: a coverage gap scored as a fabrication is worse than a fabrication
left unscored, because it inflates error rates against systems drawing on
broader corpora than the ground-truth source.

---

## A phrase query matched reporters by prefix

`reporter-prefix-match` · found 3 September 2026 · **fixed**

**The defect.** Coverage counts came from a phrase query against citation
text. A phrase query is a prefix match, so `citation:("347 U.S.")` also matched
`347 U.S. App. D.C.`. Every reporter whose name prefixes another was counted
as denser than it is.

**Evidence.** Volume 347 of U.S. returned 734 under the phrase query against
691 under the structured filter, the difference being the 43 opinions in
volume 347 of U.S. App. D.C.

**What it caused.** No published figure, but the same false-positive direction
as the entry above. That count is what decides *not found* against *not
covered*. An inflated count makes a volume look well held, which pushes a
citation the corpus does not carry toward being read as fabricated rather than
as uncovered. Two independent defects, both tilting the instrument the same
way, is the part worth remembering.

**What fixed it.** `CitationRecord/tooling` at `9276206`. Counts now come from
the structured citation filter keyed on the reporter field, which matches
exactly, and both the resolver and the census use the same filter so the two
components measure the same thing the same way.

Kept honest by `resolve/tests/test_resolve.py::test_reporter_prefix_does_not_inflate_a_volume`
and `bulk/tests/test_bulk.py::test_a_prefix_reporter_stays_distinct`, and
stated in the census README and in the `volume_holdings` docstring.

**Residual.** None known for this query. The general form has no test and no
fix: a query whose semantics differ from what the caller assumes will return a
plausible number rather than an error, and only a second method disagreeing
will reveal it.

---

## Zeros are recorded without their cause

`zeros-without-cause` · found 9 September 2026 · **open**

**The defect.** A sampled volume returning no opinions may be a gap inside the
range the corpus holds, or a volume past the end of what the corpus holds at
all. The probe records the same zero for both. Every rate computed from those
zeros therefore mixes two quantities that mean opposite things, and nothing in
the output lets a reader separate them.

**Evidence.** Two reporters from the same four-volume probe, generation
2026-06-30, in `CitationRecord/tooling` at `8e8e576` under `results/`:

| Reporter | Draw zeros | Past the extent | Inside the extent |
| --- | --- | --- | --- |
| Cal. App. 5th | 3 of 4 | 3 | 0 |
| F. Supp. 3d | 2 of 4 | 0 | 2 |

Cal. App. 5th returns three zeros because the frame drew volumes 50, 75 and 92
while the corpus stops at volume 39. Those zeros say nothing whatever about
how well volumes 1 to 39 are held. F. Supp. 3d returns two zeros from inside
its held range, where 218 of 700 declared volumes carry no citation. The first
is an artifact of the draw. The second is the sparse district court coverage
this probe was built to find, and it stands.

**What it caused.** The Cal. App. 5th rate, 3 of 4, was repeated as a 75%
coverage figure in a published article and in correspondence with Free Law
Project. It is not a coverage figure. The same reporter counted per cluster
against its parallel reporter gives 84.4%, and counted across its whole
declared range gives 61%. Three numbers, three populations, and the one that
travelled furthest was the one least connected to coverage.

**What would fix it.** Not trimming the draw to the held range. That would
hide precisely what the declared range exists to expose, and would turn a
reporter the corpus barely holds into one that looks complete. The fix is to
record the corpus extent per reporter, classify each zero as interior or
beyond-extent, and report the two rates separately, so neither can be quoted
as the other.

Not applied. It is a change to what the probe records, and the probe is to be
re-run once after both open entries are fixed rather than twice.

**What it does not affect.** The per-cluster parallel-citation counts, which
never use a volume sample. Those are exact over a complete generation and are
recorded separately.

---

## Declared volume ranges drift from the corpus, and nothing reports it

`declared-range-drift` · found 9 September 2026 · **open**

**The defect.** Each reporter's volume range is a declared parameter rather
than a fact read from CourtListener. That is deliberate and correct: asking
the corpus where its volumes stop, and then sampling only that far, would make
every reporter look complete. But nothing compares the declared range against
what the corpus actually holds, so a parameter that is badly wrong is
indistinguishable in the output from one that is well chosen.

**Evidence.** Same generation and commit as above:

| Reporter | Declared last | Last held | Drift |
| --- | --- | --- | --- |
| U.S. | 603 | 606 | corpus runs past the declaration |
| F.3d | 1000 | 999 | close |
| F. Supp. 3d | 700 | 778 | corpus runs past the declaration |
| Cal. App. 5th | 100 | 39 | declaration runs 61 volumes past the corpus |

The drift goes both ways. Two reporters are held beyond their declared last
volume, which means the frame samples a narrower span than the corpus offers.
Cal. App. 5th overshoots by 61 volumes, which is what put three of its four
draws past the end.

**Why it is separate from the entry above.** They have different fixes. The
first is about what a zero means once it has been recorded. This one is about
a parameter being wrong before any volume is drawn. A probe could classify its
zeros correctly and still be sampling the wrong span, and correcting a range
would not tell anyone what an existing zero meant. Folding them together would
repeat the error both entries exist to record.

**What would fix it.** Report declared last volume against last volume held
for every reporter, in the probe's own output, and treat a large divergence as
something to explain rather than something to absorb. The declared range stays
declared. It just stops being unfalsifiable.

Not applied, for the same reason as the entry above.

**What it does not affect.** Anything that reads the citations table directly
rather than through the frame.

---

## A note on why this register starts here

The two open entries were found by trying to reproduce a number this project
had been repeating for days, and failing.

The probe that produced it wrote no artifact. Its output directory was never
created and nothing was committed, so its figures could not be read back from
any file. They were not lost, though: the first run's per-volume counts survive
in prose, in the commit message that added the probe, `17e063c`. F. Supp. 3d as
90, 99, 0, 0 and Cal. App. 5th as 74, 0, 0, 0. Recomputing them from the bulk
citations table reproduces both exactly.

So the figures were checkable, and the first claim made about them here — that
nothing had been recorded — was itself wrong and had to be corrected in the
artifacts that carried it. The defect was never that nothing was written down.
It was that what was written down carried no schema, no provenance stamp, no
population and no caveats, so a number could leave it without the thing it was
measured over. That is how a 3-of-4 sample rate became a 75% coverage share.

Three of the four entries here bias the same way, toward reading an absence or
a gap as a failure by something under test. That is the direction this
benchmark can least afford to be wrong in, and it is worth watching for in the
fifth.
