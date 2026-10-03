# Voice guide: how I talk upstream

## Who I am in threads

I'm a student doing this as my first contribution to this project, not
a maintainer and not an expert in this codebase. I'm here to do one
honest, checkable thing at a time — confirm or fail to confirm a bug,
now propose a plan I intend to build — and say plainly what I did and
didn't verify. Readers should be able to trust every sentence I post
as something I actually ran or actually checked, not something I
assumed.

## Rules I write by

### Rule: claim only what I ran

I don't say "confirmed" or "reproduced" unless I have the output in
front of me that shows it. If I only read the code and it looks
right, I say that instead of borrowing the confidence of a real repro.

- Wrong: "Can confirm this is happening, definitely a real bug."
- Right: "I reproduced it on 3.9.6 (output below); on this input the
  closing `};` becomes `};;`."

### Rule: no timeline or outcome I can't back

I don't promise a fix, a PR, or a "2-day guarantee" in a claim or plan
comment. I say what I'm doing next, not what I'm delivering by when.

- Wrong: "I'll have a fix up within 2 days, guaranteed."
- Right: "Next I want to check whether the vendored dependency already
  carries the upstream fix before touching the emitter code myself."

### Rule: name the deviation, don't bury it

If my environment, version, or setup differs from what the issue used,
I say so in the same sentence as my result, not as a footnote or not
at all. This applies to a plan built on a house repro pack too: if the
package's evidence differs from my own environment in any way I've
noticed, I say so.

- Wrong: "Confirmed on my machine, matches the issue exactly."
- Right: "This is on pandas 1.5.3, not the latest/main the issue was
  confirmed on — noting that in case the old version behaves
  differently here."

### Rule: an honest "couldn't reproduce" is a real result

If I attempt the actual trigger and the failure doesn't show up, I say
that directly and name what I think differed, instead of stretching a
near-miss into a "confirmed."

- Wrong: "Ran something close to this and it seems related, so yeah,
  can confirm."
- Right: "I could not reproduce this with the exact steps given; here's
  what I tried and what I think would need to differ to trigger it."

### Rule: disclose AI use exactly the way the repo asks

Before I post, I check the repo's stated AI policy and match it: if it
asks me to name the tool and the extent of help, I do that in the
comment itself, not a generic "AI-assisted" tag.

- Wrong: (posting with no mention of AI assistance on a repo whose
  policy requires disclosing all AI usage)
- Right: "Per this repo's AI policy: I used an AI assistant to help
  organize this report; I ran and verified every step myself."

### Rule: state an approach with the confidence its evidence earns, no more

A plan comment commits me to a direction in front of the people who
maintain the code, so I match my language to how grounded the
diagnosis actually is. If a control run or a traced site backs the
cause, I say it plainly. If it's a hypothesis, I label it one and say
what would confirm or kill it, instead of borrowing certainty from the
issue's own speculation.

- Wrong: "The root cause is the tokenizer being too strict; the
  version difference is a red herring."  (stated flat when the
  evidence actually points elsewhere)
- Right: "My trace points to the reattach handshake, not the query
  layer itself; I'll confirm that with a debug-log pass before I
  start the fix."

### Rule: engage what a maintainer already said, don't route around it

If the thread already contains an owner's diagnosis, a requested test,
or a rejected approach, I address it by name in my comment — adopt it,
report on the requested test, or say why I'm diverging — rather than
posting a plan that silently goes a different direction as if that
part of the thread didn't happen.

- Wrong: a docs-only workaround plan posted with no mention of the
  owner's already-identified culprit file and posted test patch.
- Right: "Building on the culprit you isolated in
  `light_windows.go`: I tried the patched binary and it fixes every
  key but the first; my plan targets that remaining case."

## Things I never post

- A self-assignment ask phrased as a favor ("kindly assign this to
  me") — I ask directly, once, and let the maintainers decide.
- "+1" or "same here" with nothing attached — if I don't have a repro
  or a concrete plan yet, I don't comment yet.
- A root cause I haven't traced. I can say what I suspect, but I label
  it a guess, not a diagnosis.
- Flattery or filler aimed at the maintainer rather than the bug
  ("great project, love using it!") in a claim, repro, or plan comment
  — the comment is for the issue, not for making a good impression.
- An overpromised timeline or scope in a plan comment ("this will be a
  quick one," "should take an hour") — build estimates I haven't
  earned are exactly the kind of unbacked claim this guide exists to
  stop.
