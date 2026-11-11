# Contribution: docs: add CONTRIBUTING.md quick-start for new contributors

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Wilson Soetomo
**GitHub Username:** WilsonSoetomo
**Issue:** https://github.com/flisk-sh/flisk/issues/1842
**Pull Request:** https://github.com/flisk-sh/flisk/pull/1846

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

**Unit 5: Built the issue-scout on my Unit 1 rubric, with three `gh search issues` queries and five ranking filters**

I installed the scout over my issue-select skill and wrote a search strategy traced to my fit profile: `good first issue` and `help wanted` Python queries plus a docs `good first issue` query, ranked by dropping assigned, stale (60+ days) and 10+ comment issues and then ordering by fewest comments. My first scan (2026-10-14) graded five and accepted only one, WLAN-Pi/wlanpi-fpms#199, because three of the five slots went to one repo created that same day and an empty-body issue tied at the top with zero comments. I added a one-result-per-repository ranking filter, since repo is a free search field, and left empty bodies to the rubric's scope check, which caught that issue correctly; I have not re-scanned to see the new filter work yet. Grading three docs-only issues from StellarCanary/Protocol-Canary showed one more gap that only the rubric can see: all three documented commands whose implementing issues were still open, so it rejected them.

**Unit 6: Promoted my voice guide, added Upstream rules, and wrote a repo memory file for flisk**

I merged my three voice-guide copies into one canonical guide at `~/.claude/voice-guard/voice-guide.md` (the pr-precheck copy was the newest, so it won), symlinked repro-check, plan-check and pr-precheck to it, and added Upstream rules for claims, questions and pings modeled on the claim I actually posted on #1842. I swapped repro-check and plan-check to the wild scopes and patched my scout's "Issue scope fits a newcomer" check to `scope-feasible`, because it had rejected three docs issues for open edges. What fell short: I claimed and shipped #1842 before the guard hook was wired, so the guard never checked my real claim, and I installed it late. My `CLAUDE.local.md` for flisk is 30 lines and records the one fact that kept coming up: `bun run test` has 4 failures on a clean macOS checkout of `main`.

**Unit 7: Wrote a three-check ship manifest for flisk and ran the ship-gate on my merged branch**

[TODO(Wilson): after you run the ship-gate, say what it printed on `docs/contributor-quick-start`.] Writing the manifest made me check which flisk checks really pass on macOS: `bun install`, `bun run typecheck` and `bun run lint` pass, but `bun run test` fails with the same 4 tests on `main` and on my branch (Docker/Tilt and the `/var` to `/private/var` symlink), so I moved it to the excluded table with the date and reason instead of deleting it. The gate came after my PR had already merged, so it checked a branch that maintainers had already accepted, which shows what it would have caught but couldn't change anything.

**Unit 8: Wrote a flisk playbook from real review times and ran the PR monitor on my merged PR**

I set my staleness threshold at 3 working days: CONTRIBUTING.md promises a reply within one working day, and five recent outside PRs all got a first maintainer touch within 2 hours. The monitor read #1846 as MERGED and drafted a merged follow-up. The draft had two errors I caught by reading the thread: it suggested closing #1842, which the bot had already closed, and it thanked the reviewer for checking the `demo` step, but the approval had no comment. I cut both and kept a two-line thanks in `my-reply.md`. [TODO(Wilson): say whether you posted it or decided not to, and why.]

---

## Why I Chose This Issue

