# Evidence guide: where proof lives in a reproduction package

This guide backs the checks in `rubric.md`. For each family it says
where to find the evidence and what a `pass` actually looks like once
you're looking at the right thing.

## Environment

**Where it lives.** In an eval bundle: the "Environment:" line at the
top of the candidate repro report. Compare it against the issue's own
stated version/OS (in the Issue section) and against the repo-facts
block's latest release, so you can tell a genuine version delta from a
missing fact. Live: the same "Environment:" line in the student's
draft repro report, compared against the issue body and thread.

**What good looks like.** The record names the tool/library version
and the OS/platform, plus anything the issue's own trigger turns on —
a driver flag on a driver-specific bug, a shell on a shell-dependent
one, a browser and its language settings on a locale bug. A version
older or newer than the issue's target is fine *if the report says
so*; an environment line that's simply absent, or that drops the one
component the issue depends on (see `kubernetes/minikube#11645`-style
packages: full log excerpt, zero environment, and the Windows driver
never named), is what this check exists to catch.

## Steps

**Where it lives.** The repro report's numbered steps or command
transcript, read against the issue's own "steps to reproduce." Live:
the same section of the student's draft.

**What good looks like.** A stranger holding only the stated
environment could type the same commands from the same starting state
and land on the same shown output. Watch for two tells that steps are
not actually followable even when they read fine: a step that points
at a resource nobody else can open (a private company repo, a config
that's described but never pasted), and a step silently missing an
action the platform requires (running a Windows-specific reproduction
without the flag the issue's steps used).

## Behavior shown

**Where it lives.** The artifact block(s) in the repro report — an
output excerpt, a log, a screenshot description, a transcript — read
side by side with the issue's "actual behavior" and the exact trigger
(input, flags, or code) the issue names.

**What good looks like.** Two things have to line up: the trigger and
the failure mode. The trigger lining up means the report used the same
input shape the issue used, not a substituted one that's easier to run
(a different range syntax, a different destructured expression, an
easier layout that never reaches the failing code path) — substituting
the trigger and then narrating the result as if it confirms the issue
is the single most common way these packages go wrong. The failure
mode lining up means the same *kind* of outcome: a crash stays a
crash (a terminal that stays alive after garbled output is not the
crash a "the terminal crashes" issue reports), a compile error is not
interchangeable with a different compile error, a graceful
argument-validation message is not the same thing as a capacity-overflow
panic.

A genuine cannot-reproduce is this check's *other* passing shape, not
a lesser one — and it is allowed to look imperfect. If the report
attempted the issue's real mechanism, honestly concludes the failure
did not show up, and names what precondition it couldn't match (a
shell it didn't have, an argument-length distribution it couldn't
force), that gap is expected, not a fail: an attempt that could
replicate every precondition would usually have reproduced the bug.
Don't read "the report admits it couldn't fully match the trigger" as
"the trigger was substituted" — that phrase describes a different,
failing pattern: swapping the mechanism for an easier one *and then
claiming success anyway* (a different flag, a different expression, a
scenario that never reaches the failing code path, narrated as if it
confirmed the issue). The line between the two is the conclusion: an
honest "didn't reproduce, here's what differed" passes; a "confirmed"
or "matches exactly" resting on a swapped mechanism fails.

## Honesty

**Where it lives.** The gap between the report's stated conclusion
("confirmed," "reproduced," "cannot reproduce," a named root cause)
and what its own artifact actually shows above it. Also check the
environment line against the issue's confirmed target here: a
version/OS deviation that's real but goes unmentioned while the report
still claims a match is an honesty problem, not (only) an environment
one.

**What good looks like.** The words claim exactly what the artifact
backs, no more. A root cause ("it's a debounce race," "$b is unbound
so it's a compile error") needs a trace or transcript showing that
mechanism, not just confident assertion. Certainty language
("guaranteed," "100%," "conclusively," "ran it ten times, identical
every time") is a flag to go check what's actually shown, not a
substitute for it. An honest, evidenced cannot-reproduce report that
plainly says the failure didn't occur, and names what differed from
the issue's setup, passes this check exactly as a confirmed repro
does — the lecture's point that an evidenced "no" beats a confident,
unbacked "yes."

## Comms

**Where it lives.** The candidate claim comment (and repro comment, if
present), read against: (1) what the report actually found, (2) the
repo-facts block's contribution policy, including any stated AI-use
policy for issue/PR comments.

**What good looks like, specificity.** The comment names something
concrete and true about *this* issue and *this* attempt — what was
tried, what was found, what's next — in a voice that sounds like one
person, not a template. Boilerplate self-assignment ("kindly assign
this to me," "guaranteed fix in 2 days," "I know exactly what this
is") fails this even when the attached repro report is fine; the
comment is graded on its own, because a stranger reading the thread
sees the comment first.

**What good looks like, AI policy.** Read the repo-facts contribution
policy line closely — the requirement is not the same shape everywhere.
Treat every candidate package here as AI-assisted work by default (that's
what this course's packages are); the candidate comment saying nothing
about AI either way is *not* evidence that none was used, it's a missed
disclosure if the policy required one. Some repos require disclosing AI
assistance in the comment itself (state the tool and the extent of
help) and expect real detail, not a throwaway line — the comment needs
an actual disclosure sentence to pass, silence fails. Others require
the comment to be in the human's own words with no AI-generated
comments, disclosure or not. Others say nothing at all about comments,
or restrict the ask to PR code, in
which case this check has nothing to hold the comment against and
passes automatically. Match the comment against the specific
requirement the policy states, not against a generic "should this
mention AI?" instinct.
