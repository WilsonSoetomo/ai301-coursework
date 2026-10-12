# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives.** Eval: the Plan context block (its Scope pair, "Not in scope", Files list, test plan, and any deviation note) against the Candidate PR's Diff and Commits. Live: `plan.md` and its Deviations section against `git diff main...HEAD`. The description's claims about fidelity live here too.

**What good looks like.** Every changed file and hunk is on the plan's Files list or covered by a deviation note, and every promised deliverable is in the diff. Drift runs both ways: extra work (a rewrite, a new option or flag, unrelated rewording, renames) and missing work (a planned docs update absent). A description saying "exactly as planned" or "docs updated" over a diff that says otherwise is drift, not a comms issue. Adjacent same-file work that the plan lists is not drift.

## Test evidence (harness category: not-tested)

**Where it lives.** Eval: the Candidate PR's Test evidence section, read against the Plan context's test plan and repro. Live: your captured command output and the repo's own checks (Makefile, CI).

**What good looks like.** The plan's own repro is re-run and an observable before and after is shown, for every failure mode the test plan names. An added test must exercise the fixed path. Weak: "tested locally", "works on my machine", "cargo test passes" with the repro never shown, a control run that exercises the path that never broke, or only one of two named failure modes re-run.

## Diff quality (harness category: unreviewable)

**Where it lives.** The Candidate PR's Diff and Commits (live: `git diff main...HEAD` and `git log main..HEAD`).

**What good looks like.** The fix is visible and nothing unrelated rides along. Debris tells: debug print or eprintln, commented-out code, dead functions (or `allow(dead_code)`), TODO on an import, re-indent or reprint churn, import reshuffles, and commit messages like "wip", "misc cleanups". Mechanical churn around a correct fix still fails.

## Standards and comms (harness category: standards-wall)

**Where it lives.** Eval: the Repo facts block (PR template asks, contribution policy, AI-use policy) against the Candidate PR's Description and Diff. Live: `.github/PULL_REQUEST_TEMPLATE.md`, CONTRIBUTING, the stated AI policy, against your draft.

**What good looks like.** Each ask the repo states is met with real content: "closes #N", checklist, a changelog or whatsnew entry in the diff, i18n or cheatsheet updates. If the AI policy requires disclosure, the description names the tool and extent. A repo with no AI policy, or one that does not cover PRs, needs none. An excellent fix does not rescue a missing required entry.
