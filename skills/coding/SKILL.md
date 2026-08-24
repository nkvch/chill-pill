---
name: coding
description: "Use for ANY request to write, change, prototype, refactor or debug code — a new app, a new feature, a bug fix, a spike, \"try something\", \"добавь фичу\", \"поправь\", \"давай попробуем\", \"сделай приложение\", \"зарефактори\". Interviews the human before building, picks the right rung of the prototype→production ladder, builds the minimum that works, then reflects on what the change made obsolete. Do NOT skip it because the task looks small."
---

# Coding without losing your mind

Most of the pain of agentic coding comes from four moves: guessing instead of
asking, building a cathedral when a shed was asked for, climbing straight to
production when the human wanted to poke at an idea, and leaving behind a pile
of code nobody dares delete. This skill exists to stop all four.

**Run the phases in order.** Ceremony scales with the rung (§1); the phases
themselves do not disappear.

---

## 1. Say the rung out loud

Before anything else, classify the work and state the classification in one
sentence, so the human can override it:

| Rung | What it is | Output | Tests | Deps / infra |
|---|---|---|---|---|
| **R0 Probe** | "Can this even work?", "does this API return what we need?" | An **answer**, not code you keep | none | throwaway, label it |
| **R1 PoC** | Thinnest thing that shows the idea working end to end, happy path only | Running code, ugly is fine | none | none new unless unavoidable |
| **R2 MVP** | Something actually usable for its purpose | Working feature | core paths covered | only what the feature needs |
| **R3 Hardened** | Edge cases, failures, polish, docs | Production-shaped | unit + e2e where meaningful | as needed |

Rules:

- **Default to the lowest rung that answers the request.** "Сделай приложение,
  которое считает расходы в поездке" is R1, not R3.
- **The ratchet is one-way.** You may discover mid-task that the work is
  heavier than you thought — stop, say so, get agreement, step up. You never
  quietly step *down*, and you never climb *up* on your own initiative.
- **Never build a rung nobody asked for.** No database because "we'll need it
  later". No auth because "real apps have auth". No design system because the
  page looked plain. Those are R2/R3 concerns; at R1 they are noise.
- Each rung ends at a checkpoint: show what works, say what the next rung
  would add, and let the human decide whether to climb.

Explicit spec + explicit "just build it" → go straight to the rung the spec
describes and stop asking for permission at every step.

---

## 2. Interview before you build

Read `references/interview.md` for the question bank. Rules of engagement:

- **Ask before designing, not after implementing.** This is the single
  highest-value phase; do not skip it because the request sounds clear.
- Ask **concrete, answerable** questions — "веб, мобилка или Telegram-бот?",
  not "какие у тебя требования?". 3–7 questions is usually right; at R0 one or
  two.
- **Batch them and wait.** Do not start coding with questions outstanding.
- If the answer is "делай как знаешь" — give your recommendation and get an
  explicit yes to *that*, don't treat it as a blank cheque.

### Then push back and contribute

The human asked for a thing; you also know things. After the questions (or with
them), offer up to ~5 short notes, only where genuinely relevant:

- **What could go wrong** — the failure mode they probably haven't pictured.
- **How this is normally done** — the boring standard approach, if theirs
  diverges from it for no stated reason.
- **A cheaper path** — if 20 lines get 80% of the value, say so.
- **A real tradeoff** — two viable shapes, with the consequence of each.

Label them as suggestions, keep each to a sentence or two, and drop the ones
that are filler. Do not pad. Silence beats noise.

---

## 3. Write yourself a tiny spec

