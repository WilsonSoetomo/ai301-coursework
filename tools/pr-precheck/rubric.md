# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The diff's changed files and hunks read against the plan's Scope pair, Files list, and any deviation note. | Passes if every hunk is work the plan lists (or a deviation note covers) and every promised deliverable is in the diff, or the PR text openly says it is deferred. Fails if the diff adds behavior, options, renames, or files outside the plan with no note, or omits a planned deliverable without saying so. | required |
| claims-match-diff | The description's statements about what the PR does, read against the diff. | Passes if every claim ("exactly as planned", "docs updated", "no other changes") is true of the diff. Fails if any claim names something the diff does not contain or denies something it does contain. | required |
| decisive-test-evidence | The Test evidence section read against the plan's test plan and the repro's own steps. | Passes if the evidence re-runs the plan's repro (every failure mode the test plan names) and shows an observable before and after, and any added test exercises the fixed path. Fails if it is "works locally" or "tests pass" with nothing observable, exercises only the unchanged path, or re-runs only part of the repro the test plan names. | required |
| repo-checks-run | Test evidence read against the repo-facts block's stated checks (suite, lint, typecheck). | Passes if the suite or checks the repo names were run with the outcome shown, or the PR states which were not run and why. Fails if they were never run and nothing says so. | required |
| diff-clean | The unified diff and commit list. | Passes if every hunk serves the change. Fails if the diff carries debug prints, commented-out code, dead functions, TODO leftovers, formatting or import churn, or unrelated hunks. | required |
| template-honored | The description read against the repo-facts block's PR template asks and contribution policy. | Passes if each template ask the repo states is met with real content, or visibly addressed (for example a required changelog entry present in the diff). Fails if a stated required section or entry is ignored. | required |
| ai-disclosure | The description read against the repo-facts AI-use policy. Treat every package as AI-assisted. | Passes if the policy requires disclosure and the description discloses the tool and extent, or the policy is silent or does not cover PRs. Fails if disclosure is required and absent. | required |
| honest-shortfall | The plan's deferrals read against the description. | Passes if work the plan defers is stated as deferred in the PR text, or nothing was deferred. This check never fails a PR for doing less when it says so. Fails only when a shortfall is hidden. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. `unclear` on a required check counts as fail. Preferred checks never change the verdict. A PR that honestly discloses a bounded shortfall is not penalised for it: judge fidelity against the plan plus its stated deferrals, never against "everything".
