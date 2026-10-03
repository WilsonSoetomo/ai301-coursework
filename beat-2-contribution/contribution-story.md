# Contribution: verify_password raises UnknownHashError instead of returning False

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Wilson Soetomo
**GitHub Username:** WilsonSoetomo
**Issue:** https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72
**Pull Request:** [TODO: PR link once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: [one line on the tool work]**

[2-4 sentences: what you built (the scout, your strategy), what broke
or fell short, what changed because of it.]

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I ran my issue-select skill against the open issues on `codepath/pathreview-ai301-fa26-s1` and it accepted #72 on its live re-check on 2026-09-18: the maintainer is active (last human commit Aug 24, 2026), the repo is in use, the scope is one cohesive bug with a concrete diagnosis, and the labels include "good first issue" and "tier-1 (Starter difficulty)". It had no assignee or linked PR when I chose it. It fits me because it is a small, well-bounded security-adjacent fix in one function, with an existing `xfail` test that defines done.

[TODO: a sentence or two on what you hope to learn, and an honest read of where the scout fell short on the wild search.]

**Toolkit**

- Tool: issue-select (`tools/issue-select/`), run in live mode on the candidate issue
- What it produced: an `accept` verdict for #72 with per-check reasons (see `beat-1-sandbox/unit-1/selection.md`)
- Difficulties: [TODO]
- What changed: [TODO]

---

## Understanding the Issue

**Problem Description**

`verify_password` in `core/security.py` passes the stored hash straight to `pwd_context.verify`. When the stored value is not a recognizable bcrypt hash, passlib raises `UnknownHashError` and it escapes to the caller instead of the function returning `False`.

**Expected Behavior**

`verify_password` fails closed: an unidentifiable hash returns `False`, the same as a wrong password.

**Current Behavior**

`verify_password("password", "not_a_valid_bcrypt_hash")` raises `passlib.exc.UnknownHashError: hash could not be identified`.

**Affected Components**

`core/security.py` (`verify_password`) and `tests/unit/test_security.py` (`test_verify_with_wrong_hash_format`, currently `xfail(strict=True)`, manifest H-05).

**Toolkit**

- Tool: repro-check (`tools/repro-check/`), used to grade my claim and repro comments before posting
- What it produced: a ready-to-post verdict on both comments
- Difficulties: [TODO]
- What changed: [TODO]

---

## Reproduction Process

**Environment Setup**

macOS 26.6.2 (arm64), Python 3.11.15, fork `WilsonSoetomo/pathreview-ai301-fa26-s1` at commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`. The bug lives entirely in `core/security.py` and `Settings()` defaults every field, so no `.env` or Docker stack is needed:

```
python3.11 -m venv .venv && source .venv/bin/activate
pip install "passlib[bcrypt]>=1.7.4" "bcrypt>=4.0.1,<5.0.0" "python-jose[cryptography]>=3.3.0" "pydantic[email]>=2.5.0" "pydantic-settings>=2.1.0"
```

Installed: passlib 1.7.4, bcrypt 4.3.0, pydantic 2.13.5. Challenge: passlib 1.7.4 with bcrypt 4.3.0 prints a harmless `(trapped) error reading bcrypt version` warning on every call; it is unrelated and appears on control and bug runs alike.

**Steps to Reproduce**

1. Control: `hash_password('password')` then `verify_password('password', h)` returns `True`.
2. Run `verify_password('password', 'not_a_valid_bcrypt_hash')`.
3. Observed: `passlib.exc.UnknownHashError: hash could not be identified`.
4. `python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v` reports `XFAIL`, matching the marker on the test.

**Reproduction Evidence**

- **Commit showing reproduction:** [TODO: link to a commit in your fork, if you made one]
- **Screenshots/logs:** full commands and output in my repro comment: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5805079696
- **My findings:** the bug reproduces exactly as the issue and the `xfail` marker describe, on the commit the issue was opened against.

**Toolkit**

- Tool: repro-check (`tools/repro-check/`)
- What it produced: an accept on the repro comment (20/20 eval agreement on the skill itself)
- Difficulties: [TODO]
- What changed: [TODO]

---

## Solution Approach

**Analysis**

`core/security.py:37` returns `bool(pwd_context.verify(plain_password, hashed_password))` with no handling. `CryptContext.verify` raises `UnknownHashError` (and can raise `ValueError` for some malformed shapes) when it cannot identify the hash, so the failure propagates through the function's "True if password matches, False otherwise" contract.

**Proposed Solution**

Wrap the `pwd_context.verify` call in `verify_password` and return `False` when passlib cannot identify or parse the stored hash. Not changing: `hash_password`, the passlib/bcrypt version pins, or callers of `verify_password`.

**Implementation Plan**

1. In `core/security.py`, catch `UnknownHashError` (and check whether other malformed-hash inputs raise a different exception type) around `pwd_context.verify` and return `False`.
2. Remove the `xfail` marker from `test_verify_with_wrong_hash_format` in `tests/unit/test_security.py`.
3. Re-run the repro steps and the unit tests in `tests/unit/test_security.py`.

[TODO: record deviations from the plan in `beat-1-sandbox/unit-3/plan.md` as you build.]

**Toolkit**

- Tool: plan-check (`tools/plan-check/`), run on the plan and plan comment before posting
- What it produced: [TODO: its verdict on your plan]
- Difficulties: [TODO]
- What changed: [TODO]

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
