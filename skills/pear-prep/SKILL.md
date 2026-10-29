---
name: pear-prep
description: Prepare a shared story brief through one question per turn, building the document from answers and collecting separate final approval from both human candidates before independent implementation. Use before pear-implement and pear-compare.
---

# Pear prep

Invocation: `/pear-prep <story reference or description>`.

Two humans implement the same complete story in separate agent sessions. This command prepares their shared contract; it does not implement code or spawn agents to stand in for either human.

## Matt Pocock integration

Discover and read Matt Pocock's installed `grilling` and `domain-modeling` skills, using their actual namespaced names if installed as a plugin. These are the primitives behind his human-invoked `grill-with-docs`; do not nest that user-only command. Follow installed instructions rather than remembered versions.

Pear's interview pacing overrides any dependency instruction to ask a batch of questions: ask exactly one question per turn and wait for its answer. Retain Matt's questioning and domain-modeling discipline within that pacing.

If a dependency is missing, name it and link https://github.com/mattpocock/skills. Offer installation instructions or a clearly labeled manual preparation flow. Do not install automatically or claim the missing skill ran. If the user has already accepted a manual fallback, proceed without asking again.

## Prepare the contract together

1. Read the story, repository instructions, relevant glossary/ADRs, and existing behavior. Ask only for context that cannot be obtained from those sources. Inspect the working tree and preserve unrelated work.
2. Use `grilling` and `domain-modeling` to clarify the outcome, examples, failure behavior, scope, vocabulary and constraints, following the interview loop below. Both humans discuss questions before the operator supplies their answer. Do not manufacture agreement or resolve a disagreement just because the agent recommends an answer.
3. Agree on public testing boundaries: where behavior is exercised, what those tests establish, and what they miss. Keep internal architecture open unless it is an actual story constraint. Both agents should have room to find different solutions.
4. Create and incrementally update a concise brief using the repo's convention, defaulting to `pairing/<story-id>/brief.md`. Include:
   - Story reference, brief revision, two human participants and candidate A/B labels.
   - Desired outcome, acceptance examples with explicit expected results, key failure cases, constraints and non-goals.
   - Agreed public testing boundaries, required project checks, relevant standards/ADRs.
   - Shared brief owner, candidate branch names, and a base commit policy.
   - Separate approval entries for candidate A and candidate B, initially `pending`, tied to the brief revision.
5. Present the completed brief and collect the two final approvals using the approval sequence below. If either person requests changes, revise and return to approval of the updated document. Do not begin the handoff with only one approval.
6. After agreement, make the brief available to both sessions. Prefer committing agreed preparation/docs on a shared setup branch and freezing its full commit SHA as the common implementation base. Avoid a self-referential commit SHA: record the frozen SHA in the handoff after the brief commit, not inside that same commit. Alternatively use a separately shared brief and explicitly record its revision and an existing base SHA. Both sessions need the same brief and exact base; an inaccessible local path is not a handoff.

## One-question interview loop

Create the draft as soon as the story and document location are known; mark missing sections `unresolved`. Read an existing draft before resuming. Use available repository facts and answers already supplied rather than asking the humans to repeat them.

For each turn:

1. Choose the next unresolved decision whose prerequisites are settled. Cover participant identities, behavior, acceptance examples, non-goals, failure cases, testing boundaries and handoff details as needed; do not use a fixed questionnaire or ask questions the story already answers.
2. Present `Question 1 — <topic>`, then increment the number on each subsequent question, including clarifications and approvals. Ask one focused question, not several numbered or disguised subquestions. A short recommendation or a few choices are fine when useful. Do not show future questions or bundle independent decisions.
3. End the turn and wait for the human answer. Do not answer on their behalf or continue merely to maintain momentum.
4. After the answer, update the relevant brief section before asking the next question. State the recorded change briefly, then present only the next question. Distinguish human decisions from repository facts and agent proposals. If the answer is ambiguous, the next turn asks one clarification. If it resolves several topics voluntarily, record them all without asking those questions again.

Keep resolved decisions and unresolved questions in the document so another session can resume. Unknown answers remain unresolved; do not silently convert them into assumptions. Stop the interview when the brief is sufficient for both complete implementations, not after an arbitrary question count.

## Final approval sequence

Once all required decisions are resolved, show the completed brief (inline when short, otherwise a linked document with a concise review summary) and its current revision. Keep this review turn focused on the document and the first approval question.

1. Ask the next numbered question: **"Candidate A (<name>), do you approve brief revision <revision>, including the acceptance examples and testing boundaries, as the shared contract for both implementations?"** Offer `Approve` or `Request changes`, and end the turn.
2. Record A's explicit answer against that revision. If approved, ask the next numbered question with the same wording for **candidate B (<name>)**, and end the turn. One person's approval does not fill the other person's entry.
3. Record B's explicit answer. Once both approve the same revision, mark the brief ready and provide the handoff. Do not add another generic approval gate.

An operator may relay the named person's answer; label it operator-reported rather than claiming authenticated identity or access to the call. A vague "looks good" without an identifiable approver does not establish both approvals. If an answer explicitly supplies both named approvals, record both without requiring redundant turns.

If either person requests changes, record the request, ask any necessary follow-up one question at a time, revise the brief, and show the changed document. Clear both old approval entries and repeat the two approval questions for the new revision. On resume, retain valid approvals only when the document still matches the approved revision; any substantive change invalidates both. Cosmetic formatting that preserves all decisions need not create a new revision.

## Handoff

Provide two copyable launch prompts using actual agreed names and paths:

```text
/pear-implement <brief-path> <candidate-A-branch> --base <full-base-SHA>
/pear-implement <brief-path> <candidate-B-branch> --base <full-base-SHA>
```

Explain that each person can instead run `/pear-implement` with no arguments: it discovers recorded preparation values and asks for missing setup details one question at a time. Full launch prompts are optional shortcuts, not required syntax.

Each human starts their own session/workspace. Both implement the whole story. They may use different agents/models; do not guarantee independent reasoning from model choice alone. The skill must be available in both environments.

Explain that requirement changes must be agreed and shared with both sessions, while implementation decisions remain independent until comparison. Pushing/shared tracker edits follow the task's existing authorization; preparation is not permission to publish elsewhere.

End with the brief location, revision, frozen base SHA, outstanding questions, and launch prompts. Never replace this workflow with a shared implementation or split the story between agents.
