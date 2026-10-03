# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-build.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every `###` section. Each is graded on its own; content placed under the wrong
heading is not graded.

---

## Posted upstream

**GitHub username**

WilsonSoetomo

**Plan comment**

[TODO: permalink of the plan comment once posted on issue #72]

---

## Your branch

**Branch**

`fix/72-verify-password-unknown-hash` (https://github.com/WilsonSoetomo/pathreview-ai301-fa26-s1/tree/fix/72-verify-password-unknown-hash)

**Evidence**

Before (commit `f89c06f`, from my Unit 2 repro):
```
$ python3 -c "from core.security import verify_password; verify_password('password', 'not_a_valid_bcrypt_hash')"
passlib.exc.UnknownHashError: hash could not be identified
$ python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL
```

After (branch commit `e85c396`):
```
control True
'not_a_valid_bcrypt_hash' False
'$2b$12$short' False
'' False
$ python -m pytest tests/unit/test_security.py -q
25 passed, 2 warnings in 4.10s
```
Before the change the same file gave `24 passed, 1 xfailed`. (`ruff`, `black` and `mypy` were not installed in my venv, so I did not run them.)

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One run: 19/20. `agreement: 19/20 scored items  (bar: 18/20: PASS)`, which matches the agreement line in the committed `eval-run.txt`.

**Package analysis**

`pkg-02` (`sharkdp/bat#3844`, the `--terminal-width 1` capacity overflow). Gold label: `accept` (clear-accept). My rubric's verdict: `reject`, failing `scope-bounded` (agree: NO). It is the one disagreement in the run.

Why my rubric read it that way: the plan's Change section says to clamp the background fill with `saturating_sub` and "apply the same clamp to the sibling cursor advance at line 795 so the cursor never overshoots," plus "Audit the two other `cursor_max - cursor` sites." The diagnosis only pins the underflow at line 934, so my `scope-bounded` pass condition ("every in-scope item is a change at the site(s) the diagnosis names") reads the extra sites as work folded into the approach rather than deferred. The gold label treats the sibling site as part of the same defect class the issue itself names. I kept the rubric as is, since loosening it to admit this would also let the scope-creep packages through.

**Check rationale**

From `tools/plan-check/rubric.md`, the `scope-bounded` row, exactly as it reads now:

> scope-bounded | The plan's Scope and Files/Approach, read against the diagnosis: is the touched surface the one the diagnosis implicates, or wider? | Passes if every in-scope item is a change at the site(s) the diagnosis names, and anything beyond that site is explicitly named as deferred (not built now) with a reason, rather than folded into the approach as work to do. Fails if the approach includes a rewrite, migration, abstraction, new option, or "while I'm in there" change the diagnosis does not require, even when the core fix inside it is correct.

Why it reads that way: it separates "named as deferred with a reason" from "folded into the approach as work to do," because the scope-creep packages bury a bounded fix inside a redesign and the clean accepts defer such work explicitly. The cost is `pkg-02`, above, where an adjacent same-pattern site inside the one function reads as widening.

**Trade-offs**

The `scope-bounded` check gives up `pkg-02`: a plan that touches a sibling site of the same defect in the same function is graded as widened. I accept that miss. Nothing else changed in the run: the scope-creep category is 4/4 and every other clear-accept agrees, which I know from the per-package table in `eval-run.txt`. I did not re-run any canary with `--only`.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
