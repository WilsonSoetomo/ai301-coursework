# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer is active | repo-facts block: last 5 default-branch commits; issue thread: maintainer first-response sample and comment author associations | Pass if the repo has at least one default-branch commit in the last 90 days and at least one maintainer/owner/member/collaborator response on the issue within 30 days of the capture date, or a comparable human maintainer signal in the thread | required |

| Repo is currently used | repo-facts block: latest release, last push to any branch, archived flag, stars | Pass if the repo is not archived, the latest release is within 365 days, and the repo shows recent activity (last push within 180 days) | required |

| Issue scope fits a newcomer | issue body and comments: bug report / feature ask, acceptance criteria, discussion history, maintainer comments | Pass if the task is a bounded, single-piece change, not a tracking/umbrella issue, not a pure support question, and not explicitly described as requiring core internals or large design churn | required |

| Nobody else is already on it | issue sidebar and comments: assignees, linked PRs, claim comments, label history | Pass if there is no active assignee, no open linked PR, and no clear public claim such as “I’m working on this” that a maintainer accepted without a later handoff or closure | required |

| Beginner-friendliness is visible | issue labels, comments, or maintainer wording such as “good first issue,” “beginner-friendly,” or an explicit invitation to contribute | Pass if a maintainer clearly signals the issue is suitable for a newcomer, or the work is otherwise narrow and straightforward enough for a first contribution | preferred |

| Contribution workflow is allowed | repo contribution policy and any AI policy files or templates | Pass if there is no explicit ban on AI-assisted work; if the repo imposes conditions, they are workable (disclose use, understand/test changes, human review) | preferred |

## Verdict rule

Accept if every required check passes. Preferred checks do not change the verdict; they only help rank the accepted issues. If any required check is fail or unclear, reject. Unclear counts as fail unless a specific check explicitly says otherwise.
