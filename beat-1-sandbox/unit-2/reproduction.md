# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every `###` section. Each is graded on its own; content placed under the wrong
heading is not graded.

---

## Your identity upstream

**GitHub username**

WilsonSoetomo

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5805076602

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5805079696

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One run. `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with category floors all met:
`categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3
wrong-target 4/4`. This matches the agreement line in the committed `eval-run.txt`.

**Package analysis**

`pkg-07` (source `processing/p5.js#7168`). Gold label: `accept`. My rubric's verdict:
`accept` (agree: yes).

Reasoning: the repo-facts block states the contribution policy is conditional — "assistive
AI use is allowed, and the contributor must understand and take responsibility for every
change" — which under `repo-ai-policy` requires an explicit disclosure statement in the
comment rather than failing the package outright. The candidate claim comment contains
exactly that: "Per the AI usage policy: I used an AI assistant to help me organize this
report; I ran and verified every step myself and I understand what I'm reporting," naming
the tool's role and taking ownership. The repro report itself reproduces the issue's exact
trigger (Japanese-first browser language, `let value = 0` in `setup()`) and shows both the
failing console output and an English-first control that renders the normal FES message —
satisfying `behavior-matches-issue` and `claim-honest` — and states its environment (p5.js
1.11.7, Chrome 139 on macOS) plus the version delta from the issue's reported 1.9.4/1.10.0.
Every required check passes, matching the gold `accept`.

**Check rationale**

From `rubric.md`, the `repo-ai-policy` row, quoted as currently written:

> If the repo's stated policy requires disclosing AI assistance in comments, the candidate
> comment must contain an explicit disclosure statement (naming the tool and roughly how it
> was used) — its absence fails this check even if nothing in the package explicitly says AI
> was used. If the policy instead requires comments be in the human's own words with no
> AI-generated text, judge the comment's voice against that. If the policy is silent on
> comments, or reaches only PR code, this check passes automatically.

Reasoning behind this form: a repo's AI-use policy can land in three different places — no
mention at all, a disclosure requirement, or a ban on AI-generated text — and each demands a
different kind of evidence from the same comment. Treating every package as AI-assisted by
default (per the check's own instruction) closes the gap where a comment simply never
mentions AI and that silence gets misread as "nothing to disclose." Branching on what the
policy actually asks for, instead of a single yes/no "was AI used," is what lets `pkg-05`
(a permissive-with-responsibility policy, no disclosure line needed) and `pkg-07` (a
conditional policy, disclosure line required and present) both grade correctly under one
check.

**Trade-offs**

This check's silent-default assumption is exactly what makes `pkg-04` grade correctly
instead of slipping through on a technicality: `pkg-04` (`junegunn/fzf#2021`) is a pure "+1"
comment with no environment, no steps, and no artifact, so it fails on `no-evidence` and
`steps-followable` regardless — but if `repo-ai-policy` instead required an affirmative
"the poster says AI was used" signal before checking for disclosure, a package like this
one that says nothing about tooling either way would pass that check by default, hiding
one required-check failure behind the others already failing. The trade-off is the reverse
direction: a genuinely human-written comment on a disclosure-required repo, with no AI
involved and so nothing to disclose, still reads as a policy failure under this check
because it never states its own no-AI status — the check has no way to distinguish "silent
because AI was used and undisclosed" from "silent because none was used," so it fails both
the same way. I re-ran `pkg-05` and `pkg-07` with `--only pkg-05,pkg-07` as canaries after
reading the rubric to confirm the conditional branch resolves both directions (no
disclosure required vs. disclosure required-and-present) without one flipping the other;
both still agreed, so nothing about how the other checks handle these two packages changed.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
