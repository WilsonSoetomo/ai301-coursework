# Procedure: how this tool grades a PR package

## Read order

1. Eval: read the Repo facts block first and note the PR template asks and AI-use policy. (Live: read the template and CONTRIBUTING.)
2. Read the Plan context: note the Scope, "Not in scope", Files list, deferrals, and the test plan with every failure mode it names.
3. Read the Candidate PR Diff and Commits. List every changed file and the purpose of every hunk.
4. Read the Test evidence.
5. Read the Title and Description last, as claims to check. The plan comes before the diff and the description comes last so the description cannot frame what you see.

## Evidence gathering

- Plan fidelity: write two lists, the plan's files and deliverables, and the diff's files and hunks. Mark each hunk as planned, noted, or extra. Mark each deliverable as present or missing.
- Claims: extract each factual sentence in the description (including "exactly as planned" and "docs updated") and look for it in the diff.
- Tests: write the repro steps and failure modes from the plan's test plan; for each, find a before/after in the evidence or mark it absent. Note which repo checks the repo-facts name and which appear with outcomes.
- Diff quality: scan every hunk and commit message for the debris tells in the evidence guide.
- Standards: tick each template ask and the AI policy against the description and diff.

## Check execution

Run the checks in rubric order. Grade each from the gathered lists, quoting one fact or line. If the evidence for a check is absent from the package, grade `fail` (never guess). Re-read the package only for a check whose list is incomplete.

## Verdict assembly

Apply the rubric's verdict rule: accept only if every required check passes; any required `fail` or `unclear` rejects. Preferred checks are reported but do not enter the verdict. In the summary, quote the first failing required check in rubric order as the deciding check. End with the JSON block.
