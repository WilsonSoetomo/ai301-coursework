# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every `###` section. Each is graded on its own; content placed under the wrong
heading is not graded.

---

## Your identity upstream

**GitHub username**

WilsonSoetomo

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5805076602

> Hi, I'd like to take this on too. I see a few others are already investigating — I'll still reproduce and report independently rather than piggyback on their comments. My plan: confirm that `verify_password` lets `passlib.exc.UnknownHashError` escape when the stored hash isn't a recognizable bcrypt hash, then look at wrapping the `pwd_context.verify` call in `core/security.py` to catch that failure (and check whether other malformed-hash shapes raise a different exception type) and return `False` instead, and drop the `xfail` marker on `test_verify_with_wrong_hash_format` (manifest H-05) once the fix lands.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5805079696

> **Environment:** macOS 26.6.2 (arm64), Python 3.11.15, repo at `WilsonSoetomo/pathreview-ai301-fa26-s1` (fork of this repo), commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`. This bug lives entirely in `core/security.py`, and `Settings()` (`core/config.py`) has a default for every field, so no `.env`/Docker stack is needed — just a venv with the relevant deps:
>
> ```
> $ python3.11 -m venv .venv && source .venv/bin/activate
> $ pip install "passlib[bcrypt]>=1.7.4" "bcrypt>=4.0.1,<5.0.0" "python-jose[cryptography]>=3.3.0" "pydantic[email]>=2.5.0" "pydantic-settings>=2.1.0"
> ```
> Installed: passlib 1.7.4, bcrypt 4.3.0, pydantic 2.13.5.
>
> **Steps and observed:**
>
> Control run (a valid bcrypt hash, to confirm `verify_password` works normally first):
> ```
> $ python3 -c "
> from core.security import verify_password, hash_password
> h = hash_password('password')
> print('control (valid hash):', verify_password('password', h))
> "
> control (valid hash): True
> ```
>
> The reported trigger — the malformed hash string the repo's own covering test uses:
> ```
> $ python3 -c "
> from core.security import verify_password
> verify_password('password', 'not_a_valid_bcrypt_hash')
> "
> Traceback (most recent call last):
>   ...
> passlib.exc.UnknownHashError: hash could not be identified
> ```
>
> Also ran the repo's own covering test directly:
> ```
> $ python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
> tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL
> ```
> `XFAIL` matches the `@pytest.mark.xfail(strict=True, reason="issue #72 (manifest H-05): ...")` marker already on that test — the bug is present exactly as the marker describes it, on the same commit the issue was opened against.
>
> **Expected:** `verify_password` returns `False` for a hash it cannot identify (fail closed), the same way it returns `False` for a merely wrong password.
> **Actual:** `passlib.exc.UnknownHashError` propagates out of `verify_password` uncaught, as shown above.
>
> One unrelated note for honesty: passlib 1.7.4 against bcrypt 4.3.0 also prints a harmless `(trapped) error reading bcrypt version` warning on every call (a known passlib/bcrypt version-detection quirk). It shows up on both the control and bug runs and has nothing to do with this issue.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One run. `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with category floors all met:
`categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3
wrong-target 4/4`. This matches the agreement line in the committed `eval-run.txt`.

**Package analysis**

`pkg-07` (source `processing/p5.js#7168`). Gold label: `accept`. My rubric's verdict:
`accept` (agree: yes).

Reasoning: the repo-facts block states the contribution policy is conditional — "assistive
AI use is allowed, and the contributor must understand and take responsibility for every
change" — which under `repo-ai-policy` requires an explicit disclosure statement in the
comment rather than failing the package outright. The candidate claim comment contains
exactly that: "Per the AI usage policy: I used an AI assistant to help me organize this
report; I ran and verified every step myself and I understand what I'm reporting," naming
the tool's role and taking ownership. The repro report itself reproduces the issue's exact
trigger (Japanese-first browser language, `let value = 0` in `setup()`) and shows both the
failing console output and an English-first control that renders the normal FES message —
satisfying `behavior-matches-issue` and `claim-honest` — and states its environment (p5.js
1.11.7, Chrome 139 on macOS) plus the version delta from the issue's reported 1.9.4/1.10.0.
Every required check passes, matching the gold `accept`.

**Check rationale**

From `rubric.md`, the `repo-ai-policy` row, quoted as currently written:

> If the repo's stated policy requires disclosing AI assistance in comments, the candidate
> comment must contain an explicit disclosure statement (naming the tool and roughly how it
> was used) — its absence fails this check even if nothing in the package explicitly says AI
> was used. If the policy instead requires comments be in the human's own words with no
> AI-generated text, judge the comment's voice against that. If the policy is silent on
> comments, or reaches only PR code, this check passes automatically.

Reasoning behind this form: a repo's AI-use policy can land in three different places — no
mention at all, a disclosure requirement, or a ban on AI-generated text — and each demands a
different kind of evidence from the same comment. Treating every package as AI-assisted by
default (per the check's own instruction) closes the gap where a comment simply never
mentions AI and that silence gets misread as "nothing to disclose." Branching on what the
policy actually asks for, instead of a single yes/no "was AI used," is what lets `pkg-05`
(a permissive-with-responsibility policy, no disclosure line needed) and `pkg-07` (a
conditional policy, disclosure line required and present) both grade correctly under one
check.

**Trade-offs**

This check's silent-default assumption is exactly what makes `pkg-04` grade correctly
instead of slipping through on a technicality: `pkg-04` (`junegunn/fzf#2021`) is a pure "+1"
comment with no environment, no steps, and no artifact, so it fails on `no-evidence` and
`steps-followable` regardless — but if `repo-ai-policy` instead required an affirmative
"the poster says AI was used" signal before checking for disclosure, a package like this
one that says nothing about tooling either way would pass that check by default, hiding
one required-check failure behind the others already failing. The trade-off is the reverse
direction: a genuinely human-written comment on a disclosure-required repo, with no AI
involved and so nothing to disclose, still reads as a policy failure under this check
because it never states its own no-AI status — the check has no way to distinguish "silent
because AI was used and undisclosed" from "silent because none was used," so it fails both
the same way. I re-ran `pkg-05` and `pkg-07` with `--only pkg-05,pkg-07` as canaries after
reading the rubric to confirm the conditional branch resolves both directions (no
disclosure required vs. disclosure required-and-present) without one flipping the other;
both still agreed, so nothing about how the other checks handle these two packages changed.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
