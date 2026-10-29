# pear-skills

Claude Code skills for Pear: two people each implement the same story with their own AI agent, then reconcile the two branches into one PR.

- `pear-prep`: build a shared story brief with both people and get approval from each.
- `pear-implement`: run one person's independent implementation from the brief.
- `pear-compare`: review both branches together and reconcile them into one branch.

## Install

Clone this repo, then link each skill into your user skills folder:

```sh
for s in skills/*/; do ln -s "$PWD/$s" ~/.claude/skills/"$(basename "$s")"; done
```

Run `git pull` to pick up updates.
