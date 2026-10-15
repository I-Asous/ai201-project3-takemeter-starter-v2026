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

## 1. Overall accuracy is at least 0.60 on every seed

On the held-out test split, `accuracy` in `results.json` is at least 0.60 for
each of the three seeds run in unit 6.

**Why this target:** Always guessing `unbacked`, my biggest label, scores about
0.36, and random guessing over four labels scores 0.25. So 0.60 means the model
gets roughly two posts in three right, well clear of either shortcut. I didn't
go higher because about 140 training posts is very little, and the
`argued`/`unbacked` line depends on whether a reason is *specific*. A small
model will often blur that.

---

## 2. Every label has an F1 of at least 0.50, and `firsthand` at least 0.35, on every seed

In `results.json` → `per_label`, `f1` is at least 0.50 for `argued`,
`unbacked` and `quip`, and at least 0.35 for `firsthand`, for each of the three
seeds.

**Why this target:** This stops the model from doing well by ignoring a label.
`firsthand` gets a looser number because in my reading it was the smallest
label (about 14% of posts), so it will have only about 4–5 test examples, and
one miss moves its recall by 20–25 points. A matching target there would mostly
measure luck. 0.50 for the other three is below the overall target in #1
because the `argued` and `unbacked` scores will drag each other down where they
get confused.

---

## 3. The smallest label has at least 30 posts and the largest at most 80

In `labels.csv`, with 200 posts in total, every label has at least 30 rows and
none has more than 80 (40%).

**Why this target:** At the rate I saw while reading, collecting at random
would give `firsthand` about 28 posts. That's too few, so I'll have to look for
experience posts on purpose (hardware and troubleshooting threads). 30 is the
point where the test split still gets about 5 examples. Capping the largest
label at 40% keeps the always-guess-the-biggest-label baseline from creeping
toward the 0.60 in #1.

---

## 4. I match the staff labels on at least 24 of the 30 staff posts

Running `python agreement.py` on my labels for `data/staff_posts.csv` reports
agreement of at least 80% (24/30).

**Why this target:** This is the only check on whether I apply a written rule
consistently, and the model can only be as consistent as my 200 labels. The
staff set uses three labels with written decision rules, so 6 disagreements
leaves room for the honestly ambiguous posts. If I miss by more than that, my
labels are probably the problem, not the model. I didn't pick 90% because the
staff rules aren't mine, and the `analysis`/`hot_take` line in them has the
same "how specific is specific" problem as my `argued`/`unbacked` line.

---

## 5. The model's most confident third is at least 0.20 more accurate than its least confident third

In `results.json` → `confidence`, `accuracy_most_confident_third` minus
`accuracy_least_confident_third` is at least 0.20 on each of the three seeds.

**Why this target:** If confidence means anything, I can use it: trust the
confident predictions and hand-check the unsure ones, which should be mostly
the `argued`/`unbacked` borderline posts. With about 30 test posts each third
holds about 10, so 0.20 is two more correct posts out of ten. A smaller gap
would be one post either way, which is noise.

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
