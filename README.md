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



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `argued`

**Definition:** Makes a claim and supports it with something specific a reader
could check: a link, a quote, a named product, feature, policy or person, a
number or date, or a concrete account of how the thing works. Vague support
("for years", "everyone knows") doesn't count.

**Example 1:**
> This behavior is unfortunately normal for epic, this is the same company that said their competitors giving users more information about what they are buying "irresponsible"
> https://www.tomshardware.com/tech-industry/artificial-intelligence/epic-boss-tim-sweeney-blasts-steam-for-putting-ai-tags-on-games-says-move-is-irresponsible-of-valve

**Example 2:**
> The beta launcher has a bunch of features disabled/absent compared to the main launcher, not the least being the shopping cart. Not surprising that it would load faster when it has less to do.

### `unbacked`

**Definition:** States an opinion, judgment or prediction as if it were true,
with no specific support. It might be right. It isn't argued.

**Example 1:**
> We have to face the fact that soon enough, any type of software question will be over.  AI will be able to generate whatever software/feature  you can think of instantly. Not sure what's gonna happen to dev jobs but be ready for some huge shit to hit the fan. We need to break free from capitalism before it kills us all.

**Example 2:**
> Copyright is going away in a way that only benefits AI companies. WE still get burned at the stake for downloading shit from limewire even if we're training our own AI models.

### `firsthand`

**Definition:** Reports what happened when the writer personally used, played,
bought or fixed something, either as the whole post or as the support for a
claim.

**Example 1:**
> My sx controller got stick drift a month after getting it. I have to wiggle it like a mad man to get it to stop or my character in game will slowly move out of cover.
>
> What I'm saying is, I'm not impressed with these controllers. Had better $30 controllers.

**Example 2:**
> I had Bluetooth issues with my computer, usb adapters were not working to pair controllers. I did manage to fix it by installing motherboard's driver manager, Gigabyte Control Center, and then installing their bluetooth drivers from there. So on the off chance you have a gigabyte motherboard that also has GCC, you might be able to try that to fix your bluetooth.

### `quip`

**Definition:** A sarcastic line, insult or bare emotional reaction. Strip out
the tone and no claim is left.

**Example 1:**
> Vibe-coding a program that manages and deletes massive amounts of files, what could possibly go wrong?

**Example 2:**
> Also handles payment information. Oh boy.

### The hardest boundary

**Which two labels:** `argued` and `unbacked`. Many posts give a reason. The
question is whether the reason is specific enough to count.

The post that sits between them:
> That actually all looks pretty reasonable for what they're trying to do.  There's very little novel there, they're mostly leveraging existing security features

