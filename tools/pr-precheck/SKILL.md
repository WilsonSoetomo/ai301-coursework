---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to submit?** A PR package is a candidate pull request (title, description, commits, unified diff, test evidence) read against the plan it claims to implement and the issue that plan belongs to. Grade one package per run. Never answer a different question, and never answer from gut feel: execute `procedure.md`, applying `rubric.md` to evidence located per `references/evidence-guide.md`.

## Inputs and modes

- **Live mode**: the student's own submission. Inputs: `plan.md` (with its deviation notes), the branch diff (run `git diff main...HEAD` from the working copy; three dots), the draft PR title and description, the captured test evidence, and the issue. Gather issue-side evidence from the real repo (the thread, `.github/PULL_REQUEST_TEMPLATE.md`, CONTRIBUTING, the stated AI policy). A house-chain student reads the house plan and house repro pack instead; the same checks apply.
- **Eval mode**: a package bundle is the whole world. Use only the bundle text. Fetch nothing, read nothing else. Run every check and the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Apply its house rules. Refuse to grade a PR outside the scoped repo. If the `Repo:` line is still a placeholder (`<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after the scope and hold the outgoing PR title and description against its rules. In the summary, name and quote each rule the draft breaks. The voice guide never changes the verdict on its own, because no rubric check reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule.
- `references/evidence-guide.md` maps where each evidence family lives in a package.
- `procedure.md` is executed as written: read order, gathering, check execution, verdict assembly.

If the procedure is silent on a step, report the gap in the summary and do not improvise around it. If `rubric.md` has no checks, or `procedure.md` has no steps, refuse to grade and say so.

## Verdict and output

The verdict is binary: `accept` (submit) or `reject` (hold). No third verdict, no score. Put reservations in check evidence lines. A short per-check summary may precede the block. End the reply with the fenced JSON block below, valid and last, with nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Grade no check without the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be ready, and a polished confident one can hide drift. Read the artifact against the plan, the issue, and the stated standards, never the formatting.
- **The rubric decides, not the run.** If a check passes by its stated condition but feels wrong, it still passes.
- **The procedure decides how.** Report its gaps; do not invent steps.
- **Unclear fails** unless the rubric's verdict rule says otherwise. A claim you cannot verify from the package is a failing one.
