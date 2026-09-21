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
For all recency checks, measure days against the bundle's capture date in eval mode and today's date in live mode.

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Active maintainer | Eval: last 5 default-branch commits and their authors, maintainer first-response sample, and issue comments with author_association. Live: default-branch commit/PR history and maintainer responses or reviews. | Within the past 90 days, at least one human-authored contribution reached the default branch, or a maintainer provided a substantive issue response or PR review. A bot merging a human-authored PR qualifies; automated-only activity does not. | required |
| Repository in use | Eval: archived status, latest release, and last push to any branch in Repo facts. Live: archive banner, Releases, and branch activity. | The repository is not archived, and at least one release or push occurred within the past 180 days. | required |
| Manageable scope | Issue body and comment thread. | The contribution has one clearly defined, bounded outcome. Coordinated edits across multiple files or documentation pages can pass when they serve that same outcome; file count or a checklist alone does not make an issue an umbrella task. Several related instances of the same change can count as one bounded task. For terse issues, maintainer authorship and a good-first-issue label support a bounded-scope interpretation, unless the body or thread shows one of the rejection conditions listed below. The number of documentation topics, related files, or optional supporting edits alone is not a rejection condition. Distinguish explicitly required changes from additional suggestions; suggestions alone do not expand the mandatory scope unless the body or thread makes them requirements. Multiple causes of one reported bug do not automatically make it an umbrella issue. A good-first-issue label is supporting evidence, not a requirement; its absence alone does not cause rejection. Reject umbrella/tracking issues containing independent tasks intended to be split, unresolved design debates affecting the requested work, maintainer-confirmed core-internals changes, and pure usage questions. A short description or missing reproduction steps alone does not cause failure. Also reject an issue open for at least 2 years when the supplied history documents at least 2 separate attempts abandoned or ended for inactivity. Age alone does not cause failure, and a PR marked closed without evidence of its outcome does not by itself count as an abandoned attempt. | required |
| Issue available | Eval: assignees and linked PRs in Repo facts, plus the comment thread. Live: Assignees, Development, and the comment thread. | No current assignee, no open PR addressing the issue, and no unwithdrawn claim or progress comment within the past 30 days. Use later thread evidence to resolve conflicting status. Closed, unmerged PRs alone do not block availability. | required |
| Compatible contribution policy | Eval: contribution policy in Repo facts. Live: CONTRIBUTING.md, linked contributor docs, AI policy files, and PR/issue templates. | No explicit ban on AI-assisted or AI-generated contributions. Disclosure, personal understanding, testing, and human-review requirements are conditions to follow, not reasons to reject. No stated policy passes after checking the sources; unavailable policy evidence is unclear. | required |

## Verdict rule

Accept only if every required check passes. A required check graded
fail or unclear means reject. Preferred checks only rank accepted
issues and never change the verdict.



