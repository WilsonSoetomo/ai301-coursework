# Procedure: how this skill grades a plan package

## Read order

Read the whole package once, front to back, before grading any check.
In this fixed order:

1. **Repo facts.** Note the contribution policy's AI-use line verbatim
   (required for `ai-disclosure`) and anything the bug-report template
   asks for.
2. **Issue.** Note the reported trigger and failure mode in one
   sentence each. This is context for the repro evidence, not itself a
   source of proof for any check.
3. **Thread highlights.** Note every comment that states an explicit
   maintainer diagnosis, a requested test, or a rejected approach.
   Write these down as a short list even if the list is empty — an
   empty list is itself the fact `thread-aware` needs.
4. **Repro evidence.** Note the steps, the artifacts, and every control
   run separately from the failing run. Control runs are read before
   the plan on purpose: judging the plan's diagnosis against a
   half-remembered repro is how a grader misses a contradiction.
5. **Candidate plan**, section by section (Diagnosis, Scope, Files,
   Approach, Test plan, and any stated Risk). Note the cause claimed,
   the exact sites touched, whether every touched site traces back to
   the diagnosis, the single chosen approach (or the absence of one),
   the test plan's concrete assertion (or its absence), and any named
   unknown.
6. **Candidate plan comment.** Note whether it engages any maintainer
   direction from step 3, and whether it contains an explicit AI-use
   disclosure sentence.

Live mode only: read `scope.md` before step 1 and refuse to proceed if
the issue is out of scope or the repo line is an unfilled placeholder;
read `voice-guide.md` after step 6 and list any rule the draft comment
breaks, separately from the checks below.

## Evidence gathering

Each rubric check pulls from exactly the notes above, never a re-read:

- `diagnosis-grounded`: the Diagnosis note (step 5) against every
  control-run note (step 4). Check control runs first — a control that
  contradicts the diagnosis decides this check regardless of how many
  artifacts otherwise look consistent with it.
- `scope-bounded`: the Files/Approach note (step 5) against the
  Diagnosis note (step 5). List every item the approach touches, mark
  each one "at the diagnosed site," "named deferral," or "unexplained
  addition." Any "unexplained addition" fails the check.
- `executable`: the Files/Approach note (step 5) alone. Flag any
  unresolved "or" between approaches, any site named as "somewhere" or
  "TBD," and any missing file/site list.
- `test-plan-decisive`: the Test plan note (step 5) against the Repro
  evidence steps/artifacts note (step 4). Confirm the test plan names
  an observable result tied to the fix, not only a suite run or a
  feeling.
- `thread-aware`: the thread-direction list (step 3) against the
  Approach and comment notes (steps 5-6). If the list from step 3 is
  empty, this check passes with no further reading.
- `ai-disclosure`: the repo-facts AI-use line (step 1) against the
  comment note (step 6).
- `unknowns-labeled`: the Risk/unknown note (step 5) against the
  confidence of the Diagnosis/Approach notes (step 5).

## Check execution

Grade in the table order above: `diagnosis-grounded`,
`scope-bounded`, `executable`, `test-plan-decisive`, `thread-aware`,
`ai-disclosure`, then `unknowns-labeled` last (it's `preferred` and
never changes the verdict, so it costs nothing to grade last). Grade
every check from the read-order notes; only re-open the original
package text to pull the exact quote for the output's `evidence`
field, never to re-decide a grade you already reached from the notes.

When the notes contain nothing relevant to a check (no thread
direction stated, no risk section written), that is a real finding,
not a gap in this procedure: grade the check by its own pass
condition, which each say explicitly what an absence means (silent
pass for `thread-aware` and `ai-disclosure` when their trigger
condition never arises; `unclear` for `diagnosis-grounded` when the
evidence is simply silent on the claimed cause). Never invent evidence
that is not in the notes, and never grade a check `pass` without
naming the specific note that decided it.

## Verdict assembly

Apply the rubric's verdict rule exactly: `accept` only if every
`required` check (`diagnosis-grounded`, `scope-bounded`, `executable`,
`test-plan-decisive`, `thread-aware`, `ai-disclosure`) graded `pass`.
Any one of them graded `fail` or `unclear` makes the verdict `reject`,
full stop — required checks do not average or offset each other.
`unknowns-labeled` is reported in the output but never flips the
verdict either way.

For the output JSON, the `evidence` field for each check is the one
fact or quote from that check's evidence-gathering step that actually
decided the grade (the contradicting control run, the unexplained
addition, the missing file list, the un-engaged thread comment, the
missing disclosure sentence) — the same fact a second grader would
need to reach the same verdict independently. When multiple required
checks fail, report all of them; the verdict is `reject` regardless of
how many, and the summary should not imply one failure "mattered
more" than the rest.
