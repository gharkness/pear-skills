# pear-skills

Claude Code skills for Pear: two people each implement the same story with their own AI agent, then reconcile the two branches into one PR.

- `pear-prep`: build a shared story brief with both people and get approval from each.
- `pear-implement`: run one person's independent implementation from the brief.
- `pear-compare`: review both branches together and reconcile them into one branch.

## How it works

```
 TOGETHER                     APART (in parallel)                TOGETHER
┌──────────────┐   handoff   ┌─────────────────────┐   refs   ┌───────────────┐
│ /pear-prep   │ ──────────▶ │ A: /pear-implement  │ ───────▶ │ /pear-compare │
│              │             │   → candidate A     │          │               │
│ one shared   │             ├─────────────────────┤          │ go through    │
│ brief + base │ ──────────▶ │ B: /pear-implement  │ ───────▶ │ the diffs →   │
│ commit       │             │   → candidate B     │          │ one team PR   │
└──────────────┘             └─────────────────────┘          └───────────────┘
 one session                  two separate sessions             one session
```

| Step | Partner needed? |
|---|---|
| `/pear-prep` | Yes, from the first question to the final approval |
| `/pear-implement` | No. Only if a requirement is unclear and the brief has to change |
| `/pear-compare` | Yes, for every decision item and the final plan |

In the joint steps, one person types (the "operator") and the other joins by call. Claude can't tell who is on the call, so every approval is recorded as operator-reported.

### 1. `/pear-prep <story>`: together

- Claude asks one numbered question per turn about the outcome, acceptance examples, failure cases, non-goals, and where tests sit. Talk it through, then the operator answers. Claude won't settle disagreements for you.
- Each answer updates `pairing/<story-id>/brief.md`. Internal design is left open on purpose, so the two implementations can differ.
- Each candidate approves the brief separately, A first, then B. Any later change to the brief clears both approvals.
- It freezes a shared base commit SHA and prints a `/pear-implement` launch command for each person.

### 2. `/pear-implement`: apart

- Each person works in their own session and worktree and builds the **whole** story. The story isn't split between them.
- Running it with no arguments works. It finds the brief and asks for anything missing, one question at a time.
- It builds test-first, reviews its own diff against the base, and writes `candidate-A.md` or `candidate-B.md` next to the brief.
- Talk to each other only if a requirement turns out to be unclear. The brief owner then publishes a new revision, and both sessions have to accept it before continuing. Design choices stay private until compare.

### 3. `/pear-compare <branch-A> <branch-B>`: together

- It diffs A against B directly and checks both against the base and the brief. The differences become decision items (`D01`, `D02`, …), tracked in `pairing/<story-id>/comparison.md`.
- It shows one item per turn with the tradeoff, the evidence, and a recommendation. Both people choose: A, B, combine, custom, omit both, or defer.
- After both approve the plan, it builds the result on a new output branch (A and B are left as they are), reviews it, and drafts one team PR.

The shared base commit keeps the A-vs-B diff fair: if one candidate started from a newer `main`, unrelated commits would show up as differences.

## Prerequisites

The skills use [Matt Pocock's skills](https://github.com/mattpocock/skills) (`grilling`, `domain-modeling`, `tdd`, `code-review`, and `pr`). If one is missing, the skill names it and offers a manual fallback.

## Install

Clone this repo, then link each skill into your user skills folder:

```sh
for s in skills/*/; do ln -s "$PWD/$s" ~/.claude/skills/"$(basename "$s")"; done
```

Run `git pull` to pick up updates.
