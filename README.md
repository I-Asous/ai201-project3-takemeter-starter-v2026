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

TakeMeter sorts comments from [games@lemmy.world](https://lemmy.world/c/games),
a public general-gaming community, by how a comment makes its point, not by
whether it's right. It has four labels. `argued` backs a claim with something
checkable. `unbacked` states a claim with no specific support. `firsthand`
rests on the writer's own experience. `quip` is sarcasm or an insult with no
claim left once you remove the tone. It's a fine-tuned
`distilbert-base-uncased` trained on 208 labelled comments.

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

**Moment 1: stress-testing the labels**

- *What I asked for:* I gave Claude the four draft definitions and asked it to
  write eight posts that sit on the boundaries between them, then label them
  under the rules.
- *What came back:* Six of eight labelled cleanly. Two didn't. "Xbox has been
  losing money on hardware for years, everyone knows that" sounded like
  evidence, and the definitions didn't say whether a vague fact counts. A
  Bethesda prediction backed by "I've played every game since Morrowind" was
  caught between `firsthand` ("experience is the main content") and the rule
  that personal experience comes first.
- *What I changed:* The definitions were tightened. A fact only counts as
  support if it has a number, date, name, quote or link. `firsthand` now covers
  experience used as support for a claim, not only experience as the whole
  post. Both changes are in the taxonomy above and in `label_definitions.txt`.
  The weakness: the same model wrote the test posts and graded them, so this
  was a weak test.

**Moment 2: labelling the 260 comments**

- *What I asked for:* For Claude to collect comments through Lemmy's public API
  and label them against the definitions and decision rules.
- *What came back:* 229 labelled comments, plus 31 skipped as unclassifiable.
  It also turned up a gap the rules didn't cover: replies to a news thread
  whose only "fact" is the headline itself (hard case 2), and sarcastic posts
  quoting the thing they're mocking.
- *What I changed:* A new decision rule, *the headline isn't evidence*, was
  added to the taxonomy mid-labelling. `unbacked` came out at 44%, which broke
  criterion 3, so 21 `unbacked` rows were dropped at random to bring it to 80.

**Pre-labelling disclosure:** Every label in `labels.csv` was assigned by
Claude, not pre-labelled and then corrected by me. Claude also did most of
this project's other work: choosing the community, the reading notes, the
taxonomy, `criteria.md`, and running the training. Each row's note says so.
The 30 staff-set labels in `my_staff_labels.csv` were also assigned by Claude,
under the staff taxonomy and before the staff key was seen.
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

Baseline is `facebook/bart-large-mnli` zero-shot, prompted with the four
definitions in `label_definitions.txt` (copied from the taxonomy above). Both
columns are the same 32 held-out posts in `test_split.csv`.

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.375 | 0.375 | +0.000 |
| Macro F1 | 0.245 | 0.140 | -0.106 |
| F1 — `argued` | 0.182 | 0.000 | -0.182 |
| F1 — `firsthand` | 0.286 | 0.000 | -0.286 |
| F1 — `quip` | 0.000 | 0.000 | +0.000 |
| F1 — `unbacked` | 0.513 | 0.558 | +0.045 |

**What the definitions were worth.** The same baseline run with bare label
names instead of definitions (`baseline_results_bare.json`) scored 0.250
accuracy and 0.108 macro F1. It labelled nearly everything `argued`. The
definitions added +0.125 accuracy and +0.137 macro F1. The table above uses
the definitions run.

**What I predicted before I looked:**
No prediction was written down before the trained numbers came in, so there
isn't one to report. Writing one now, after seeing the results, wouldn't be a
prediction.

**What the gap actually means:**
The fine-tuning added nothing. On these 32 posts it did worse than the
baseline on every measure except `unbacked`. The two models tie on accuracy
only because 12 of the 32 posts are `unbacked`, and the trained model labels
31 of 32 posts `unbacked` (see the confusion matrix). It learned the majority
class, not the taxonomy, so its +0.045 on `unbacked` comes from that and is
not a real gain. The zero-shot baseline is weak too (macro F1 0.245, and it
never found a `quip`), but at least it tells `argued` and `firsthand` apart
from the rest some of the time. With 32 test posts, a single post moves
accuracy by about 0.03, so these gaps are rough. Still, a model that predicts
one class for almost every post fails here whatever the sample size.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

All three seeds ran in one execution of section 6, on `mps` (Apple Silicon
GPU), torch 2.14.1, default settings (3 epochs, lr 2e-5, batch 16, max length
128). The numbers are in `results_three_seeds_before.json`.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.60 every seed | 0.375 | 0.375 | 0.375 | MISSED |
| 2. Per-label F1 | ≥ 0.50 (`firsthand` ≥ 0.35) every seed | argued 0.000 · unbacked 0.558 · firsthand 0.000 · quip 0.000 | argued 0.000 · unbacked 0.545 · firsthand 0.000 · quip 0.000 | argued 0.000 · unbacked 0.545 · firsthand 0.000 · quip 0.000 | MISSED |
| 3. Label counts | smallest ≥ 30, largest ≤ 80 | 32 / 80 (dataset, same for all seeds) | — | — | MET |
| 4. Staff agreement | ≥ 24/30 | 30/30 (labelling check, same for all seeds) | — | — | MET |
| 5. Confidence gap (top third − bottom third) | ≥ 0.20 every seed | 0.60 − 0.20 = 0.40 | 0.60 − 0.30 = 0.30 | 0.70 − 0.10 = 0.60 | MET on paper |

The spread table from section 6:

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.375 | 0.375 | 0.375 | 0.000 |
| Macro F1 | 0.140 | 0.136 | 0.136 | 0.003 |
| F1 — `argued` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `unbacked` | 0.558 | 0.545 | 0.545 | 0.013 |
| F1 — `firsthand` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `quip` | 0.000 | 0.000 | 0.000 | 0.000 |

**The zero spread is not stability.** The notebook calls 0.000 "stable enough
to trust", but every seed scores 0.375 because the model guesses `unbacked` on
almost every post. Each stratified test split has 12 `unbacked` posts out of
32, and 12/32 = 0.375. The three runs agree because they all failed the same
way, not because the measure is reliable. So three seeds can't tell me how much
a working model would move between splits.

Criterion 5 passes, but only in a narrow sense. All predictions are `unbacked`,
so "confidence" is just how strongly the model leans `unbacked`. The posts it
leans hardest on are more often truly `unbacked`, so that third scores higher.
The criterion was meant to show that confident guesses can be trusted across
labels, and with one label predicted it can't show that.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

Seed 42 (section 7 prints the first seed). Seeds 7 and 2024 are the same except
the one `quip` → `argued` becomes `quip` → `unbacked`.

| true \ predicted | argued | unbacked | firsthand | quip |
|---|---|---|---|---|
| **argued** | 0 | 8 | 0 | 0 |
| **unbacked** | 0 | 12 | 0 | 0 |
| **firsthand** | 0 | 5 | 0 | 0 |
| **quip** | 1 | 6 | 0 | 0 |

**My biggest off-diagonal number, and what it means:**
All 8 real `argued` posts were called `unbacked`, and no `unbacked` post went
the other way. The model never learned the argued/unbacked boundary (whether a
reason is specific); it falls back on `unbacked`, the majority label, for every
opinion post.


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.60 on every seed | MISSED | 0.375 on all three seeds, which is exactly the share of `unbacked` posts in each test split. |
| 2 | Per-label F1 | ≥ 0.50 (`firsthand` ≥ 0.35) on every seed | MISSED | `argued`, `firsthand` and `quip` are 0.000 on all three seeds; only `unbacked` clears 0.50 (0.545–0.558). |
| 3 | Label counts | smallest ≥ 30, largest ≤ 80 | MET | `labels.csv` has `firsthand` 32 and `unbacked` 80, out of 208. It met the target only because 21 `unbacked` rows were dropped (see The Dataset). |
| 4 | Staff agreement | ≥ 24/30 | MET | `agreement_results.json`: 30/30. |
| 5 | Confidence gap | top third ≥ 0.20 more accurate than bottom third, every seed | MET, but hollow | Gaps of 0.40, 0.30 and 0.60. With the model predicting one label, "confidence" just measures how strongly it leans `unbacked`, so the gap says nothing about whether its confident guesses can be trusted across labels. |

**Diagnoses**

**Criteria 1 and 2: the model underfit and fell back on the majority label.**
The cause is a model that can't reach the task from this much data and this
much training. It isn't a labelling problem. How I know:

- *The training loss barely moved.* It ended at 1.28–1.31 on all three seeds.
  A model that spreads its guess evenly over four labels scores ln 4 ≈ 1.386.
  With 144 training posts and batch size 16, three epochs is only 27 gradient
  steps. The model never got far from its starting point, and the cheapest
  loss reduction from there is to predict the most common label.
- *The agreement report rules out the obvious labelling cause.* The same
  labeller that produced `labels.csv` matched an outside key 30/30, including
  all seven posts staff marked HARD, and the staff rules hinge on the same
  question as mine (is the fact load-bearing?). If the labels were applied
  inconsistently, I'd expect it to show up there. It didn't, so pile (a), a
  rule applied loosely, is unlikely to be the main cause.
- *The failure doesn't follow any label boundary.* If one boundary had been
  labelled inconsistently, the errors would cluster on that pair. Instead
  every label collapses into `unbacked`, including `quip`, which shares little
  with it. That's the signature of a model that didn't learn, not of a
  confusing pair.

What the agreement check *can't* clear is pile (b): gaps in my own four-label
definitions, which no one else has ever applied. The `argued`/`unbacked` line
("is the reason *specific*?") is still the hardest boundary even if training
fixes the collapse, and with 37 `argued` training posts it's a genuinely hard
pair for this much data.

**Criterion 5 (met, but hollow).** No miss to diagnose, but the pass doesn't
show what the criterion was for, for the reason in the table.



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

**Who labelled these 30:** Claude, under the staff taxonomy, before seeing any
staff label. That's on purpose. Claude assigned every row in `labels.csv`, so
this check measures the labeller who actually made the training data. My
labels are in `my_staff_labels.csv`, the staff key is in
`data/staff_labels.csv`, and the comparison is in `agreement_results.json`.

**Calls I expected to be argued, written down before seeing the key** (all
eight matched staff):

- **sp_002, sp_005, sp_007, sp_010** (first-person training logs with
  numbers) → `analysis`. They read like personal experience, but each one
  argues from a checkable figure (6–8 s/mile, 2% + 2%, 90 s over six miles,
  within 10 s over 5 miles), and the conclusion rests on it. Rule 3 says
  immediacy and the personal angle don't outrank a load-bearing fact.
- **sp_014** ("down about 3% since the shoe rules changed, so obviously the
  shoes are the whole story") → `hot_take`. It's almost word for word Rule 1's
  own example: the 3% decorates a conclusion already reached, and nothing links
  the number to the cause.
- **sp_019** ("Nobody actually enjoys the long run…") → `hot_take`. It's a
  general claim about all runners with no support; Rule 2 puts that in
  `hot_take`, however personal the tone.
- **sp_021** ("Four minutes off. FOUR.") → `reaction`. It has a number, but the
  number is the event being reacted to, not evidence for a claim. Nothing is
  argued.
- **sp_029** (PB shoes bought in the wrong size) → `reaction`. It's an aside
  about the writer that just happened; there's no general claim to make it a
  `hot_take`.

**Disagreements**

None. With no disagreements there's nothing to sort into piles (a), (b) or (c).
The closest thing to adjudication is comparing which posts each side thought
were hard:

| Post | Staff flagged HARD | I flagged in advance | Both labels |
|---|---|---|---|
| sp_005 | yes | yes | `analysis` |
| sp_010 | yes | yes | `analysis` |
| sp_014 | yes | yes | `hot_take` |
| sp_019 | yes | yes | `hot_take` |
| sp_029 | yes | yes | `reaction` |
| sp_020 | yes | no | `hot_take` |
| sp_028 | yes | no | `reaction` |
| sp_002, sp_007 | no | yes | `analysis` |
| sp_021 | no (staff's note covers the same point) | yes | `reaction` |

The two I missed are the same kind of case. In sp_020 ("every injury thread is
the same injury") and sp_028 ("third DNF this year"), something *countable* is
mentioned but no count is used to argue anything. I labelled both correctly,
but I didn't see them as hard, so the "a number has to be load-bearing" check
was working without my noticing it.

**What the pattern in my disagreements tells me:**

A perfect score is weaker evidence than it looks, for three reasons:

1. **The set is in order.** Posts sp_001–010 are all `analysis`, 011–020 all
   `hot_take` and 021–030 all `reaction`. I labelled post by post from the
   text, but anyone reading in file order sees the blocks, and I can't rule out
   that the order helped on the borderline posts.
2. **The staff rules are tight and come with worked examples.** sp_014 is
   nearly word for word Rule 1's example. Three labels, four rules and
   examples on the hardest boundary make this an easier task than my own
   four-label taxonomy.
3. **It's the same labeller as `labels.csv`, not a second human.** It shows
   Claude applies a *written* rule consistently. It doesn't show those rules
   match what a person would mean, and there is still no human check on
   `labels.csv` (`cold_check.csv` is still unlabelled).

Still, it counts for something in Milestone 4. When a labeller who just
matched an outside key 30/30, including all seven posts staff marked HARD,
applies my own `argued`/`unbacked` rule, the model's collapse to `unbacked` is
unlikely to come mostly from inconsistent labelling (pile (a)). It points more
at too little data per label and a boundary ("is the reason *specific*?") that
DistilBERT can't learn from ~140 training posts. The one weakness in the labels
it can't clear is a gap in my own definitions, pile (b), because those were
never checked against anyone else.

---

## The Improvement

**What I changed:** One setting: `EPOCHS` from 3 to 10, set in section 2 of
the notebook. Everything else is unchanged: same data, same labels, same
learning rate (2e-5), batch size, max length, and the same three seeds (42, 7,
2024). Section 6 was re-run with `RUN_LABEL = "after"`, so the before-numbers
in `results_three_seeds_before.json` were kept, and the new ones are in
`results_three_seeds_after.json`. Same device: `mps`, torch 2.14.1.

**Which diagnosis pointed at it:** Criteria 1 and 2 were diagnosed as
underfitting: the training loss ended at 1.28–1.31, barely below the 1.386 of
a model that splits its guess evenly over four labels, after only 27 gradient
steps. The agreement report (30/30) made relabelling the less likely fix. So
the change targets the amount of training, not the labels.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.60 every seed | 0.406 | 0.500 | 0.500 | MISSED |
| 2. Per-label F1 | ≥ 0.50 (`firsthand` ≥ 0.35) every seed | argued 0.444 · unbacked 0.545 · firsthand 0.000 · quip 0.000 | argued 0.556 · unbacked 0.600 · firsthand 0.000 · quip 0.364 | argued 0.375 · unbacked 0.645 · firsthand 0.000 · quip 0.500 | MISSED |
| 3. Label counts | smallest ≥ 30, largest ≤ 80 | 32 / 80 (dataset, unchanged) | — | — | MET |
| 4. Staff agreement | ≥ 24/30 | 30/30 (unchanged) | — | — | MET |
| 5. Confidence gap | ≥ 0.20 every seed | 0.50 − 0.10 = 0.40 | 0.70 − 0.40 = 0.30 | 0.80 − 0.30 = 0.50 | MET |

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.406 | 0.500 | 0.500 | 0.094 |
| Macro F1 | 0.247 | 0.380 | 0.380 | 0.133 |
| F1 — `argued` | 0.444 | 0.556 | 0.375 | 0.181 |
| F1 — `unbacked` | 0.545 | 0.600 | 0.645 | 0.100 |
| F1 — `firsthand` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `quip` | 0.000 | 0.364 | 0.500 | 0.500 |

Confusion matrix after, seed 42:

| true \ predicted | argued | unbacked | firsthand | quip |
|---|---|---|---|---|
| **argued** | 4 | 4 | 0 | 0 |
| **unbacked** | 2 | 9 | 0 | 1 |
| **firsthand** | 2 | 3 | 0 | 0 |
| **quip** | 2 | 5 | 0 | 0 |

**Did it help, and how do I know:** Yes, partly, and not enough to meet a
missed criterion.

- *It fixed the collapse.* Before, the model predicted `unbacked` for 31 or 32
  of 32 posts on every seed. Now it predicts `argued`, `unbacked` and `quip` on
  every seed except seed 42, which still never predicts `quip`. Training loss
  ended at 0.91–0.99 instead of 1.28–1.31, so the model actually moved.
- *Every averaged measure went up on every seed.* Macro F1 went from 0.14 to
  0.25–0.38, and accuracy from 0.375 to 0.41–0.50. `argued` went from 0.000 to
  0.38–0.56 F1, so the model now learns part of the `argued`/`unbacked`
  boundary.
- *It still misses criteria 1 and 2 on every seed.* The best accuracy is 0.50,
  against a target of 0.60. `firsthand` is still 0.000 everywhere: it's never
  predicted correctly on any seed.
- *The spread is now real, and it's large.* Accuracy moves 0.094 and macro F1
  0.133 across seeds; `quip` F1 swings from 0.000 to 0.500. With the collapse
  gone, the seeds finally disagree. That shows how much of any single number
  here comes from the split: with 5–8 test posts per minority label, one post
  moves a label's F1 by 0.1–0.2.
- *Criterion 5 now means something.* The gaps (0.40, 0.30, 0.50) are measured
  on a model that predicts several labels, so a confident guess really is more
  often right. It was MET before only in the hollow sense described above.

---

## What's Still Broken

**Criterion 1 (accuracy ≥ 0.60), missed on every seed (0.41–0.50).** Next I'd
add data, not more training. Ten epochs on 144 posts already brought training
loss to about 0.9 while the test score stays near 0.5, so more epochs would
mostly memorise. The real ceiling is about 35–55 training examples per label.
I'd label another ~200 posts, aimed at the thin labels, and re-run all three
seeds. I stopped because that's a labelling job, not a setting, and it's out
of scope for this unit.

**Criterion 2 (per-label F1), missed on every seed, mainly from `firsthand`
at 0.000.** `firsthand` has 22 training posts and 5 test posts, and the model
reads them as `argued` or `unbacked`. That makes sense: a `firsthand` post
usually makes a claim too, and what sets it apart (the support is the writer's
own experience) is a subtle cue in 22 examples. The fix is again more
`firsthand` data, collected on purpose from threads where people describe
their own play. A cheaper test, if I had kept going, would be class-weighted
loss, to see whether the model can find `firsthand` at all when missing it
costs more. I stopped at one change so the before/after comparison stays
clean.

**The gap between what I meant my labels to capture and what the model
learned:**
I meant the labels to capture *how* a comment supports its point (a checkable
fact, the writer's own experience, nothing, or just tone). Even after the fix,
the model mostly learned *whether* a comment sounds like it's making a point:
in the after-matrix, 5 of 7 `quip` posts and 3 of 5 `firsthand` posts go to
`unbacked`, and no post is ever called `firsthand`.



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
