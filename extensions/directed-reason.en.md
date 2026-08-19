# The directed reason as a retrieval unit

## A proposed addition to OKF-Legal (extending §3.1)

*Draft for community discussion. August 2026.
[Polish original](./directed-reason.md) — this is a condensed English version
of the same argument; section numbers match.*

---

## 1. What we didn't invent

An honest anchor has to come first, or everything after it looks like taking
credit for someone else's idea.

**The idea that the retrieval unit should be a claim, not a text fragment,
already exists in NLP and is documented.** Chen et al., in *Dense X
Retrieval: What Retrieval Granularity Should We Use?*, introduced the
**proposition** — an atomic, self-contained statement carrying one fact — as
an indexing unit, and showed it beats sentences and passages for
domain-specific retrieval. A related line of work is atomic-fact
decomposition in claim verification and factuality evaluation.

Our contribution isn't discovering that a smaller unit retrieves better.
**A directed reason is Chen et al.'s proposition, extended with the layers
that legal text requires and encyclopedic text does not.** A proposition
works where a sentence is a fact. In a judgment's reasoning, a sentence
usually isn't a fact — it's a position in a dispute, and a position without
knowing whose it is and how it was resolved isn't usable in a brief.

---

## 2. Why a proposition isn't enough in law

A proposition is, by definition, **directionless**. "The fee amounted to 25%
of the principal" is a fact. But a sentence from a judgment's reasoning is
almost never just a fact — it's a **reason in a dispute**, voiced by someone,
in someone's favor, in a particular mode, at a particular point in the
argument.

A lawyer's query is "give me the reasons against thesis X," not "give me
sentences about X." If the retrieval unit carries no direction, the query's
result mixes both sides' arguments and is, in practice, misleading — because
counsel citing, in a brief, a sentence in which the court merely reported the
opponent's position, has made a serious drafting error.

**Directed reason** — a claim extracted from legal text, anchored to a
verbatim quote, tagged with whose favor it was resolved in and where it sits
in the argument.

Formally: **directed reason = a proposition + four layers** that a
proposition, in the general sense, lacks:

| Layer | Carries | Why the reason is useless without it |
| --- | --- | --- |
| **Direction** | in whose favor this reason was resolved | without it, "reasons for" and "reasons against" come back mixed |
| **Mode** | *ratio* or *obiter*, the court's own reasoning or merely reported | citing *obiter dictum* as a basis for the ruling is a drafting error |
| **Source instance** | whether this reasoning is this court's own, or inherited from a lower court | otherwise a statistic counts a reversal as a win |
| **Position** | its place in the parent document's argument | lets the **order** of the reasoning be reconstructed, not just its components |

On top of that comes **anchoring**: a reason without a verbatim quote from
the source is a model's claim, not a fact from the judgment. That is a
necessary condition for procedural use, and the only layer that makes the
whole thing auditable.

---

## 3. What follows, and what a proposition alone can't give you

Two consequences are non-obvious, and they're the reason this deserves its
own concept.

**Position turns a set of quotes into a sequence.** A court's reasoning
isn't a bag of arguments, it's their order: characterizing the institution,
purpose, interpretation, subsumption, decision. Because every reason has a
quote anchored in the text, that order is **deterministically**
reconstructible — without asking the model "how did the court reason."
Reconstructing the argument stops being generation and becomes sorting.

**Direction turns a content search engine into a holding search engine.**
Only once a reason carries direction can it be aggregated with reasons from
many judgments into a **disputed axis** — an issue on which case law splits
into two camps, with a distribution and representatives on both sides. A
disputed axis is an entity that cannot be built from documents, chunks, or
directionless propositions.

---

## 4. Empirical evidence

**Retrieval accuracy.** In a blind test (a model judge unaware of which
retrieval path produced each answer, 30 questions), retrieval over directed
reasons gave a direct answer for **75%** of practical questions and **80%**
of edge-case questions of the type "give me the courts' reasons for thesis
X." Classic document chunks: **45%** and **30%** respectively. The gap grows
with question difficulty, which matches the intuition: a chunk describes the
neighborhood of a topic, a reason answers the question.

**Blind fusion hurts.** Combining both tracks with RRF on a short result
list **dilutes** the reasons (65% and 70% instead of 75% and 80%). The
operational conclusion: reasons lead, chunks serve as a context window
around a hit reason, and wide fusion only makes sense before reranking, not
instead of ranking.