Before the first edit, post in chat (**never as a file in the user's repo**):

```
Goal:        one sentence, in terms of the problem, not the code
Done when:   2–4 checkable conditions
Out of scope: the things you are deliberately NOT doing
Plan:        1. step → verify: how you'll know
             2. step → verify: ...
Rung:        R1
```

Keep it short. It is a leash for you, not a deliverable for them. Track the
steps with TodoWrite for anything beyond two steps.

---

## 4. Build the smallest thing that satisfies the spec

- No feature beyond what was asked. No speculative flexibility, configurability
  or extension points.
- No new dependency unless it is doing real work you would otherwise hand-write
  badly. Say why when you add one.
- No abstraction for a single call site. No error handling for impossible
  states.
- No unrequested UI polish, CSS frameworks, animations, dark mode, or icon
  sets. Plain and working beats pretty and half-broken. Beauty is a separate,
  explicitly requested task.
- Build **top-down from the goal**, not bottom-up from a foundation. Don't lay
  a schema, a service layer and a config system before anything runs.
- If you wrote 200 lines and 50 would do, rewrite it before showing it.
- Match the surrounding code's style even when you'd do it differently.

**Surgical while building.** Don't "improve" adjacent code, reflow comments,
reorder imports or refactor things that are not in your way. Every changed line
should trace to the request or to your own mess. Cleanup is phase 6, and it is
deliberate, not a side effect.

---

## 5. Tests, when they mean something (R2+)

- Test at the level of **the problem a module solves**, not at the level of
  individual trivial functions. "Does the splitter settle debts correctly" —
  yes. "Does this getter return the field" — no.
- Prefer: write the check first, watch it fail, make it pass. For a bug: a test
  that reproduces it, then the fix.
- Tests are the executable half of the project model (see below): if the model
  says the module exists to solve problem X, there should be a test asserting X.
  When a new problem is added to the model, add its test.
- If the internals of a module change but the problem it solves does not, the
  tests should not need to change. If they do, one of the two was wrong — say so.
- R0/R1 get no tests. Don't argue for them there.

---

## 6. Closing reflection — what did this make pointless?

**Mandatory before you call the task done at R1+.** Load the `cleanup` skill
and run it. In short: your change often obsoletes older code — a superseded
implementation, a now-unreachable branch, a feature whose reason to exist just
disappeared. Obvious ones you remove yourself; the rest you list and propose.
Leaving them is how a codebase turns into a swamp nobody can reason about.

---

## 7. The project model

Reasoning about a codebase by reading code makes every 100 lines look equally
important. They are not. Before non-trivial work in an existing repo:

- Check for a project model (the `project-model` skill knows where they live).
- **If one exists, read it first** and use it to weigh decisions: what is
  load-bearing, what is nice-to-have, what is legacy kept alive for one reason,
  what is already dead.
- If none exists and the task is R2+ or touches unfamiliar areas, offer to
  build one — it is a one-time interview, and it pays for itself the first time
  you avoid maintaining something that should have been deleted.
- **After the work lands, update the model**: new capability added, old one
  retired, status changed. A stale model is worse than none.

---

## 8. Commits

- **Commit messages are always in English**, whatever language the conversation
  is in. Imperative mood, say what changed and why, no emoji.
- One coherent change per commit; don't bundle unrelated cleanup with a feature.
- Commit when asked to. If the repo's own conventions (CLAUDE.md, contributing
  docs) say otherwise, they win.

---

## Red flags

| Thought | Reality |
|---|---|
| "It's obvious what they want" | It is obvious to you because you picked one reading. Ask. |
| "I'll ask after I show them something" | You'll show them the wrong thing and burn the round trip. |
| "This is too small to interview" | Then it's one question, not zero. |
| "While I'm here I'll also…" | You are not here for that. Note it, move on. |
| "They'll want this configurable later" | They'll ask later. Later is cheaper than wrong now. |
| "It needs a proper architecture" | At R1 it needs to run. |
| "Tests would slow down this prototype" | Correct — that's why R0/R1 have none. Don't use it as an excuse at R2. |
| "The old code still works, leave it" | It works and it is now pointless. That is exactly what phase 6 is for. |
| "I'll write a plan file in their repo" | Their repo is not your notebook. Chat, or the project model directory. |
