# Known Weaknesses

The register of defects found in this project's own instruments, each under
its own name, stated before anyone else has to find it.

Not yet published. It lives under `_notes/` until there is a methodology
document to carry it, and promoting it to a public page is a separate
decision. Entries are written as though they were already public, because
they will be.

An entry stays open until the defect is fixed or accepted. A defect that is
accepted rather than fixed says so, and says why.

Two entries so far, both in the census sampling frame, both found while
tracing an unsupportable figure back to where it came from.

---

## Zeros are recorded without their cause

`zeros-without-cause` · opened 9 September 2026 · open

**The defect.** A sampled volume returning no opinions may be a gap inside the
range the corpus holds, or a volume past the end of what the corpus holds at
all. The probe records the same zero for both. Every rate computed from those
zeros therefore mixes two quantities that mean opposite things, and nothing in
the output lets a reader separate them.

**Evidence.** Two reporters from the same four-volume probe, generation
2026-06-30, in `CitationRecord/tooling` at commit `d500a9c` under `results/`:

| Reporter | Draw zeros | Past the extent | Inside the extent |
| --- | --- | --- | --- |
| Cal. App. 5th | 3 of 4 | 3 | 0 |
| F. Supp. 3d | 2 of 4 | 0 | 2 |

Cal. App. 5th returns three zeros because the frame drew volumes 50, 75 and 92
while the corpus stops at volume 39. Those zeros say nothing whatever about
how well volumes 1 to 39 are held. F. Supp. 3d returns two zeros from inside
its held range, where 218 of 700 declared volumes carry no citation. The first
is an artifact of the draw. The second is the sparse district court coverage
this probe was built to find.

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
beyond-extent, and report the two rates separately, so that neither can be
quoted as the other. No fix is applied yet; the classification is a change to
what the probe records, and the probe has not been re-run since.

**What it does not affect.** The per-cluster parallel-citation counts, which
never use a volume sample. Those are exact over a complete generation and are
recorded separately.

---

## Declared volume ranges drift from the corpus, and nothing reports it

`declared-range-drift` · opened 9 September 2026 · open

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

**What it does not affect.** Anything that reads the citations table directly
rather than through the frame.

---

## A note on why this register starts here

Both entries were found by trying to reproduce a number this project had been
repeating for days, and failing. The probe that produced it wrote no artifact:
its output directory was never created and nothing was committed, so there was
no record to check the figure against, and the figure travelled on memory
alone.

That is the failure this benchmark exists to measure, occurring inside the
benchmark. It belongs at the top of this register rather than in a footnote to
it.
