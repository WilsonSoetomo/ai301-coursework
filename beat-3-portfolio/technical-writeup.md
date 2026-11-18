# Fixing Flisk #1842

## Overview

[One short paragraph, your words: what flisk is, the problem in #1842, what you did, how it ended (merged, shipped in v1.3.205). Link the PR: https://github.com/flisk-sh/flisk/pull/1846. If you include a tool, name the part you wrote (rubric, manifest, or playbook).]

## What happened

I claimed the issue publicly after finding it on GitHub by filtering for public issues that matched my criteria. It was a good first issue with no assignees, and I posted a plan with my claim.

I think one of the biggest issues I faced was understanding how to repeat the dry run. I ran the issues on a clean main which was wrong. It also wasn't a clean run: it cloned the demo, installed flisk globally and ran the stack locally.

```
Issue step 7:  node cli/bin/tdk.js demo --dry-run
Observed:      not a dry run: cloned the demo, installed flisk globally, then `flisk up` exited 1
Shipped:       node cli/bin/tdk.js doctor
```

My PR contained a new page plus a link to it in CONTRIBUTING.md, in a signed-off commit, with a deviation list and an offer of a separate issue. The 4 failing tests were explained and checked.

```diff
 ## Quick setup
 
+New here? The [contributor quick start](docs/contributing/quick-start.md) goes from clone to your first pull request in about five minutes.
```

| Step | Command | Result |
| --- | --- | --- |
| 3 | `npm --prefix cli test` | 2561 passed, 4 failed, 93 skipped (4 need Docker/Tilt or the macOS /var symlink; CI on main green) |
| 4 | `npm --prefix cli run lint` | passes (3 warnings, 5 infos) |
| 7 (issue's version) | `demo --dry-run` | not a dry run |

My PR was approved and merged within 30 minutes. The issue closed automatically, and the change shipped in a release the next day.

> Merged, thank you @WilsonSoetomo! 🎉 This fixes #1842. ... This is your first merged PR here.
> Shipped in v1.3.205 (from #1846).

## Limitations and what's next

The biggest limitations were that some of the tests were failing on Mac, and that my tools ran after the merge, so they didn't catch the right errors.