**Scale.** The reason level, running in production: **135,090 directed
reasons derived from 1,095 judgments**, with 95% of quotes matching the
source character-for-character (median 97% per judgment). This is not a
demo — it is the corpus a law firm works from.

**Something a chunk can't do at all.** From directed reasons we built a
registry of **490 disputed axes** with a resolution distribution on both
sides. The query "exactly where does case law split on this issue and who
stands on which side" is unanswerable in a chunk model, because it requires
a unit that carries direction.

---

## 5. Factor X — what resists both arithmetic and categorization

Section 4 shows directed reasons retrieve better. That's a technical
argument, and ultimately not the point. The deeper reason is that **in a
legal ruling, something is left over that can neither be counted nor
categorized** — and a data unit either carries that residue forward, or
destroys it.

### 5.1 The residue is measurable in its existence, if not in its content

This isn't a philosophical claim. We measured it with our own tools, three
times, against our own expectations each time.

**Argument statistics collapse under control.** When we moved from a raw
outcome percentage to a lift above a comparable-case baseline, **most of the
differences between claim types fell within a few percentage points, i.e.
noise**. Of 490 disputed axes, only two carried real information. The rest
of the rankings that circulate in this field measure case selection, not
argument strength.

**Categorization loses what decides.** An attempt to build a "pro-consumer
judgment archetype" from labels ended in a tautology — the "decisive reason"
qualifier turned out to be empty, because 97–100% of present issues have a
decisive reason. The class doesn't distinguish cases, because the difference
lives inside the class.

**Effectiveness is local, not general.** The largest deviation in the
entire corpus turned out, on inspection, to belong not to a court as such,
but to one division, after a case-law reversal in a specific year, carried
out by three judges, one of whom decided two-thirds of those cases. What
the statistic reported as a "court effect" was actually an event embedded in
a bench and a moment in time.

The conclusion isn't "statistics are useless." It's: **statistics mark the
boundary of what can be generalized, and beyond that boundary lies factor
X** — the fit between one specific formulation and one specific bench's
frame of mind, at one specific moment in a case-law line.

### 5.2 Why chunks and tables each destroy X in their own way

**A chunk destroys X by blurring.** A passage fragment carries the
neighborhood of a topic, but loses whose reason it expresses and where it
sits in the argument. It returns topical similarity, and X lives in the
difference, not the similarity.

**A category destroys X by flattening.** A label like "an informational
defect" merges a technical omission with a genuine information gap that
actually prevented assessing an obligation. Once assigned to a class, the
difference stops existing in the data, even though in the judgments it
decides the outcome.

**A number destroys X by aggregation.** A percentage is, by definition, an
operation that removes the individual case. And in law, the individual case
is sometimes the entire value: one sentence, phrased a particular way, is
exactly what transfers into a brief.

### 5.3 A directed reason is a unit that carries X forward

The four layers from §2 aren't decoration — each protects a different
dimension of the residue.

**The verbatim quote** preserves the formulation instead of a summary of it.
This is X in its purest form: a paraphrase describes what the court talked
about, a quote preserves **how** it said it, and in legal argument, the
difference between the two is sometimes the difference between winning and
losing.

**Direction** preserves the dispute. Without it, only the topic remains, and
X lives in the fact that two courts ruled oppositely on the same topic, and
it's worth knowing in which words.

**Position** preserves the arc. The order of the links is part of the
argument, not its metadata; the same reason, first in line versus fourth,
means something different in the reasoning.

**Mode and source instance** preserve the statement's status — whether it's
a basis for the ruling, a side remark, or someone else's reasoning inherited
by a higher court.

In other words: **a directed reason is designed so an aggregate can be
derived from it, but never replaced by it.** A disputed axis's statistic is
a view over reasons, not a self-standing entity; from every number you can
descend back to the quotes that produced it, and see how those cases
actually differed beyond the category.

### 5.4 Consequence for the profile

From this follows a rule stronger than a technical recommendation:

> **A layer of aggregates must not be published without the layer of
> reasons it was built from.** A bundle containing only distributions and
> statistics is, in this profile's sense, incomplete, because it denies the
> consumer the ability to descend to the material where factor X lives.

This is also the test for the whole idea's value. If the residue doesn't
exist, tables are enough and the profile is overengineering. Our three
measurements say it exists, and that it's large — which is why the base
unit has to be something that carries it.

### 5.5 An honest limit: X is not mysticism

Factor X is not a call to abandon measurement, nor a cover for lacking a
method. It is the residue left after subtracting what's explained, and it
splits into at least two parts that must be told apart.

