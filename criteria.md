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

On the held-out set, overall accuracy is at least **0.60 in each of the three
seeds**.



**Why this target:** With three labels, a uniform guess is right about one-third
of the time. Sixty percent is a meaningful improvement over that, while still
allowing for overlap between personal business advice requests and general
discussion prompts.

---

## 2.

The held-out **F1 is at least 0.50 for each of `experience_report`,
`specific_advice`, and `open_discussion` in each seed**.



**Why this target:** The labels have different cues: business experience
reports often describe outcomes, while practical guidance and open discussion
can both include recommendations or questions. A per-label floor prevents a good overall score from
hiding a label the model is mostly failing to recognize. I chose 0.50 rather
than a higher floor because each label's test support will be limited in a
200-post dataset.

---

## 3.

In the 200-post labelled dataset, **each label accounts for at least 20% and
no label accounts for more than 60%** of posts (at least 40 and at most 120
posts per label).



**Why this target:** The archive contains practical guides, personal business
stories, and open discussion. Requiring at least 40 examples per label leaves
useful training data for every distinction, while the 60% ceiling allows the
community's naturally uneven post mix.

---

## 4.

On a random sample of **30 posts independently labelled by a second reader,
at least 24 labels match mine (80% exact agreement)**.



**Why this target:** The main subjective boundary is whether a post's main
purpose is to recount a specific business case or to offer transferable
guidance from it. Agreement of 24 out of 30 would show that the decision rule
is clear enough to apply consistently, while leaving room for genuinely
ambiguous posts.

---

## 5.

For each seed, the model's accuracy on its **most-confident third** of held-out
posts is at least **0.70** and at least **0.10 higher** than its accuracy on
the least-confident third.



**Why this target:** A classifier is more useful if its strongest predictions
are dependable and its confidence separates clearer examples from boundary
cases. The notebook reports accuracy for both confidence thirds; a 0.10 gap
is large enough to be meaningful, and 0.70 on the most-confident third is
higher than the overall 0.60 target without demanding certainty from every
prediction.



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
