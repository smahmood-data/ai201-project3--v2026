# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1.

<!-- Your criterion. It must name a number. -->

On the held-out test set, overall accuracy must be at least **0.65**.

**Why this target:**

With three labels, uninformed guessing is about 0.33. A 0.65 accuracy target
requires the classifier to learn useful differences between requests, supported
advice, and unsupported reactions while allowing for overlap in real posts.


---

## 2.

<!-- Your criterion. -->

Each of the three labels must have an F1 score of at least **0.55** on the
held-out test set, and macro F1 must be at least **0.60**.

**Why this target:**

The labels are intended to be useful as a set, so a strong score on
`detailed_request` cannot hide a model that almost never recognizes
`brief_reaction`. I chose a slightly lower per-label floor because the smallest
label will have fewer test examples and therefore a noisier F1.


---

## 3.

<!-- Your criterion. -->

The labelled dataset may not have any label representing more than **60%** of
the posts, and each label must represent at least **15%** of the posts.

**Why this target:**

These thresholds leave enough examples for every class to appear in training,
validation, and test splits. They also prevent the model from getting an easy
accuracy advantage by mostly predicting whichever label is most common.


---

## 4.

<!-- Your criterion. -->

On the 30-post staff agreement set, my labels must agree with the staff labels
on at least **75%** of posts.

**Why this target:**

Agreement tests whether I can apply written rules consistently, independently
of model performance. A 75% target is demanding enough to expose ambiguity but
allows for genuine judgment calls at the `reasoned_advice`/`brief_reaction`
boundary.


---

## 5.

<!-- Your criterion. -->

Among predictions with confidence at least **0.80**, at least **80%** must be
correct on the held-out test set, and this high-confidence accuracy must exceed
the accuracy of predictions below 0.80 by at least **0.10**.

**Why this target:**

Confidence is only useful if it separates safer automatic decisions from cases
that should be reviewed. The 0.80 threshold gives a clear operational cutoff,
while the 0.10 gap requires confidence to carry information rather than merely
track the model's overall accuracy.


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
