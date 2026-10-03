# Plan: issue #72, `verify_password` raises `UnknownHashError` instead of returning `False`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72
Repro: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5805079696

## Diagnosis

`verify_password` in `core/security.py` returns `bool(pwd_context.verify(plain_password, hashed_password))` with no handling. My repro shows the control (a hash from `hash_password`) returns `True`, while `verify_password('password', 'not_a_valid_bcrypt_hash')` raises `passlib.exc.UnknownHashError: hash could not be identified`, and the repo's own `test_verify_with_wrong_hash_format` reports `XFAIL` under its `xfail(strict=True)` marker. So the fault is only in the unguarded `verify` call: the correct-hash path works and only an unidentifiable stored hash escapes. `UnknownHashError` subclasses `ValueError`, and a truncated bcrypt hash (`$2b$12$short`) raises a plain `ValueError`, so both malformed shapes share one exception base.

## Scope

In scope: guard the `pwd_context.verify` call in `verify_password` so these failures return `False`; un-xfail the covering test.
Not in scope (deferred): `hash_password`, the passlib/bcrypt version pins (the harmless `(trapped) error reading bcrypt version` warning is unrelated), and callers of `verify_password`. They don't need to change for the fix.

## Files and approach

1. `core/security.py`: wrap the `pwd_context.verify` call in `try/except ValueError` and return `False` in the except branch (one chosen exception type; it covers `UnknownHashError` and the malformed-bcrypt `ValueError`).
2. `tests/unit/test_security.py`: remove the `@pytest.mark.xfail(strict=True, ...)` decorator from `test_verify_with_wrong_hash_format`.

## Test plan

- Re-run the repro: `verify_password('password', 'not_a_valid_bcrypt_hash')` goes from raising `UnknownHashError` to returning `False`; the control stays `True`; `$2b$12$short` and `''` also return `False`.
- `python -m pytest tests/unit/test_security.py`: `test_verify_with_wrong_hash_format` goes from XFAIL to PASS (strict xfail would otherwise error once fixed), with the other 24 tests unchanged.

## Risks / unknowns

I have not checked callers of `verify_password` (e.g. a login route) to see whether any relies on the exception; I expect none, since an exception there would be a 500.

## Plan comment (to post on the issue)

I'm taking #72 (my claim and repro are above). The cause is the unguarded `pwd_context.verify` call in `verify_password` (`core/security.py`). The plan is to catch `ValueError` there (`UnknownHashError` subclasses it, and a truncated bcrypt hash raises plain `ValueError`) and return `False`, then remove the `xfail` on `test_verify_with_wrong_hash_format`. Nothing else is touched; callers and the passlib/bcrypt pins are out of scope. Test: the repro goes from `UnknownHashError` to `False`, the valid-hash control stays `True`, and the covered test goes XFAIL to PASS. Disclosure: I used Claude Code to help draft this plan and the patch; I ran and verified every step myself and I understand the change.

## Deviations

Nothing material changed; the plan held. One refinement while building: the plan named `UnknownHashError`, and checking other malformed shapes showed `ValueError` is the right single catch, since `UnknownHashError` is a subclass of it. That was the "check other malformed-hash shapes" step in my claim comment, not a scope change.
