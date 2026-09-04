# chill-pill

**Vibe coding without the headache.**

Three skills for Claude Code. No slash commands to memorise — you talk normally,
they trigger themselves.

The thing they're treating: you ask for a small tool, you get a cathedral with a
design system. You ask a question, you get an implementation of one of the four
things you might have meant. You add a feature, and the two features it made
obsolete stay in the repo forever. Eventually you're negotiating with the agent
instead of building.

## What's in it

### `coding`
Runs on any request to write, change, prototype or debug code.

1. **Says the rung out loud.** R0 probe → R1 proof of concept → R2 MVP → R3
   hardened. Defaults to the lowest rung that answers your request, never climbs
   on its own initiative, and checkpoints with you at the top of each rung. No
   database "because we'll need it later".
2. **Interviews you before designing — one question at a time.** Concrete and
   answerable, asked singly, waiting for each answer and following up on it. No
   walls of numbered questions. Then it contributes: what could go wrong, how
   this is normally done, a cheaper path, the decision that will be expensive to
   reverse.
3. **Writes a tiny spec** — in chat, never as a file in your repo.
4. **Builds the minimum that satisfies it.** No speculative flexibility, no
   unrequested UI polish, no abstraction for a single call site, no new
   dependency without a reason. Surgical while building.
5. **Tests where they mean something** (R2+), at the level of the problem a
   module solves, not at the level of getters.
6. **Reviews its own diff** (R2+) with Open Mercato's
   [`om-code-review`](https://github.com/open-mercato/skills/tree/main/skills/om-code-review)
   — validation gate, severity-ranked findings, breaking-change checklist,
   mechanical verdict. Blockers and majors get fixed before you see the work.
   Install it separately: `npx skills add open-mercato/skills --skill om-code-review`.
7. **Reflects at the end** — hands off to `cleanup`.

Everything durable — commit messages, comments, docs, the project model — is written in
English, whatever language you're speaking. Artifacts outlive the conversation.

### `project-model`
A code map tells you what exists; it can't tell you what matters. Read code
alone and the hundred lines that are the whole product look exactly like the
hundred lines someone vibe-coded one evening for fun — and the big ones look
most important of all. That's why agents defend dead code.

This skill builds a **high-level map of what the project is for**: per
capability, which problem it solves, for whom, and whether it's `core`,
`supporting`, `nice-to-have`, `legacy-keep` (with the reason it survives),
`dead`, or honestly `unknown`. It's built by reading the code **and interviewing
you** — because only you know that the export module is kept alive for one
client.

Models live outside your repo, at `~/.claude/project-models/<slug>/MODEL.md`
(override with `CHILL_PILL_MODEL_DIR`), so your colleagues never have to merge
them.

### `cleanup`
The closing reflection. Sorts what your change touched into three buckets:
orphans your change created (deleted without asking), pre-existing code your
change made pointless (proposed with grep evidence and a recommendation), and
unrelated rot (mentioned, not touched). Deletions go in their own commit.

Explicitly *not* a licence to tidy while building — that's the behaviour this
avoids. Cleanup is a phase, not a side effect.

## Install

```bash
claude plugin marketplace add nkvch/chill-pill
```

```bash
claude plugin install chill-pill@chill-pill
```

Then just talk to Claude. Skills fire on their own.

## Prior art

Assembled from things that already worked, plus the parts nobody had written:

- [Andrej Karpathy's notes on LLM coding](https://x.com/karpathy/status/2015883857489522876)
  and [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)
  — the simplicity rules and surgical-changes discipline.
- Anthropic's official `feature-dev` plugin — the interview-before-designing
  phase, and asking rather than assuming.
- [obra/superpowers](https://github.com/obra/superpowers) — classifying the work
  and saying the classification out loud, the one-way ratchet.
- [GSD](https://github.com/gsd-build/get-shit-done) — the idea of mapping a
  codebase up front, here deliberately made about *purpose* rather than code,
  and kept out of your repo.

New here: the four-rung ladder, the interview-built project model, the closing
obsolescence pass, and the rule that cleanup is a deliberate phase rather than
something that happens to your diff while you weren't looking.

## Licence

MIT.
