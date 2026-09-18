# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every `###` section. Each is graded on its own; content placed under the
wrong heading is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

Summary of checks (live mode, re-verified 2026-09-18 against tools/issue-select/rubric.md, no activity on the issue since first pass):
- Maintainer is active — pass. Last default-branch commit by Aburke225 (human-authored) on Aug 24, 2026, within 90 days; satisfies the check's first branch on its own.
- Repo is currently used — pass. Not archived; last push Aug 24, 2026 (within 180 days). No release has ever been published, but the check's own wording states that "having no published release does not fail an otherwise active repository."
- Issue scope fits a newcomer — pass. Reproducible bug with a specific observed behavior (`UnknownHashError` escaping `verify_password` instead of returning `False`) and a concrete diagnosis (catch the exception, un-xfail test H-05); maintainer-authored, one cohesive change, no umbrella/tracking shape.
- Nobody else is already on it — pass. No assignee, no linked PR, no claim comments (zero comments on the thread).
- Beginner-friendliness is visible (preferred) — pass. Labels include "good first issue" and "tier-1 (Starter difficulty)".
- Contribution workflow is allowed (preferred) — pass. `docs/CONTRIBUTING.md` contains no AI-use ban or conditions.

```
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
  "checks": [
    {"name": "Maintainer is active", "grade": "pass", "evidence": "Last default-branch commit by Aburke225 (human-authored) on Aug 24, 2026, within 90 days"},
    {"name": "Repo is currently used", "grade": "pass", "evidence": "Not archived, last push Aug 24, 2026 (within 180 days); no release ever published, which the check does not penalize"},
    {"name": "Issue scope fits a newcomer", "grade": "pass", "evidence": "Reproducible bug with specific observed behavior and concrete diagnosis in core/security.py + one test file, maintainer-authored, 1-2hr estimate"},
    {"name": "Nobody else is already on it", "grade": "pass", "evidence": "No assignee, no linked PR, zero comments on the thread"},
    {"name": "Beginner-friendliness is visible", "grade": "pass", "evidence": "Labels include 'good first issue' and 'tier-1 (Starter difficulty)'"},
    {"name": "Contribution workflow is allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI-use ban or conditions"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

One run. `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with category floors all met:
`categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`. This
matches the agreement line in the committed `eval-run.txt`.

**Issue analysis**

`issue-06` (source `Itqan-community/quran-apps-directory#298`). Gold label: `accept`. My
rubric's verdict: `accept` (agree: yes).

Reasoning: the repo facts state "latest release: none published" and "last push to any
branch: 2026-07-31" (captured 2026-08-05, so 5 days old), with the last 5 default-branch
commits authored by a human maintainer (`abubakr-itqan`) as recently as 2026-07-14. Under
the "Repo is currently used" check, the repo is not archived and shows recent activity
through that last push within 180 days; because no release has ever been published, the
check's own escape clause applies ("having no published release does not fail an
otherwise active repository") rather than failing the issue for lacking one. The issue
itself ("Make 'Submit App' visible on desktop + functional on mobile menu") is a bounded
UI bug with concrete acceptance criteria and a named starting location
(`src/app/components/header/`), opened by a collaborator, unclaimed (assignees: none;
linked PRs: none). Every required check passes, matching the gold `accept`.

**Check rationale**

From `rubric.md`, the "Repo is currently used" row, quoted as currently written:

> Pass if the repo is not archived and shows recent activity through a last push within
> 180 days; if a release exists, it must be within 365 days, but having no published
> release does not fail an otherwise active repository

Reasoning behind this form: many actively-maintained repos — especially application and
UI projects rather than libraries — never cut a GitHub Release at all, so treating "no
release" as an automatic fail would conflate "doesn't use the Releases feature" with
"abandoned." The check keys primarily on push recency and the archived flag as the
liveness signal, and only holds release recency to a standard when a release actually
exists to judge.

**Trade-offs**

This check's escape clause is exactly what makes `issue-06` grade correctly: without it,
a strict "release must exist and be within 365 days" reading would reject a repo that is
visibly active (merged PRs and commits within the last 3 weeks of capture) purely because
it has never published a release, producing a false reject against the gold `accept`. The
trade-off is that the check leans entirely on commit/push activity as its liveness signal
once a repo has no release history, so it would be fooled by a repo that pushes trivial,
non-substantive commits (e.g. CI churn) on a schedule without real maintainer engagement
with issues — a case the "Maintainer is active" check is left to catch instead, via its
own human-authored-commit and response-time conditions.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. This fits my time and interests well: it's a small, well-scoped Python bug fix in
   `core/security.py` with a 1-2 hour estimate, which matches what I said in my fit
   profile — I want practice reading existing code and making small, low-risk fixes
   rather than taking on a broad redesign or unclear framework work.

2. The verdict correctly confirmed the mechanical signals: the repo is active, the issue
   is unclaimed, and it's a single bounded change rather than a tracking issue. What I
   weighed beyond that is that this is security-sensitive code (password hash
   verification) — the rubric doesn't grade for how carefully a fix needs to be tested,
   but I noticed the fix has to preserve "fail closed" behavior exactly, so I'll need to
   write a test for the malformed-hash case, not just silence the exception.

3. I expect low technical difficulty — it's one function and one test file, and the
   maintainer (course staff) already named the exact fix and the test to un-xfail. The
   main friction I anticipate is procedural rather than technical: following this
   course's claim and PR process (claiming in a comment, keeping CI green, understanding
   the xfail-contract convention referenced in the repo's docs) rather than the code
   change itself.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
