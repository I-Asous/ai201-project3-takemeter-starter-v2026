# Milestone 1 — Community and reading notes

## The community

**games@lemmy.world** — https://lemmy.world/c/games

- General video-game discussion: news, hardware, platforms, industry. 51k
  subscribers, ~10k posts, ~226k comments (as of 2026-10-14).
- **Public:** readable without an account. The public API serves it too
  (`/api/v3/comment/list?community_name=games@lemmy.world`), so collecting 200
  posts is easy.
- **Not built around one release:** threads I read covered the Epic launcher,
  an Xbox controller, AI decompilation, and Hytale. Spoilers are rare.
- **Unit of text:** comments, not top-level posts. Most top-level posts here are
  bare links, and the actual discussion is in the comments.

(I tried r/nba first. Reddit blocks anonymous API access, so collecting from it
would be painful.)

## What I read

40 recent comments from 5 threads, sampled at random from the 89 newest
comments over 40 characters long. The numbers below (#n) refer to that sample.

## Candidate distinctions

**1. Some point at a source you could check. Most don't.**
About 5 of 40 link a source or name a specific, checkable fact.
- #16: calls Epic's behaviour "normal for epic", then links the Tom's Hardware
  article where Sweeney called Steam's AI tags "irresponsible".
- #30: "they just effectively killed their own cloud gaming platform", linked
  to the Xbox news post.
- Contrast #15: "soon enough, any type of software question will be over. AI
  will be able to generate whatever software/feature you can think of
  instantly." A confident prediction with nothing behind it.

**2. Some give a reason ("because…", how something works). Others just assert.**
About 8 of 40 explain why.
- #6: the beta launcher is faster because it "has a bunch of features
  disabled/absent… Not surprising that it would load faster when it has less
  to do."
- #39: on an immutable distro you can still recompile the kernel, "if EAC
  only accepts kernels signed by 'trusted partners' that freedom goes away."
- Contrast #10: "Copyright is going away in a way that only benefits AI
  companies." The claim is stated, but no reasoning comes with it.

**3. Some report the writer's own first-hand experience.**
About 5 of 40.
- #7: "My sx controller got stick drift a month after getting it."
- #29: fixed Bluetooth pairing by installing Gigabyte Control Center's drivers.
- #1: "Tried it and I'm surprised how much faster it is."

**4. Some are a one-line jab: sarcasm or an insult, with no claim to argue with.**
About 7 of 40, often under 15 words.
- #13: "Vibe-coding a program that manages and deletes massive amounts of
  files, what could possibly go wrong?"
- #23: "Also handles payment information. Oh boy."
- #12: "Fuck Epic and fuck Tim Sweeney."

## Things to watch for later

- **Mixed posts.** #5 states checkable facts ("Valve banned blockchain-based
  games… Valve regulated AI and required disclosure") and then insults Sweeney.
  #40 makes a real argument about immutable distros and then ends on a fart
  analogy. Hostile tone and real evidence can sit in the same post. My
  definitions will need a rule for which one decides the label.
- **Context-dependent replies.** #32 ("Lost in translation I guess…") and
  #35 make no sense outside their thread. Decide at collection time whether to
  skip these or keep them and label them on their own.
- **Questions.** #18 and #31 are clarifying questions ("Is that right?"). None
  of the distinctions above fit them. They may need their own label or a rule.
