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

This project classifies posts from the public [r/Entrepreneur](https://www.reddit.com/r/Entrepreneur/) community. The local archive file [Entrepreneur_data.csv](../archive/Entrepreneur_data.csv) contains 999 posts with source URLs; 959 have non-empty bodies. I will use each post's full title and body, selecting 200 posts with bodies for the labelled dataset. The labels describe each post's main purpose: recounting a business experience, seeking advice for a specific situation, or inviting general discussion.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `experience_report`

**Definition:** Primarily recounts a business or entrepreneurial experience and what happened, rather than asking for recommendations about the writer's situation.

**Example 1:**
> [I lost nearly $8000 selling on Amazon FBA](https://www.reddit.com/r/Entrepreneur/comments/ayzlqu/i_lost_nearly_8000_selling_on_amazon_fba/) — recounts the writer's failed business attempt, costs, and lessons.

**Example 2:**
> [I changed careers and went from $40k to $100k in one year](https://www.reddit.com/r/Entrepreneur/comments/ge9swn/i_changed_careers_and_went_from_40k_to_100k_in/) — tells the writer's own career transition and outcome.

### `specific_advice`

**Definition:** Primarily offers or requests practical, actionable guidance for a concrete business task, problem, or decision.

**Example 1:**
> [I can't in good conscience have my manufacturing done in China anymore. What are my other options and how do I find suppliers?](https://www.reddit.com/r/Entrepreneur/comments/dfe3a6/i_cant_in_good_conscious_have_my_manufacturing/) — asks for sourcing options for the writer's manufacturing needs.

**Example 2:**
> [My patent was just granted. I'm looking to locate a manufacturer. Alibaba isn't working out. Help?](https://www.reddit.com/r/Entrepreneur/comments/65jmfz/my-patent-was-just-granted-im-looking-to-locate-a/) — asks for practical sourcing help for a specific product.

### `open_discussion`

**Definition:** Primarily invites broad opinions, discussion, or community experiences without focusing on practical guidance or one concrete business case.

**Example 1:**
> [Who here has an interesting small business that just ticks over with a profit each and every month? What are your stories?](https://www.reddit.com/r/Entrepreneur/comments/5okzhj/im_tired_of_reading_about_people_making_6_figures/) — invites examples from the community generally.

**Example 2:**
> [Is it just me or are you also sick of ads for online courses on how to make millions from making online courses?](https://www.reddit.com/r/Entrepreneur/comments/gnu4an/is_it_just_me_or_are_you_also_sick_of_ads_for/) — opens a broad discussion about a trend in the community.

### The hardest boundary

**Which two labels:** `experience_report` vs. `specific_advice`

**The decision rule I used every time:**
If most of the post recounts a specific business case or experience and its outcome, use `experience_report`, even if it includes lessons or ends with a question. If its main purpose is to give or request transferable steps, recommendations, or instructions for a business task, use `specific_advice`. Use `open_discussion` for broad opinions or questions that are not mainly a how-to or a case narrative.

### Boundary stress test

These eight synthetic posts were generated with AI to test the definitions; they are not community posts.

| Synthetic boundary post | Label | Why |
|---|---|---|
| "I opened a small online shop last year. It took six months to break even, and this month I finally paid myself. I wanted to share what changed." | `experience_report` | Recounts a completed business experience and outcome. |
| "After two years of freelancing, I hired my first employee last month. The first payroll run was nerve-racking, but we delivered our biggest project yet." | `experience_report` | Mainly tells the story of a recent business milestone. |
| "My first product launch is next month and my supplier has doubled the minimum order. How should I renegotiate without delaying launch?" | `specific_advice` | Requests practical guidance for a concrete business decision. |
| "A client is 60 days late paying a large invoice, and I have payroll due Friday. What steps would you take first?" | `specific_advice` | Requests actionable help for an immediate situation. |
| "What was the first business expense that made the biggest difference for you?" | `open_discussion` | Invites general experiences rather than instructions for a particular task. |
| "Do you think subscription pricing works better than one-time sales for small software businesses?" | `open_discussion` | Asks for broad opinions about a business model. |
| "I built a prototype and showed it to 12 potential customers. Eight said they might buy, but none would pre-order. Is that enough validation?" | `specific_advice` | The post asks for a judgment on a concrete decision; the numbers are context for that request. |
| "Our first location took 14 months to become profitable. Has anyone else found that expansion takes longer than expected?" | `experience_report` | The main purpose is a specific business case and outcome; the question invites comparison. |

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** The local [Entrepreneur_data.csv](../archive/Entrepreneur_data.csv) file, a collection of posts from [r/Entrepreneur](https://www.reddit.com/r/Entrepreneur/). It has 999 records with Reddit source URLs; 959 include a non-empty body and 40 are title-only. I selected 200 unique posts with non-empty bodies and kept each complete title and body in `labels.csv`; the source URL is recorded in each row's note. The archive is beside the `TakeMeter` folder.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `experience_report` | 93 | 46.5% |
| `specific_advice` | 67 | 33.5% |
| `open_discussion` | 40 | 20.0% |
| **Total** | **200** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "I own a 2.7M follower Instagram 'niche' account and just received a buyout offer for $120,000 USD. I'm not ready to give up my account... Thoughts?"
>
> *Could have been:* `experience_report` or `specific_advice`
>
> *I chose, because:* `specific_advice` — the numbers are context, but the post's purpose is to get thoughts on a live decision about the offer.

**2.**
> *The post:* "I've been having a huge drought when it comes to hiring employees. My company is short staffed... What things have helped you in these low unemployment times?"
>
> *Could have been:* `specific_advice` or `open_discussion`
>
> *I chose, because:* `specific_advice` — it asks for practical approaches to the writer's current staffing problem.

**3.**
> *The post:* "5 months ago I made a post about buying a coffee shop and everyone told me not to do it. I am back with an update."
>
> *Could have been:* `experience_report` or `specific_advice`
>
> *I chose, because:* `experience_report` — the main purpose is to report what happened after the earlier decision, even though it may include lessons or questions.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate `2e-5`, batch size 16, max length 128 tokens, seed 42.

**Training device:** CPU (`torch 2.14.1+cpu`).

**Anything I changed from the defaults, and why:** Nothing; I used the notebook defaults.

**Split sizes:** train 139, validation 31, test 30. The test split has 14 `experience_report`, 10 `specific_advice`, and 6 `open_discussion` posts. The `open_discussion` test count is below 8, so its metrics may vary substantially.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Help compare the available archived datasets and choose one with enough long posts and real variation for this project.
- *What came back:* `Entrepreneur_data.csv` stood out with 999 posts, 959 non-empty bodies, varied post flairs, and Reddit source URLs. I selected 200 unique posts with full titles and bodies for `labels.csv`.
- *What I changed:* I switched the project from r/running to r/Entrepreneur and adapted the examples, label definitions, and decision rule to business posts. I selected 200 posts and assigned AI-assisted labels to the full set; the first 20 were not cold-labeled.

**Moment 2**

- *What I asked for:* Eight synthetic posts near the label boundaries, then a classification of each using the written rules; I also asked for first-pass labels on the collected archive sample.
- *What came back:* The examples exposed overlap between case narratives and practical how-to posts. 
- *What I changed:* I broadened `specific_advice` to include practical how-to guidance as well as advice requests, and documented the case-versus-guidance decision rule. 

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
| F1 — `experience_report` |  |  |  |
| F1 — `specific_advice` |  |  |  |
| F1 — `open_discussion` |  |  |  |

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
| `experience_report` |  |  |  |
| `specific_advice` |  |  |  |
| `open_discussion` |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real experience_report posts were called specific_advice and
     only 3 went the other way" is a direction, not just an error rate. -->



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
