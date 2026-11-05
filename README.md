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

For Milestone 1, I chose the public `r/boardgames` community. I will collect
public posts with enough variation in recommendation, rules, strategy, and
experience-based discussion to classify the kind of contribution each post
makes.

Candidate distinctions from reading the community:

- Some posts give specific constraints such as player count, play time,
  complexity, budget, or space; others ask for general recommendations.
- Some posts provide detailed personal context and examples of games already
  enjoyed; others provide only a short title or vague preference.
- Some replies explain their reasoning using mechanics, difficulty,
  interaction, or replayability; others only list game names.
- Some posts report firsthand experience, including what worked or failed;
  others rely on reviews, rumors, or assumptions.



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `detailed_request`

**Definition:** Primarily asks for help or recommendations and gives at least
two concrete constraints, examples, or requirements that another person could
use to tailor an answer.

**Example 1:** The post asking for a medium-weight game for three or four
players specifies player interaction, acceptable complexity, and dislike of
three-hour solitaire play. ([source](https://www.reddit.com/r/boardgames/comments/1w8mprb/))

**Example 2:** The travel-game request specifies two players, easy rules,
small size, and existing games the author likes and dislikes.
([source](https://www.reddit.com/r/boardgames/comments/1w8mprb/))

### `reasoned_advice`

**Definition:** Makes a recommendation, evaluation, or explanation and
supports it with specific mechanics, rules, firsthand outcomes, or a concrete
comparison.

**Example 1:** The reply comparing *Castles of Burgundy* and *Grand Austria
Hotel* distinguishes their component quality, rulebook demands, punishment,
theme, and solo modes. ([source](https://www.reddit.com/r/boardgames/comments/1wc9dhg/))

**Example 2:** The reply recommending *Sniper Elite* explains its complexity,
beginner map, difficulty settings, and which player should take the hider role.
([source](https://www.reddit.com/r/boardgames/comments/1wc9dhg/))

### `brief_reaction`

**Definition:** Gives a short opinion, reaction, or list with no supporting
mechanic, rule, comparison, or firsthand detail.

**Example 1:** The reply “Lost Cities, baby!” names a game but gives no reason
or supporting detail. ([source](https://www.reddit.com/r/boardgames/comments/1wc9dhg/))

**Example 2:** The reply “Bohnanza, Startups” supplies two recommendations
without explaining why they fit the request.
([source](https://www.reddit.com/r/boardgames/comments/1w8mprb/))

### The hardest boundary

**Which two labels:** `detailed_request` and `reasoned_advice`

**The decision rule I used every time:** If the post contains at least one
specific supporting reason—such as a mechanic, rule, comparison, or firsthand
result—and its primary purpose is to answer or evaluate, it is
`reasoned_advice`. If its primary purpose is to ask for help, it is
`detailed_request` even when the author gives reasons. If it only names an
opinion or recommendation without support, it is `brief_reaction`.
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

### Boundary stress test

I asked an AI tool to generate eight borderline examples using these
definitions. The rule above classified each one cleanly: four examples that
named a mechanic or outcome were `reasoned_advice`, and four that only named a
game or preference were `brief_reaction`. I tightened “support” to require a
specific mechanic, rule, comparison, or firsthand result rather than general
positive adjectives such as “great” or “fun.”



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**

I collected 200 public posts from `r/boardgames` using two public Reddit JSON
feed pages. The full post text was kept in `labels.csv`, including the title
where available.

**How I labelled them:**

I used the taxonomy above to label the posts with AI assistance, then reviewed
the generated labels against the definitions. The first 20 rows are marked for
cold review, but that review still needs to be completed independently by me;
they are not being represented as cold labels yet.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `detailed_request` | 34 | 17.0% |
| `reasoned_advice` | 72 | 36.0% |
| `brief_reaction` | 94 | 47.0% |
| **Total** | **200** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* A post asking whether a long, detailed collection-building post
> should be treated as a request for recommendations or as advice.
>
> *Could have been:* `detailed_request` or `reasoned_advice`
>
> *I chose, because:* I used `detailed_request` when the primary purpose was
> asking others for tailored suggestions, even if the author included reasons.

**2.**
> *The post:* A rules question that explains a specific game situation and
> asks how the rule should be applied.
>
> *Could have been:* `reasoned_advice` or `brief_reaction`
>
> *I chose, because:* The concrete rules and situation make it supported
> reasoning rather than an unsupported short reaction.

**3.**
> *The post:* A short post naming a game and asking whether others have tried
> it, with little additional detail.
>
> *Could have been:* `brief_reaction` or `detailed_request`
>
> *I chose, because:* It does not provide at least two concrete constraints or
> examples, so it remains `brief_reaction`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

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

- *What I asked for:* I asked an AI tool to apply my three boardgames labels to
  the collected public posts using the written definitions.
- *What came back:* It produced an initial label for each post, but the first
  pass showed that recommendation questions and supported advice were easy to
  confuse.
- *What I changed:* I checked the labels against the decision rule, kept the
  primary-purpose distinction between `detailed_request` and
  `reasoned_advice`, and marked the first 20 rows for a separate cold review.

**Moment 2**

- *What I asked for:* I asked the AI tool to inspect the training setup and run
  the notebook on the completed CSV.
- *What came back:* The notebook was configured with the starter taxonomy
  (`analysis`, `hot_take`, `reaction`) instead of my project labels, so the
  dataset check would have rejected the CSV.
- *What I changed:* I replaced the notebook label map with
  `detailed_request`, `reasoned_advice`, and `brief_reaction`, then reran the
  notebook and verified the generated results and test split.

**Pre-labelling disclosure:**

AI assistance was used to pre-label the 200-post batch. The labels were
reviewed against the written taxonomy, but the required 20-post independent
cold-label pass has not yet been completed; those rows are explicitly marked
for cold review in `labels.csv`.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.333 | 0.600 | +0.267 |
| Macro F1 | 0.167 | 0.400 | +0.234 |
| F1 — `brief_reaction` | 0.000 | 0.757 | +0.757 |
| F1 — `detailed_request` | 0.000 | 0.000 | +0.000 |
| F1 — `reasoned_advice` | 0.500 | 0.444 | -0.056 |

**What I predicted before I looked:**

I predict the trained model will beat the definitions-only baseline overall,
especially on `detailed_request`, because the fine-tuning data contains many
community-specific examples of recommendation questions and their boundaries.

**What the gap actually means:**

Fine-tuning substantially improved overall accuracy and recognition of
`brief_reaction`, but it did not learn `detailed_request` and slightly reduced
performance on `reasoned_advice`. The model learned the easiest short-reaction
distinction, while the request/advice boundary remains unresolved and needs
better-balanced, more consistently reviewed examples.



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
