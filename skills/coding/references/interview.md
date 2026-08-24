# Interview question bank

Pick the few that actually matter for this task. Never dump the whole list.
Concrete beats open-ended: "веб, мобилка или Telegram-бот?" gets an answer;
"какие требования?" gets a shrug.

## Always, in one form or another

- **What problem is this solving?** Not "what should it do" — what goes wrong
  today without it.
- **Who uses it?** You alone, a team, strangers on the internet? That single
  answer decides auth, error handling, and polish.
- **What does done look like?** The moment you'd say "yes, that's it".
- **How long should this live?** Throwaway experiment, weekend tool, or
  something that has to survive a year.

## New thing from scratch

- Shape: web app / mobile / CLI / script / Telegram bot / browser extension /
  library / cron job?
- Where does it run: your laptop, a VPS you already have, a hosted platform,
  someone else's machine?
- Is there existing stack to match, or is this greenfield? Language preference?
- Does it need to persist anything between runs? If yes, how much would you
  lose if it vanished — file, SQLite, real DB?
- Does anyone else need to log in? (Usually the answer is no, and that removes
  half the work.)
- Is there an existing thing it should look/behave like?

## Feature in an existing codebase

- Which flow does this attach to — walk me to the entry point.
- Should it replace something that exists, or live alongside it?
- Who currently depends on the behaviour you're about to change?
- Is there an existing pattern in this repo I should copy rather than invent?
- Does this need to keep working for existing data / existing users?
- Anything in this area you already know is broken or dead?

## Bug fix

- What did you expect, what happened instead, and how do I reproduce it?
- When did it last work? Anything change around then?
- Is the fast fix acceptable, or do you want the root cause?
- Should this get a regression test?

## Refactor / cleanup

- What is the actual pain — reading it, changing it, or it being wrong?
- What must not change from the outside?
- Is there test coverage I can lean on, or do I need to build a safety net first?
- How far do you want this to go — this file, this module, or the pattern
  everywhere?

## Scope-cutting questions (the most valuable ones)

- Which of these do you want *now* and which are "eventually"?
- If we could only ship one part of this today, which part?
- Is it OK if this is ugly for now?
- Is it OK if this only handles the happy path?

## Contributions to offer alongside the questions

Only when genuinely relevant, max ~5, one or two sentences each:

- **Failure mode they haven't pictured** — concurrency, restarts, rate limits,
  someone typing a comma into a number field, the second user.
- **The boring standard approach** — if their plan diverges from how this is
  normally done, name the standard and ask whether the divergence is intended.
- **A cheaper path** — "there's a library that does exactly this", "we can fake
  this part until it matters", "20 lines get you 80% of it".
- **A real tradeoff** — two viable shapes and the consequence of each, with a
  recommendation. Not a menu; an opinion.
- **A decision that will be expensive to reverse** — data format, public API
  shape, anything other people will build on. Flag these early; everything else
  can be changed later.

Filler to avoid: restating the request back as a "question", asking about
things with obvious defaults, asking for permission to think.