**The decision rule I used every time:** A reason only makes a post `argued`
if it names something specific you could look up or test: a link, a named
product/feature/policy/person, a number or date, or a concrete account of how
the thing works. A reason that names nothing you could check ("existing
security features", "it's obvious", "everyone knows") counts as no support, so
the post is `unbacked`. The same goes for a factual-sounding claim with no
number, date, name, quote or link ("they've been losing money for years").
Under this rule the post above is `unbacked`.

**Two more rules for the other boundaries:**
- **Tone never decides.** If a hostile or sarcastic post contains a specific
  checkable fact that supports its point, it's `argued`. If it contains a claim
  with no support, it's `unbacked`. It's `quip` only when removing the tone
  leaves no claim at all.
- **The headline isn't evidence.** A fact that only repeats what the thread is
  about doesn't count as support, and neither does a quote of the thing being
  complained about. The fact has to add something. (Added during labelling;
  see hard case 2.)
- **Personal experience beats everything else.** If the post's main support
  is "I did / I had / I tried", it's `firsthand`, even if it names specific
  products.

**Not collected:** pure questions, and replies that make no sense without the
comment they answer.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Comments from
[games@lemmy.world](https://lemmy.world/c/games), a public general-gaming
community. I pulled 652 comments through Lemmy's public API (newest, top and
controversial, six pages of each). I kept comments between 60 and 1,500
characters and dropped deleted ones, ones that open with a quote of the parent
comment, and image-only ones. I sampled 260 of them, at most 10 per thread, from
168 threads. Every post is kept whole. The note column holds each comment's
source link.

**How I labelled them:** All 260 were labelled by Claude (an AI model), one at a
time, by reading each against the definitions and decision rules above. **No
human has checked these labels yet.** Nothing is marked `cold`, because a cold
pass has to be done by a person with no help. Of the 260, 31 were skipped as
not classifiable: pure questions, replies that only make sense in their thread,
image-only posts, and one offer to share pirated software. That left 229.
`unbacked` had 101 of them (44%), which breaks criterion 3, so I randomly
dropped 21 `unbacked` rows (seed 5). That leaves 208.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `unbacked` | 80 | 38% |
| `argued` | 53 | 25% |
| `quip` | 43 | 21% |
| `firsthand` | 32 | 15% |
| **Total** | 208 | 100% |

**Three hard cases**

**1.**
> *The post:* "Boy, it sure is a good thing that Sony charges a subscription fee for any and all network multiplayer traffic." (thread: PSN has been down for 14 hours)
>
> *Could have been:* `quip` or `argued`
>
> *I chose, because:* `argued`. It's pure sarcasm, but strip the tone and a
> checkable fact is left (PlayStation charges for online play), and that fact
> is the point: you're paying for a service that's down. It isn't just a
> restatement of the headline. Under "tone never decides" that makes it
> `argued`, even though it reads like a quip.

**2.**
> *The post:* "They will turn off the live service and make it work offline? I don't understand. Industry bootlickers told me it was impossible and would cost billions to implement, hurting small indie studios the most. Yet, Nintendo does it voluntarily with seemingly no difficulties." (thread: Pocket Camp going offline)
>
> *Could have been:* `argued` or `unbacked`
>
> *I chose, because:* `unbacked`. Its only fact is the thread's own
> headline. "Billions to implement" is attributed to nobody in particular. This
> case made me add a rule: **a fact that only repeats what the thread is about
> doesn't count as support.** Otherwise every reply to a news post would be
> `argued`.

**3.**
> *The post:* "You are nuts to sleep on Castlevania. It's $30! Much cheaper and it'll be free."
>
> *Could have been:* `argued` or `unbacked`
>
> *I chose, because:* `argued`. A price is a specific, checkable number, and
> it's the reason given. I'm not comfortable with it: a 15-word post becomes
> `argued` because it contains one number. This is where I expect the model,
> and a second labeller, to disagree with me.

**Two more that made me pause:**
- *"I love steam, and 90% of my game purchases or from their store… I think
  their market position should have some scrutiny."* Could be `firsthand` or
  `unbacked`. I chose `unbacked`: the personal fact doesn't support the claim
  about scrutiny, so experience isn't the support here.
- *Game recommendations* ("[Heaven's Vault](…) is a game about archaeology and
  translating a dead language…"). Could be `argued` or `firsthand`. I chose
  `argued`: they describe specific mechanics with a store link, and never say
  "I played". There are three of these in the file, and they're the weakest fit
  in the taxonomy.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128
tokens, seed 42.

**Device:** `mps` (Apple Silicon GPU), torch 2.14.1, Python 3.13.0. This is
also recorded in `results.json`. Training took about 15 seconds.

**Anything I changed from the defaults, and why:** No hyperparameters changed.
I did change the environment: the project's `.venv` had been built on Python
3.14, which `requirements.txt` doesn't support. Training crashed inside
`datasets`/`dill` (`TypeError: Pickler._batch_setitems() takes 2 positional
arguments but 3 were given`). I rebuilt `.venv` on Python 3.13 from the same
requirements, with the same package versions.

**Split sizes:** 144 train / 32 val / 32 test (70/15/15, stratified, seed 42).

| Label | Train | Val | Test |
|---|---|---|---|
| `unbacked` | 56 | 12 | 12 |
| `argued` | 37 | 8 | 8 |
| `quip` | 29 | 7 | 7 |
| `firsthand` | 22 | 5 | **5** |

`firsthand` (5) and `quip` (7) have fewer than 8 test examples, so their
scores will jump around between seeds next unit.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:** Every label in `labels.csv` was assigned by
Claude, not pre-labelled and then corrected by me. Each row's note says so.
`cold_check.csv` holds 20 of those posts with the label blank, for a human cold
pass. Until that pass is done and compared, there's no human check on these
labels.

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
