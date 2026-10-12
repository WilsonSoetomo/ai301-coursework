# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every `###` section. Each is graded on its own; content placed under the wrong
heading is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/139

**Branch**

`fix/72-verify-password-unknown-hash`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Two full runs, no component edits between them: 18/20, then 18/20 (the first with `--include-calibration`, the second the confirming run saved with `--save-run`). Between them I re-ran only `--only pkg-05,pkg-11` to read the evidence lines for the two misses. Last line of `eval-run.txt`: `agreement: 18/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

`pkg-11` (clear-accept; gold: `accept`). My rubric said `reject`, failing `claims-match-diff` with: "Description claims tests for 'escaped, unescaped, and double-backslash cases' and 'One function touched', but the diff has no unescaped-trim test and touches two functions (parse_gitignore_line plus the new trim_unescaped_trailing_spaces)." The check passes only if every claim is true of the diff, so a description that overstates the test cases and the function count fails. The gold note calls the package "terse but complete", so the instructor weighed the substantive fix and evidence over wording slips in the description.

**Check rationale**

From `tools/pr-precheck/rubric.md`, exactly as it reads now:

> | decisive-test-evidence | The Test evidence section read against the plan's test plan and the repro's own steps. | Passes if the evidence re-runs the plan's repro (every failure mode the test plan names) and shows an observable before and after, and any added test exercises the fixed path. Fails if it is "works locally" or "tests pass" with nothing observable, exercises only the unchanged path, or re-runs only part of the repro the test plan names. | required |

It reads that way because the not-tested packages (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`) all fail on the same gap: the plan's own repro is never re-run. The "every failure mode" clause is there for `calib-04`, where the evidence re-runs repro 1 and is silent on the `--exec` abort the plan named. I wrote it before any run and did not revise it.

**Trade-offs**

`decisive-test-evidence` gives up `pkg-05`: its evidence shows before/after for the issue script only, and the plan's second run (same-name same-key row plus warning) is asserted in prose, so the strict "every failure mode" clause rejects a package the gold labels accept. I accept that miss. Loosening the clause to "the primary repro" would likely flip `calib-04` and the partial-repro package `pkg-07` the other way, and I did not re-run canaries to test that. Nothing else changed between runs, which I know from the identical 18/20 and the same two misses (`pkg-05`, `pkg-11`) in both full runs.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
