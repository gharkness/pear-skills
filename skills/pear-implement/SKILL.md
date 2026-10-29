---
name: pear-implement
description: Guide one human and their AI agent through an independent full-story implementation, collecting missing brief, candidate and base parameters one question at a time. Use in each of two separate sessions before pear-compare.
---

# Pear implement

Invocation: `/pear-implement`.

Optional shortcut: `/pear-implement <brief-path> <candidate-branch> --base <full-base-SHA>`. Accept partial arguments too; no parameter is required up front.

This session implements one complete candidate under one human's direction. The other human independently implements the same story in another session. Do not spawn the second human's replacement, split the story, inspect their candidate code, or reconcile implementations here.

## Resolve the handoff

Read `$ARGUMENTS` and repository guidance. Discover shared briefs through the repository's convention, default `pairing/<story-id>/brief.md`. Inspect only shared preparation metadata and this candidate's records; do not read the peer's delivery record or implementation to fill in setup details. Establish the brief path/revision, this human's identity and A/B label, candidate branch/workspace, and exact frozen base SHA. Reuse explicit arguments and unambiguous recorded values; validate them rather than asking the user to repeat them. Do not infer the human's identity from machine account names.

## One-question setup interview

Collect missing or conflicting parameters through a numbered conversation. Ask exactly one focused question per turn, then end the turn and wait for the answer. Do not list a questionnaire, combine independent decisions in one question, or ask the human to supply a complete command. This pacing also applies to setup clarifications and missing dependency choices.

Use this dependency order as needed, skipping resolved questions:

1. **Brief:** If exactly one relevant shared brief exists, load it. If several exist, ask which story to implement, showing concise choices. If none exists, ask for the agreed brief's location or suggest the pair run `pear-prep` first. Do not silently create requirements here.
2. **Participant:** Ask which named person is using this session if not established. Use the brief's mapping to resolve A/B. If that mapping is missing or ambiguous, ask the candidate-label question on the next turn. Never choose both people for the current session.
3. **Branch:** Use the candidate branch assigned in the brief when unambiguous. Otherwise ask for the candidate branch name, offering a repository-appropriate suggestion. If an argument conflicts with the brief, explain that conflict and ask one question to resolve it; do not silently override either source. Never use the peer's assigned branch.
4. **Base:** Prefer the exact frozen base recorded in the handoff or explicitly supplied by the user. Resolve refs to a full commit SHA. If unavailable or conflicting, ask which shared starting commit the pair agreed to. You may suggest an evidence-backed commit, but do not substitute current HEAD or latest main without agreement. Accept a ref as the answer and resolve it; the user need not type a full SHA.
5. **Workspace:** Inspect whether this candidate has a suitable existing workspace. Propose a distinct worktree using repo conventions when needed. Ask about an ambiguous/conflicting workspace separately; never overwrite unrelated work or use the peer's active workspace.

Number questions `Question 1 — <topic>`, `Question 2 — <topic>`, etc. After each answer, briefly state what was recorded, update the setup section in this candidate's delivery record beside the brief, and ask only the next unresolved question. Before the brief and candidate label are known, keep answers in the conversation; persist them once this candidate's record can be identified. Preserve existing delivery content and mark unfinished implementation fields `pending`. Persist brief path/revision, human, label, branch, workspace and frozen base so a resumed session does not repeat the interview.

Require evidence in the brief or the user's report that both humans approved the same brief revision and testing boundaries. If that evidence is absent, ask one question about the missing agreement before proceeding; do not claim the peer approved it. This implementation session's setup confirmation is not a replacement for joint preparation approval.

Once setup is resolved, present a compact summary: story/brief revision, human and candidate label, branch/workspace, frozen base and shared approval status. If an interview was needed, ask the next numbered question: **"Is this setup correct, and should I start your independent implementation?"** Wait for the answer. Corrections are resolved one question at a time and the summary is updated. If all values were already unambiguously supplied and the user explicitly requested implementation, show the summary and proceed without an extra confirmation.

Only this session's human confirms their implementation setup; do not collect the other candidate's setup approval here. Never create candidate branches or edit implementation code before the interview and any required setup confirmation are complete.

Resolve the base to a commit and inspect working tree state before implementation. For a fresh candidate create the agreed distinct branch/worktree from that base using repository conventions and existing permissions. If the branch/workspace already exists, inspect and resume it rather than resetting. Verify a resumed candidate descends from the base and disclose unrelated or updated-main changes. Never force checkout over local work.

## Matt Pocock integration

Discover and read Matt's installed `tdd` and `code-review`, plus `codebase-design` or `diagnosing-bugs` when relevant. Plugin entries may be namespaced; distinguish his review from Claude's bundled skill. Follow installed versions.

Do not nest Matt's user-only `implement` command. If a needed dependency is absent, identify it and link https://github.com/mattpocock/skills; offer a clearly labeled manual equivalent or installation instructions. Do not auto-install or claim it ran. Honor a previously accepted fallback.

## Independently implement

1. Implement the full story using Matt's `tdd` at the agreed public boundaries. Work one behavior/test/minimal implementation at a time, with observed failing and passing results. Expected results come from acceptance examples or independently worked cases, not a restatement of the implementation.
2. Keep the human directing design choices and reviewing small, meaningful increments. Ask at unresolved requirements or important design tradeoffs rather than silently delegating every choice to the model. Do not impose architecture differences merely to make candidates look different.
3. Preserve the shared brief. If requirements or expected behavior are ambiguous, pause affected work, present the question for both humans, and have the designated brief owner publish a revision. Both sessions must acknowledge that revision before affected work continues. Do not inspect the other implementation to resolve the ambiguity.
4. Commit candidate code locally under repo conventions. Run Matt's `code-review` against the frozen base and brief; ensure its committed-diff review includes all intended changes. Call out anything uncommitted/untracked that the review would omit. Address substantive findings, simplify at review, and run the required checks on the resulting candidate.
5. Write a delivery record beside the brief, default `candidate-A.md` or `candidate-B.md`. Include human, agent/model if known, brief revision, frozen base SHA, exact candidate SHA, branch/workspace, approach/tradeoffs, acceptance coverage, additional cases, actual check commands/results, review disposition, limitations, and sharing status. Write only this candidate's record. Commit the record if it must travel with the branch; record the final branch tip separately and make clear whether any commit after code verification changes code or only the delivery record. Never invent execution evidence.
6. Preserve this candidate branch. Push/share it when requested or already authorized; otherwise give the human a clear local handoff. Do not open a candidate PR by default. The outcome is a reviewable candidate, not a reconciled story.

End with exact refs, test status, unresolved questions and:

```text
/pear-compare <candidate-A-branch> <candidate-B-branch> --brief <brief-path>
```

Use actual names when both are known; otherwise label the unknown peer branch rather than guessing. Both refs and the shared brief must be accessible in the comparison session, including when humans use different machines.
