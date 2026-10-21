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

This project classifies public posts from [r/running](https://www.reddit.com/r/running/), a broad running community with race reports, training discussions, and advice questions. I read 35 posts from the public [Top: past year feed](https://www.reddit.com/r/running/top/?t=year) while developing the taxonomy. The labels describe each post's main purpose: recounting completed running, asking for personal advice, or inviting general discussion. The full 200-post dataset still needs to be collected and labelled.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `experience_report`

**Definition:** Primarily recounts the writer's completed race, run, or training experience rather than asking for advice.

**Example 1:**
> [Baystate Half Marathon - first time going sub 1:30!](https://www.reddit.com/r/running/comments/1xasvjb/baystate_half_marathon_first_time_going_sub_130/) — a completed half-marathon report with a finishing time, splits, and training details.

**Example 2:**
> [Race Report - Loch Ness Marathon - PB, BQ, SUB 3](https://www.reddit.com/r/running/comments/1wuzdvc/race_report_loch_ness_marathon_pb_bq_sub_3/) — recounts the writer's completed marathon and preparation.

### `specific_advice`

**Definition:** Asks for actionable recommendations about the writer's own current or upcoming running situation.

**Example 1:**
> [How to increase running volume](https://www.reddit.com/r/running/comments/1tn57i8/how_to_increase_running_volume/) — gives current mileage and a target, then asks how to increase safely.

**Example 2:**
> [New to racing, anxiety?](https://www.reddit.com/r/running/comments/1qzzxej/new_to_racing_anxiety/) — describes anxiety about an upcoming first half and asks for advice.

### `open_discussion`

**Definition:** Invites general knowledge, opinions, or other runners' experiences rather than advice for the writer's own current decision.

**Example 1:**
> [How much did a smart watch and/or heart rate monitor change your training and running progress?](https://www.reddit.com/r/running/comments/1qh4l4r/how_much_did_a_smart_watch_andor_heart_rate/) — asks the community broadly about effects on training and goals.

**Example 2:**
> [Marathon known course vs unkown](https://www.reddit.com/r/running/comments/1rsyx52/marathon_known_course_vs_unkown/) — asks runners to compare experiences with familiar and unfamiliar courses.

### The hardest boundary

**Which two labels:** `specific_advice` vs. `open_discussion`

**The decision rule I used every time:**
First, if most of the post recounts the writer's completed run, race, or training cycle, use `experience_report`, even if it ends with a question. Otherwise, if the writer asks what to do about their own current or upcoming situation, use `specific_advice`; if they ask for general knowledge, opinions, or other people's stories, use `open_discussion`. Personal details alone do not make a post `specific_advice`.

### Boundary stress test

These eight synthetic posts were generated with AI to test the definitions; they are not community posts.

| Synthetic boundary post | Label | Why |
|---|---|---|
| "My first 10K is next week and I can comfortably run 5K. Should I start at my usual easy pace or aim faster?" | `specific_advice` | Asks what to do in the writer's upcoming race. |
| "What pace do you usually start at for a 10K?" | `open_discussion` | General question; no personal decision to advise on. |
| "I finished my first 10K in 58 minutes today. I started too fast, settled down after 4K, and felt strong at the end." | `experience_report` | Mainly recounts a completed race. |
| "I ran my first half last weekend and cramped at mile 10. What should I change about my fueling next time?" | `specific_advice` | A past event is context for a personal, actionable next-step question. |
| "I ran 18 miles for the first time on Sunday. The last three were rough, but I finished; has anyone else had that happen?" | `experience_report` | The body mainly recounts a completed run; the closing question does not change its main purpose. |
| "For a marathon build, how many long runs over 16 miles do you find useful?" | `open_discussion` | Seeks general practice and opinions. |
| "I'm at 25 miles a week and want to reach 35. Is adding a fourth running day a sensible next step?" | `specific_advice` | Requests a recommendation for the writer's stated plan. |
| "I ran my first 5K in 31 minutes yesterday and want to improve. What does everyone do to get faster?" | `specific_advice` | Despite asking broadly, it asks for help with the writer's own improvement goal. |

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public posts in [r/running](https://www.reddit.com/r/running/); the 35-post reading sample came from its [Top: past year feed](https://www.reddit.com/r/running/top/?t=year). The complete 200-post dataset has not yet been collected.

**How I labelled them:** I manually read 35 posts to develop these categories and examples. I have not yet labelled the full dataset.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |
| **Total** |  | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**2.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**3.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

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

- *What I asked for:* Help turn the differences I noticed in r/running posts into labels based on a post's main purpose, not a subjective quality judgment.
- *What came back:* A three-label draft distinguishing completed-experience reports, personal advice requests, and general discussion.
- *What I changed:* I clarified that completed-experience reports take precedence when most of the post recounts the event, even if it ends with a question.

**Moment 2**

- *What I asked for:* Eight synthetic posts near the label boundaries, then a classification of each using the written rules.
- *What came back:* The examples exposed overlap between personal advice requests and open discussion, and between race reports and posts ending in a question.
- *What I changed:* I made the decision order explicit: completed-experience report first, then actionable advice for the writer's own situation, then general discussion.

**Pre-labelling disclosure:** No real posts were pre-labelled by AI. AI helped draft the taxonomy and generated the eight synthetic stress-test examples; I read the real examples linked above.

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