[TODO(Wilson): in your own words, say where you found #1842 (Hacktoberfest label, the flisk "good first issue" list, or something else) and why you picked it over the scout's Unit 5 pick, wlanpi-fpms#199.] The issue was labeled `good first issue`, `difficulty: easy` and `rank: 8 kyu`, it listed the exact nine steps and the two candidate files, and it had no assignee or claim when I found it. It fits me because it is a bounded docs change where every claim on the page can be checked by running a command, and because the flisk CONTRIBUTING.md and AGENTS.md welcome AI-assisted contributors. [TODO(Wilson): one or two sentences on what you hoped to learn.]

An honest read of the scout: it never graded #1842. Its Unit 5 scan accepted one issue (wlanpi-fpms#199) and its old scope check rejected three docs issues on Protocol-Canary for open edges. My docs query (`--label documentation --label "good first issue"`, no language filter) matches #1842's labels, but the scan ran on 2026-10-14 and #1842 was opened on 2026-10-21, so it didn't exist yet. The scout ran once and never re-scanned. After the Unit 6 scope patch, I'd expect #1842 to pass `scope-feasible` with a "straightforward" difficulty note, but I didn't rerun it because the issue is now closed.

Claim comment: https://github.com/flisk-sh/flisk/issues/1842#issuecomment-6328546479

**Toolkit**

- Tool: issue-scout (`~/.claude/skills/issue-scout/`), my Unit 5 search strategy and rubric. I used it for the Unit 5 scan, not to pick #1842.
- What it produced: one accept (wlanpi-fpms#199) out of five in scan 1, and three scope rejects on docs issues in a direct grade (scan-notes.md, 2026-10-14).
- Difficulties: it ran once, a week before #1842 existed, so a fresh issue matching my docs query never reached it; and the strict scope check rejected docs issues with any open edge.
- What changed: I replaced "Issue scope fits a newcomer" with the `scope-feasible` row and added the difficulty note (scan-notes.md, 2026-11-11). Next time I'd re-run the scan right before choosing, so the issue I pick is one it graded.

---

## Understanding the Issue

**Problem Description**

flisk has no single short page that takes a new contributor from clone to first pull request. `CONTRIBUTING.md` is about 26 KB, and its Quick setup section covers Codespaces and the dev container, while the install, test, lint, build and sign-off loop is spread across `CONTRIBUTING.md`, `docs/contributing/01-first-change.md` and `docs/contributing/03-open-a-pr.md`.

**Expected Behavior**

A five-minute page with nine steps (prerequisites, clone and install, tests, lint, build, try the CLI, a demo, make a change with DCO sign-off, open a PR), in `CONTRIBUTING.md` or `docs/contributing/quick-start.md`.

**Current Behavior**

No quick start exists on `main` at `e7b877d4` (`git show main:docs/contributing/quick-start.md` fails), and the issue's own step 7, `node cli/bin/tdk.js demo --dry-run`, is not a dry run: it clones the demo, installs `flisk` globally and runs `flisk up`.

**Affected Components**

`CONTRIBUTING.md` (Quick setup section) and a new `docs/contributing/quick-start.md`. The `demo` behavior lives in `cli/src/cli.ts` (the `templateArg` branch), which this change documents but does not modify.

**Toolkit**

- Tool: [TODO(Wilson): how you used Claude Code (or anything else) to read the issue and find the `demo` handling in `cli/src/cli.ts`.]
- What it produced: the pointer to the `templateArg` branch in `cli/src/cli.ts`, confirmed by running `demo --dry-run`.
- Difficulties: [TODO(Wilson): what was confusing, or "none met".]
- What changed: the `demo` finding went into the PR as a stated deviation instead of copying the issue's step 7.

---

## Reproduction Process

**Environment Setup**

macOS arm64, Node 25.9.0, Bun 1.4.3, Docker installed but not running, no Tilt. Fork `WilsonSoetomo/flisk`, clean checkout of `main` at `e7b877d4`, then `bun install`.

**Steps to Reproduce**

1. `git show main:docs/contributing/quick-start.md` fails: the page doesn't exist, and Quick setup in `CONTRIBUTING.md` links to no quick start.
2. Run the issue's nine commands in order on `main`: `bun install`, `npm --prefix cli test`, `npm --prefix cli run lint`, `npm --prefix cli run build`, `node cli/bin/tdk.js --help`, `node cli/bin/tdk.js demo --dry-run`.
3. Observed: tests give 2557 passed, 4 failed, 97 skipped on macOS (Docker/Tilt and the `/var` symlink; CI on Linux is green). Lint and build pass. `demo --dry-run` ignores the flag, clones the demo into `~/.flisk/flisk-demo`, installs `flisk` globally, and `flisk up` exits 1.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:** the step table in the PR description, https://github.com/flisk-sh/flisk/pull/1846
- **My findings:** the gap is real, and step 7 as written would make a newcomer's "dry run" install software globally and start a stack.

**Toolkit**

- Tool: none for this phase. I ran the commands by hand. repro-check was still on its sandbox scope then.
- What it produced: the step-by-step table that became the PR's Evidence section.
- Difficulties: repro-check was built for bug reports, and a missing-docs issue has no "bug" to reproduce, so I didn't grade a repro draft.
- What changed: I swapped repro-check to the wild scope in Unit 6. For docs issues I'd frame the repro as "run every listed command and record what it does."

---

## Solution Approach

**Analysis**

The loop a newcomer needs is split across three documents, and Quick setup in `CONTRIBUTING.md` (around line 45) stops at the dev container. One of the issue's steps is wrong: in `cli/src/cli.ts`, the `templateArg` branch for `demo` ignores `--dry-run`, clones into `~/.flisk/flisk-demo`, installs `flisk` globally and runs `flisk up`. The test step also needs a caveat, because 4 tests need Docker/Tilt or a Linux path layout.

**Proposed Solution**

Add `docs/contributing/quick-start.md` with the nine steps, kept to a five-minute read, and one link line under Quick setup in `CONTRIBUTING.md`. Step 7 uses `node cli/bin/tdk.js doctor` (read-only) and states what `demo` really does. The test step tells readers to compare failures with CI on `main`. Not changing: any CLI code (a real `demo --dry-run` belongs in its own issue), the existing Quick setup content, or the numbered guides.

**Implementation Plan**

1. On a clean checkout of `main`, run every command from the issue in order and record the output.
2. Write `docs/contributing/quick-start.md`, linking to `01-first-change.md` and `03-open-a-pr.md` for depth.
3. Add the one-line link under Quick setup in `CONTRIBUTING.md`.
4. Check that every relative link resolves.
5. Commit with `git commit -s` on `docs/contributor-quick-start`; the PR says `Fixes #1842` and states the `demo --dry-run` deviation.

**Toolkit**

- Tool: plan-check (`~/.claude/skills/plan-check/`, wild scope), run on `plan.md` and `plan-comment.md` in my flisk clone.
- What it produced: [TODO(Wilson): paste its verdict after running step 6 of the Unit 7 tab.]
- Difficulties: I wrote the plan after the fix had merged, so plan-check graded a plan against a known-good outcome, which can't catch a wrong direction the way it would before building.
- What changed: [TODO(Wilson): what you revised in plan.md, or "nothing" if it accepted first time.]

---

## Testing Strategy

The page is the change, so the test is running every command on it, in order, on the built branch, and comparing with `main`:

- Before (`main` at `e7b877d4`): no quick start exists; the issue's step 7 (`demo --dry-run`) installs `flisk` globally and runs `flisk up` (exit 1 without Docker).
- After (branch at `87b8f86e`): `bun install` exit 0; `npm --prefix cli test` 2557 passed, 4 failed, 97 skipped, the same 4 failures as `main`; `npm --prefix cli run lint` exit 0 (3 warnings, 5 infos); `npm --prefix cli run build` exit 0 with a clean working tree; `node cli/bin/tdk.js --help` exit 0; `node cli/bin/tdk.js doctor` exit 1, reporting Docker/Tilt not ready and changing nothing.
- Links: `01-first-change.md`, `../../CONTRIBUTING.md` and `03-open-a-pr.md` from the new page, and the new link in `CONTRIBUTING.md`, all resolve to existing files.
- Repo checks in my ship manifest: `bun install`, `bun run typecheck`, `bun run lint`, all exit 0. On GitHub, CI on the PR showed 20 passing checks.

**Toolkit**

- Tool: my ship manifest (`ship-manifest.md` in the flisk clone) and the commands on the page, run in a fresh shell without my environment.
- What it produced: `test_evidence.md` with the before/after table and the manifest checks' output.
- Difficulties: `bun run test` fails 4 tests on macOS on `main` too, so "tests pass" was never true locally. I had to prove the failures weren't mine by running the suite on both `main` and the branch.
- What changed: `bun run test` moved to the manifest's excluded table with the date and the 4 test names, and the page itself now warns readers about it.

---

## Implementation Notes

**2026-10-21**

Found #1842 [TODO(Wilson): where]. Posted my claim with a plan (link the page from Quick setup, run every command on a clean checkout first), then "I'll take this", which the issue-claims bot listens for; it assigned me within seconds.

**2026-10-21**

Ran the nine steps on `main` at `e7b877d4`. Step 7's `demo --dry-run` turned out to be a real `demo`: it cloned into `~/.flisk/flisk-demo` and installed `flisk` globally. I traced it to the `templateArg` branch in `cli/src/cli.ts` and chose `doctor` for step 7 instead of changing CLI code, because a CLI change is out of scope for a docs issue. Wrote the page, added the link, signed off the commit (`git commit -s`, the DCO check needs it) and opened #1846, which was approved and merged 30 minutes later.

**2026-11-11**

Wrote the Unit 7 ship manifest. Ran the checks in a fresh shell (`env -i`) to match how the ship-gate runs them: install, typecheck and lint pass; `bun run test` fails 4 tests on both `main` and the branch, so it went to the excluded table. Wrote `plan.md`, `test_evidence.md` and `pr_draft.md` after the fact from the PR.

**2026-11-11**

Wrote the playbook from five recent merged outside PRs (#3972, #3951, #3943, #3934, #3928), all touched by a maintainer within 2 hours. Ran the PR monitor: it read MERGED and drafted a thank-you, which I trimmed because two of its claims didn't match the thread.

**Toolkit**

- Tool: Claude Code, used to draft the page (the PR says "drafted with Claude Code; I ran the commands and reviewed the page").
- What it produced: the first draft of `docs/contributing/quick-start.md`.
- Difficulties: [TODO(Wilson): what you had to correct in the draft.]
- What changed: [TODO(Wilson): what you'd change in how you prompt or check drafts.]

---

## Pull Request

**PR Link:** https://github.com/flisk-sh/flisk/pull/1846

**PR Description:** Adds `docs/contributing/quick-start.md`, a five-minute path from clone to first PR covering the nine steps in the issue, and links it from Quick setup in `CONTRIBUTING.md`. States one deviation: step 7's `demo --dry-run` isn't a dry run (it clones the demo, installs `flisk` globally and runs `flisk up`), so the page uses `doctor` and says what `demo` does, with an offer to open a separate issue. Says `Fixes #1842`. Checks: ran every command on the page on a clean checkout, verified relative links, and said that typecheck/lint/test weren't rerun after the edit because no code changed. Includes a step-by-step evidence table (including the 4 macOS-only test failures) and an AI-use note.

**Maintainer Feedback:**

- 2026-10-21: odykyi approved with no comments and merged as `94fa057` 30 minutes after I opened it; the bot closed #1842 and later announced it shipped in v1.3.205. Later comments asked for a Product Hunt upvote and offered a Founding Contributor badge, which are unrelated to the change.
- 2026-11-11: [TODO(Wilson): "posted a short thank-you (link)" or "decided not to reply because …"]

**Status:** Merged

**Toolkit**

- Tool: ship-gate (`~/.claude/ship-gate/check_gate.py --branch`), run by hand on `docs/contributor-quick-start` with my manifest, `plan.md`, `test_evidence.md` and `pr_draft.md`.
- What it produced: [TODO(Wilson): PASS, or the checks it named.]
- Difficulties: the ship-gate arrived after my PR merged, so it ran on a shipped branch. The repo-check half needed a fresh-shell command list, which surfaced the 4 macOS test failures.
- What changed: [TODO(Wilson): any rubric or manifest fix the gate forced.]

---

## Learnings and Reflections

**Technical Skills Gained**

[TODO(Wilson) — draft to rewrite:] Reading a CLI's flag handling (`cli/src/cli.ts`) to check what a documented command really does; DCO sign-off with `git commit -s`; using `.git/info/exclude` to keep personal files like `CLAUDE.local.md` and `ship-manifest.md` out of a PR; running a repo's checks in a clean shell (`env -i`) to separate "my environment" failures from "my change" failures; and reading a project's merged PRs to estimate how fast it reviews.

**Challenges Overcome**

[TODO(Wilson) — draft to rewrite:] The issue told me to document `demo --dry-run`, and running it showed it wasn't a dry run, so I had to decide between copying the issue, fixing the CLI, or documenting what's true. I chose the last option and stated the deviation in the PR. Locally, 4 tests always failed, and I proved they weren't mine by running the suite on `main`.

**What I'd Do Differently Next Time**

[TODO(Wilson) — draft to rewrite:] Set up the voice guard, plan-check and ship-gate before claiming, not after merging, so the tools gate the work instead of describing it afterward. Re-run the scout right before choosing, so the issue I pick is one it graded.

---

## Resources Used

- flisk `CONTRIBUTING.md` (Quick setup, Code review, Sign your commits): https://github.com/flisk-sh/flisk/blob/main/CONTRIBUTING.md
- `docs/contributing/03-open-a-pr.md`, "Run the checks for your change" (what a docs-only PR must check): https://github.com/flisk-sh/flisk/blob/main/docs/contributing/03-open-a-pr.md
- `cli/src/cli.ts` (the `demo` command's `templateArg` branch), which showed `--dry-run` is ignored
- The issue thread and the issue-claims bot's reply, which explained claiming and `Fixes #1842`: https://github.com/flisk-sh/flisk/issues/1842
