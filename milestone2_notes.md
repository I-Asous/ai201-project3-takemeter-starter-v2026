# Milestone 2 — Label design notes

The labels, definitions, examples and decision rules are in the README under
**Label Taxonomy**. `label_definitions.txt` holds the same definitions. This
file covers the stress test and the breakout prompt.

## Stress test: eight boundary posts

These are made-up posts, written to sit on the boundaries. The facts in them are
invented. **Try labelling them before you read the answers below.**

1. Xbox has been losing money on hardware for years, everyone knows that, so of
   course they're going PC-only.
2. lmao Tim Sweeney said Steam's AI tags were "irresponsible." Clown.
3. Bought the Nano yesterday and the d-pad is mushy. Don't buy it.
4. 8bitdo controllers are just better. The Ultimate has hall effect sticks and
   costs $50.
5. Ugh. Another launcher.
6. I've played every Bethesda game since Morrowind and they all shipped broken.
   Their next one will too.
7. Valve is the only company in gaming that actually respects its users.
8. A vibe-coded launcher that deletes files? Epic's own changelog says the
   cleanup tool was "mostly written by agents." Great.

### Answers, and what broke

| # | Label | Clean? | Why |
|---|---|---|---|
| 1 | `unbacked` | **No** | "Losing money on hardware for years" sounds like evidence but names no number, date or source. The first draft didn't say whether a vague factual-sounding claim counts as support. |
| 2 | `argued` | Yes | A checkable quote supports the point, so per *tone never decides* the "Clown" doesn't change the label. |
| 3 | `firsthand` | Yes | The support is the writer's own purchase. |
| 4 | `argued` | Yes | Named product, named feature, a price. |
| 5 | `quip` | Yes | Strip out the "Ugh" and there's no claim. |
| 6 | `firsthand` | **No** | The main content is a prediction, but the support is personal experience. The first draft's `firsthand` definition said experience must be "the main content", which conflicted with the *personal experience beats everything else* rule. |
| 7 | `unbacked` | Yes | Confident, no support. |
| 8 | `argued` | Yes | Sarcastic, but quotes a checkable source. |

**6 of 8 clean on the first draft.** Two changes made in response:

- **Vague facts don't count (from #1).** A factual-sounding claim is only
  support if it carries a number, date, name, quote or link. "For years",
  "everyone knows" and "studies show" aren't enough.
- **`firsthand` redefined (from #6).** It now covers posts where personal
  experience is the content *or* the support for a claim. That removes the
  conflict with the precedence rule.

After those changes, all 8 classify cleanly.

## For the breakout

Share two definitions (`argued` and `unbacked` are the interesting pair) and
this post. **Don't say which label you picked**:

> That actually all looks pretty reasonable for what they're trying to do.
> There's very little novel there, they're mostly leveraging existing security
> features

If you're working alone, the strongest case for each side:

- **For `argued`:** it gives a reason for its judgment, and the reason is about
  how the thing works ("leveraging existing security features"), not about
  feelings. Someone could dispute it on technical grounds.
- **For `unbacked`:** the reason names nothing you could check. Which
  features? Existing where? It's an impression dressed as an explanation, about
  as specific as "seems fine to me".

Both cases are reasonable, which is why the README's decision rule exists
(support has to name something you could look up). If your group splits on
this post, that's expected. Check whether they agree once they've heard the rule.
