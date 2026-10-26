# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter classifies posts and comments from Reddit's r/space community by
what kind of contribution they make to the conversation. Every post gets one
of three labels: `reaction` (an emotional or conversational response),
`hot_take` (a confident claim with no support), or `analysis` (an argument
backed by a specific, checkable fact). Most threads in the sample argue about
the same topics, like colonizing Mars, JWST, Artemis/SLS, and whether space
spending is worth it, so the classifier has to go by how a post argues, not
what it's about.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `reaction`

**Definition:** An immediate response to something that just happened (a
launch, a new image, another commenter's post), expressing a feeling or a
conversational move rather than arguing a position.

**Example 1:**
> as someone who is seeing the comparison between hubble photos and jwst
> photos, i am absolutely blown away

**Example 2:**
> I was on James Island and caught it. Cool stuff to even see the boosters
> fall away.

### `hot_take`

**Definition:** A confident claim about how things are or should be, with no
support offered.

**Example 1:**
> The JWST hype is basically NASA trying really hard to burry the fact that
> they wasted $10 billion for a $1 billion telescope.

**Example 2:**
> Colonizing the most inhospitable spot on the surface of the Earth would be
> **trivial** in comparison to colonizing any other body in the solar system


### `analysis`

**Definition:** Makes an argument backed by a specific, checkable fact, such
as a number, a measurement, a named mission or study, or a source link.

**Example 1:**
> Titan has a non-toxic atmosphere with a pressure of 1.45 atm made up of
> 94.2% nitrogen. It's similar to how earths atmosphere was before life but
> thicker. It's definitely cold there though.

**Example 2:**
> We're not going to have an exponential increase in consumption because
> researchers estimate our population will peak at around 11 billion, at
> worst: [https://www.nature.com/articles/d41586-021-02522-6](https://www.nature.com/articles/d41586-021-02522-6)

### The hardest boundary

**Which two labels:** `hot_take` vs. `analysis`

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

If the post gives at least one specific fact you could check (a number, a
measurement, a named mission or study, or a link) **and uses it to support its
claim**, it's `analysis`, even if the tone is confident or rude. If the claim
stands on its own, or the only "support" is more opinion ("everyone knows",
"it's obvious"), it's `hot_take`, however long or detailed the post is.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** A Kaggle dataset of sample posts and comments
from Reddit's r/space. Blank rows from the export were removed, leaving 199
labelled posts.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
TODO: say plainly how the labels were produced (by hand cold, pre-labelled and
corrected, or carried over from the Kaggle dataset) and whether you read every
row.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `reaction` | 69 | 34.7% |
| `hot_take` | 83 | 41.7% |
| `analysis` | 47 | 23.6% |
| **Total** | 199 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "We either colonize Mars, and further planets, or inevitably go
> extinct. What's to debate?"
>
> *Could have been:* `reaction` or `hot_take`
>
> *I chose, because:* `reaction`. TODO: your reason. It's a reply that
> dismisses the thread, but it also makes a confident claim with no support,
> which is the `hot_take` definition.

**2.**
> *The post:* "False. There is millions of tons of regolith on the moon. It
> all contains oxygen if you can process it correctly."
>
> *Could have been:* `reaction` or `analysis`
>
> *I chose, because:* `reaction`. TODO: your reason. It opens as a curt
> rebuttal to another commenter, but "regolith contains oxygen" is a
> checkable fact offered as support.

**3.**
> *The post:* "Massively improving upon Hubble space telescope, JWST has
> provided to be more useful in detecting intracluster light between
> galaxies"
>
> *Could have been:* `hot_take` or `analysis`
>
> *I chose, because:* `hot_take`. TODO: your reason. It names a specific
> capability (intracluster light), but the claim isn't backed up by anything
> that could be checked.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** 

device: cpu  (CPU)
torch:  2.14.1+cu130

⚠️  WRITE THIS DOWN. Your results depend on it.

Two people running this notebook on the same data with the same seed can
get different numbers, because different hardware does the arithmetic in
a slightly different order. That is not a bug and it is not one of you
being wrong — it is a real property of the thing you are building, and
unit 6 asks you to report it alongside your three seeds.

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Clean up the labels data file which had extra lines and a non-conforming presentation format
- *What came back:* A clean dataset that was acceptable for training
- *What I changed:* Nothing

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
