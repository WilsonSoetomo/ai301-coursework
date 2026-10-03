# Evidence guide: where evidence lives in a plan package

This guide backs the checks in `rubric.md`. For each family it says
where to find the evidence and what a `pass` actually looks like once
you're looking at the right thing.

## Diagnosis and grounding

**Where it lives.** In an eval bundle: the candidate plan's Diagnosis
section, read against the Repro evidence block's steps, artifacts, and
especially its control runs (a control is what rules a cause in or
out, not just the failing run alone). Live: the student's draft
`plan.md` diagnosis against their posted repro comment (or, on the
house issue, the house repro pack as quoted in the drafts).

**What good looks like.** The stated cause is a fact the repro
evidence's own artifacts demonstrate. Read every control run before
judging: a control that reproduces cleanly with the "culprit"
unchanged, or that isolates a different site than the diagnosis names,
rules that diagnosis out even when the plan states it with total
confidence and cites the issue's own speculation (`httpie/cli#1838`-
style packages: the tokenizer is blamed, but the repro's own
`--debug` trace shows argparse never hands the items to the tokenizer
at all). A diagnosis the evidence simply doesn't speak to either way
is `unclear`, not a pass — don't let confident prose stand in for a
missing citation.

An inferred mechanism is not the same thing as an ungrounded one: a
plan can name a cause no single artifact shows directly (no debug
trace catches the stale pointer in the act) and still pass, as long as
that cause is the one explanation consistent with every control run
recorded (`zellij-org/zellij#5174`-style packages: fresh attach clean,
every reattach leaking, the pre-regression version clean, one clean
attach after a cache clear — no artifact shows the stale pointer
mid-flight, but no control fits any other explanation either). Reserve
`unclear` for when the cited evidence is silent on the claimed cause,
not for "inferred rather than caught on camera."

## Scope

**Where it lives.** The candidate plan's Scope statement and its
Files/Approach list, read against the Diagnosis: is every listed
change at a site the diagnosis implicates, or does the approach reach
further?

**What good looks like.** One bounded change touches the file(s) or
site(s) the diagnosis names and nothing structurally larger. Two
things both count as fine, not scope creep: a plan that is honestly
narrow (fixes the one case the repro covers, explicitly defers a
related-but-untested variant with a reason — `sharkdp/fd#2067` and
`zellij-org/zellij#5174`-style packages) and a plan that engages
existing prior art instead of racing it. What fails is the approach
folding in work the diagnosis never required: a redesign, a version
bump "since we're in there," a new abstraction unifying code paths
nobody asked to unify, a new CI matrix, a settings panel — every one
of these can sit next to a perfectly correct core fix
(`kubernetes/minikube#21408`, `prettier/prettier#19116`,
`laurent22/joplin#16215`-style packages), and the correctness of the
core fix does not rescue the check.

## Executability

**Where it lives.** The candidate plan's Files and Approach sections,
read as a stranger with no access to the author and no ability to ask
a follow-up question.

**What good looks like.** Specific files or code sites are named, one
approach is chosen (not "X or Y, not sure yet"), and the steps have
enough order that work could start today. Watch for decisions
deferred to build time dressed up as a plan: "recover() somewhere",
"fix upstream or vendored, whichever is easier", "not sure if it's
gocui or tcell" (`golangci/golangci-lint#6467`,
`jesseduffield/lazygit#5703`-style packages) all fail even when the
prose around them is confident and detailed, because the one thing a
stranger needs — where to start typing — was never decided.

## Test plan

**Where it lives.** The candidate plan's Test plan, read against the
Repro evidence's steps and artifacts (the same commands, the same
observable failure) so you can tell whether the test plan actually
closes the loop the repro opened.

**What good looks like.** A decisive test plan re-runs the repro's own
steps and names the expected observable change: an error becomes a
clean exit, a garbled value becomes the right one, a fixture across
the reported spellings all resolve the same way. A vague test plan
substitutes a feeling for an observation — "should feel fast",
"nothing else should feel broken" (`starship/starship#7677`-style
packages) — or defers to the general suite with no assertion about the
fix's own behavior (`BurntSushi/ripgrep#3070`-style: a genuinely solid
plan that still only says "run the full test suite," which proves the
suite didn't regress, not that this fix works). Running the existing
suite is a fine addition to a test plan; it cannot be the whole of it.

## Honesty

**Where it lives.** The gap between the plan's Diagnosis/Approach
language and its own stated Risk or open-questions material, if any;
live, also the gap between a mid-build deviation and whether `plan.md`
was updated to record it before the follow-up comment went out.

**What good looks like.** Every claim of cause or completeness is
exactly as strong as what's cited for it, and a genuine unknown — an
untested platform, an unmeasured cost, a maintainer question still
open — is named as unknown rather than folded into confident prose
(`ghostty-org/ghostty#11261`-style packages name the untested
benchmark and the open per-use-vs-two-sites question directly, which
is what an honest plan under real uncertainty looks like). A plan with
nothing to flag and nothing overclaimed also passes; this family
exists to catch dressed-up certainty, not to require a risk section
for its own sake.

## Comms

**Where it lives.** The candidate plan comment, read against two
things: (1) the Thread highlights, for any explicit maintainer
diagnosis, requested test, or rejected approach; (2) the repo-facts
block's contribution policy, including any stated AI-use policy.

**What thread-aware looks like.** When a maintainer has already named
a culprit file, posted a patched test binary and asked for it to be
tried, or explicitly rejected an approach as too costly, the plan and
comment engage that fact — by adopting it, running the requested test,
or stating a reasoned divergence — rather than pursuing an unrelated
angle in silence. `junegunn/fzf#4260` is the clean failure shape: a
correctly-scoped, honestly-tested docs-only plan that never engages
the owner's already-posted culprit file and patched binary, so the
comment reads as if that half of the thread didn't happen. No thread
highlight stating a direction means this has nothing to check against,
and it passes.

**What good looks like, AI policy.** Read the repo-facts AI-use line
for its exact shape, not a generic "should this mention AI?" instinct.
Treat every candidate comment here as AI-assisted work by default (the
comment saying nothing about AI is not evidence none was used). A
policy that requires disclosing AI assistance in comments needs an
actual disclosure sentence naming the tool and the extent of help —
`ghostty-org/ghostty#11261`-style packages fail this even with an
otherwise excellent, thread-aware plan, because the disclosure sentence
is simply absent. A policy asking for the comment to be in the human's
own words is judged on voice instead. A policy silent on comments, or
scoped only to PR code, has nothing to hold the comment against, and
this check passes automatically.