The first part is **residual argument**: the real unpredictability of a
ruling, coming from how one specific argument fits one specific case and one
specific bench.

The second part is **residual measurement**: our own errors — categories
that are too coarse, labeling drift, corpus limits. This part is reducible,
and reducing it is an obligation, not an ambition.

Conflating the two is the most common abuse in this field: one's own
measurement shortcomings get sold as a mystery of the law, and genuine
unpredictability gets sold as a defect to be fixed with a bigger model. A
profile should require that a consumer declare which part it is describing.

---

## 6. Limits — what this thesis costs

This section wouldn't be honest without this paragraph.

**Directed reasons aren't free.** They require extraction with a dosage of
literalness control, and that costs money. In our case the order of
magnitude is roughly a dollar per judgment with a hybrid model choice, down
from six.

**The reason level requires a map of the parent document's sections.**
Judgments ingested without a segmented reasoning section turned out to be
invisible to the extractor — they couldn't be broken down at all. That's an
argument for a conformance rule: a bundle declaring the reason level must
carry a section map with anchors.

**The rule for deriving an axis's resolution must be declared.**
Statistics computed from reasons depend on how "resolved in favor of" is
defined for an axis. Two bundles, both conformant with the profile, using
different aggregation rules, will give contradictory numbers — we checked
this on our own data, and the differences ran to dozens of percentage
points.

**An axis's resolution share does not measure argument strength.** It
mainly measures the selection of cases in which a given argument tends to be
raised at all. A profile consumer should be warned of this directly — it is
the most common trap in case-law analysis.

---

## 7. Proposal for discussion

Left open for the community to resolve: should the direction layer be
mandatory for every reason, or only for those derived from the dispositive
part of the reasoning; and what to call units that carry no direction
(general theses, test elements) — do they remain directed reasons with an
empty field, or form a separate type?

The claim we're putting forward for discussion is this: **in legal text, the
retrieval unit is neither a fragment nor even a claim, but a claim with a
direction and a position in the argument** — because what decides a legal
ruling rests, to a significant degree, on something that resists both
arithmetic and categorization, and a data unit either carries that residue
forward or permanently erases it. The rest of the profile follows from this
one decision.

---

### External sources

- Chen et al., *Dense X Retrieval: What Retrieval Granularity Should We
  Use?* — the proposition as a retrieval unit, arXiv:2312.06648.
- Min et al., *FActScore: Fine-grained Atomic Evaluation of Factual
  Precision in Long Form Text Generation* (2023) — atomic-fact decomposition
  in factuality evaluation; an adjacent inspiration, a directionless fact by
  definition (the line continues: VeriFastScore, DnDScore, OpenFActScore).
- Bhattacharya et al., *Identification of Rhetorical Roles of Sentences in
  Indian Legal Judgments* (2019, arXiv:1911.05405) and follow-ups: Malik et
  al. (13 roles, multi-task learning), Bambroo et al. *MARRO* (2025,
  arXiv:2503.10659), *LegalSeg* (2025, arXiv:2502.05836), *Segment First,
  Retrieve Better* (2025, arXiv:2508.00679) — the closest existing line to
  the "mode" layer; a rhetorical role without direction and without
  aggregation into a distribution.
- Poudyal et al., *ECHR: Legal Corpus for Argument Mining* (ACL 2020,
  argmining-1.8) and follow-ups (373 judgments, Springer AI & Law 2023) —
  premise/conclusion structure without procedural status (ratio/obiter).
- Shepard's Citations / Westlaw KeyCite (`followed`/`distinguished`/
  `overruled`) and a fresh LLM benchmark *Validate Your Authority* (2026,
  arXiv:2605.17691) — the only existing realization of the "direction"
  layer, but only at the level of a whole judgment-to-judgment citation,
  never a single argument, and without aggregation into a distribution like
  a disputed axis.
- SAILER (arXiv:2304.11370) and ReaKase (arXiv:2510.26178) — document
  structure as a case-to-case matching signal, not atom extraction.
- Lex Machina / Trellis — litigation analytics on case metadata
  (judge/motion type/firm), with no "reason" unit at all.

Full review with citations and a comparison table: `prior-art-and-lift-metric.md`
(Polish; a full English translation is a candidate for a follow-up). None of
the reviewed work combines direction + mode + source instance + position +
quote in one unit that aggregates into a countable disputed axis — the
review's result strengthens, not weakens, this section's claim.
