# Contribution: Isolate per-file exceptions so one failure does not abort the batch

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Oscar Cabezas
**GitHub Username:** ocabezas95
**Issue:** https://github.com/brunothinker/FallByte/issues/50
**Pull Request:** https://github.com/brunothinker/FallByte/pull/73

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

**Unit 5: Building and refining my issue-scout**

2026-10-28

I configured issue-scout with four GitHub search queries and ranking rules based on my Python experience and interest in AI projects, documentation, and small fixes. The initial scan rejected all five candidates, showing that search labels and metadata alone did not establish whether an issue was available or manageable. I added `-linked:pr` to all four queries to exclude issues with linked pull requests and tested the updated first query. I also graded two candidates from the CodePath list, `PyVista # 1629` and `PyGMT # 1666`, and their rejections taught me to look for one clearly defined change instead of a broad tracking issue.

**Unit 6: Checking my claim and recording repository context**

2026-11-18

My initial Unit 6 work concerned the YouTube autoplay issue, preserved in `youtube-contribution-story.md`. I revised its claim after the voice guard required clearer evidence that I had read the discussion and checked availability, and I created `CLAUDE.local.md` for that repository. For `FallByte #50`, the guard required a concrete deadline for a progress update; I revised the claim to promise an update within two days. The revised claim passed, and the maintainer assigned the issue to me. These checks helped me distinguish a clear investigation commitment from a promise to finish a fix.

**Unit 7: Using the ship gate to check my FallByte contribution**

2026-11-18

I configured FallByte’s ship manifest with batch/UI tests and Ruff checks, and documented the full-suite macOS limitation in the excluded-checks table. Early gate runs rejected gaps in plan deviations, test evidence, logging coverage, documentation scope, and course naming conventions. I corrected those findings and refreshed the evidence against the committed code rather than treating passing focused tests as sufficient. The final gate passed all manifest checks and pre-PR checks, and I opened PR #73 through Claude Code.

**Unit 8: PR monitor and follow-up playbook**

2026-11-22

I used the PR monitor on my merged FallByte PR #73, and it drafted a thank-you that acknowledged the maintainer’s suggestion to move repeated error handling into a BaseController helper. The draft couldn’t verify whether issue #50 had closed, so it included a follow-up that wasn’t needed. I kept the draft private and recorded the helper suggestion as a lesson for future changes. I also set a four-day wait before nudging an open PR, based on a review of five recent merged FallByte PRs.

---

## Why I Chose This Issue

I switched to FallByte #50 because Python is more familiar to me, and this issue offered a clearer reproduction path than the YouTube autoplay bug I had been investigating. I preserved that earlier work in youtube-contribution-story.md.

I found this candidate through GitHub searches and reviewed its description, availability, relevant code, and contribution guidelines with help from ChatGPT and the GitHub connector. My initial FallByte candidate, #54, was already assigned when we rechecked it, so I selected #50 instead. I did not record a formal issue-scout verdict for #50.

The issue describes a concrete failure: an exception while processing one file stops the remaining batch. A three-file test could demonstrate the problem without requiring real media conversion. This fit my Python experience and gave me a manageable way to practice contributing to an existing project.

Claim comment: https://github.com/brunothinker/FallByte/issues/50#issuecomment-6938680601

The maintainer assigned the issue to me and approved proceeding, asking me to follow the contribution guide and PEP 8/Ruff standards.

**Toolkit**

- Tool: GitHub connector and ChatGPT, using my fit profile to review candidates and repository evidence.
- What it produced: A recommendation for #50 based on its Python implementation, concrete failure, and available test approach.
- Difficulties: Candidate #54 was already assigned by the time we rechecked it. Search results alone did not establish current availability.
- What changed: I checked the upstream issue discussion and assignment status before claiming #50. I would make that freshness check earlier in future searches.

---

## Understanding the Issue

**Problem Description**

FallByte processes media files in batches. Previously, an unexpected exception while handling one file caused the batch runner to stop, leaving later files unprocessed and failure counts incomplete.

**Expected Behavior**

An ordinary file error should count as one failure, identify the affected file, and allow processing to continue. User cancellation should stop the batch and run its cleanup. Errors that prevent the batch from continuing should be reported clearly to the user.

**Current Behavior**

Before the fix, a three-file reproduction with an exception on the second file reported only one success instead of the expected two. The exception escaped the loop into an outer handler, which returned immediately.

The submitted fix isolates per-file errors and reports batch-level errors explicitly. The reproduction now passes. Maintainer review is still required.

**Affected Components**

- src/utils/batch_runner.py: processing loop, result counts, progress reporting, and cancellation cleanup.
- The six image, video, and audio compression/conversion controllers in ui/controllers/: displaying returned batch errors.
- tests/batch/test_run_batch.py: runner regression tests.
- tests/ui/test_controller_execution.py: error-response tests across all six controllers.
- docs/ARCHITECTURE.md: the documented error-handling contract.

**Toolkit**

- Tool: Claude Code and ChatGPT for source review, explanation, and review of the reproduction.
- What it produced: A diagnosis connecting the early batch exit to one try/except surrounding the entire loop.
- Difficulties: The original test expected the crashed file not to be counted as failed, so passing existing tests did not establish correct behavior.
- What changed: I changed the reproduction to use three files, with the second raising RuntimeError, and checked the expected success and failure counts.

---

## Reproduction Process

**Environment Setup**

I used macOS 27.0 arm64 with Python 3.13.16. I cloned my FallByte fork, created a .venv environment, and installed the dependencies from tests/requirements-dev.txt.

The existing batch tests passed before I modified the reproduction. The test uses temporary files and a fake processor, so reproducing this bug does not require real media conversion or FFmpeg.

**Steps to Reproduce**

1. Open tests/batch/test_run_batch.py and locate test_run_batch_handles_unexpected_exception.
2. Create three temporary input files named a.png, b.png, and c.png.
3. Make the fake processor return True for a.png and c.png, but raise RuntimeError for b.png.
4. Expect total_files == 3, success == 2, failed == 1, and cancelled is False.
5. Run:

```bash
.venv/bin/python -m pytest tests/batch/test_run_batch.py::test_run_batch_handles_unexpected_exception -v
```

**Reproduction Evidence**

Before the production fix, the test failed at the success-count assertion:

```text
E       assert 1 == 2
```

The total-file and cancellation assertions passed. The failed-file assertion was not reached because the test stopped at the earlier assertion.

After the fix, the same reproduction passed. A later assertion also verified that an ERROR log record contains the second file’s path.

- Implementation commit: https://github.com/brunothinker/FallByte/pull/73/commits/356784d
- Follow-up test and documentation commit: https://github.com/brunothinker/FallByte/pull/73/commits/9154c6b
- Pull request containing the reproduction and fix: https://github.com/brunothinker/FallByte/pull/73

**Toolkit**

- Tool: pytest, Claude Code, and ChatGPT.
- What it produced: A failing three-file reproduction before the fix and a passing regression test afterward.
- Difficulties: The original one-file test could not show whether processing continued after a crash. The repository’s test folders had also changed from an earlier version we reviewed.
- What changed: I used the current tests/batch/ layout, added files after the crashing input, and checked observable counts and error logging rather than relying on a general “tests pass” result.

---

## Solution Approach

**Analysis**

In src/utils/batch_runner.py, one outer try/except surrounded the entire processing loop. When the second file raised an exception, execution left the loop and returned incomplete results before processing the third file.

The controllers also treated returned results as normal completion unless cancellation or empty input was reported. They needed a separate response for batch-level errors.

**Proposed Solution**

Handle ordinary file errors inside each loop iteration, count each completed file’s outcome once, and continue processing. Preserve InterruptedError as cancellation rather than counting it as a file failure.

Return stats["error"] when setup or batch-level reporting fails. Have all six batch controllers display that error instead of a success message. Keep existing empty-input handling and reuse existing translated messages.

**Implementation Plan**

1. Isolate destination construction, processing, and output checks per file.
2. Update counters after determining the file’s final outcome.
3. Preserve cancellation and tracking of outputs eligible for cleanup.
4. Return explicit batch-level errors from setup and progress reporting.
5. Update the six controllers to handle returned errors.
6. Add runner and controller regression tests and update architecture documentation.

**Deviations**

During implementation review, start-progress callback errors were being counted as file failures, while completion-callback errors reached the outer handler. We added \_ProgressCallbackError and \_emit_progress to handle both consistently as batch-level errors without misclassifying the media file.

Cleanup tracking was extended to include known-new outputs left by failed or interrupted processors. Tests exposed these cleanup gaps and verified the changes without deleting pre-existing destination paths.

**Toolkit**

- Tool: plan-check reviewed plan.md and plan-comment.md; Claude Code assisted implementation; ChatGPT reviewed the changes.
- What it produced: An accepted plan and implementation covering continuation, cancellation, error reporting, tests, and documentation.
- Difficulties: The first plan review queried my fork’s disabled issue tracker rather than the upstream issue. It also identified missing reproduction evidence and insufficiently specific edit locations.
- What changed: I supplied the upstream issue URL, quoted the failing output, named affected files and methods, and recorded implementation deviations.

---

## Testing Strategy

I used a regression test that failed before the production change and passed afterward: three files, a RuntimeError on the second, and expected results of two successes and one failure. The test also verifies that the error log names the failed file.

Additional runner tests cover destination-building errors, cancellation, cleanup of newly created outputs, preservation of pre-existing destination paths, progress-callback errors, scanner errors, and output-folder creation errors. Controller tests check that all six batch controllers display returned errors, suppress success dialogs, and restore controls and standard streams.

Validation commands:

```bash
.venv/bin/python -m pytest tests/batch tests/ui
.venv/bin/python -m ruff check .
.venv/bin/python -m ruff format --check .
git diff --check main...HEAD
```

The batch and UI tests passed: 241 total. Ruff lint, formatting, and the whitespace check passed.

The full suite was also run on commit 9154c6b:

```bash
.venv/bin/python -m pytest
```

Result: 3 failed, 343 passed, 28 skipped, and 68 errors. The recorded pristine baseline had 3 failed, 326 passed, 28 skipped, and 68 errors. The three failing test names and grouped error counts matched; the baseline did not retain every error ID, so this was not a complete ID-by-ID comparison.

The failures and errors were associated with the existing FFmpeg binary manager rejecting macOS with “Unsupported operating system: darwin”. Linux/Windows FFmpeg-dependent behavior was not verified locally. I disclosed this limitation in the PR rather than claiming the full suite passed.

**Toolkit**

- Tool: pytest, Ruff, git diff --check, Claude Code, and the ship gate.
- What it produced: Passing focused regression checks and an explicit record of full-suite platform limitations.
- Difficulties: Earlier evidence referred to an older working-tree state, and formatting changed repeatedly on disk. The cause of the rewrites was not confirmed.
- What changed: I refreshed the evidence with commands, results, commit references, and file hashes. I corrected formatting and verified it again with the VS Code project closed. The final ship-gate run passed the manifest checks and pre-PR review.

---

## Implementation Notes

**2026-11-17 — Implementation and review**

I worked on the fix with Claude Code and reviewed the code and test results. The implementation isolates ordinary file errors while preserving cancellation. Review also exposed cleanup gaps and inconsistent treatment of progress-callback failures, which we addressed with focused regression tests.

Some implementation edits disappeared between steps, and formatting repeatedly changed on disk. We confirmed the repository, branch, and actual saved diff before reapplying missing changes. The cause was not established, so I did not attribute it to a particular tool.

**2026-11-18 — Submission checks**

The first ship-gate review rejected the branch because deviations were undocumented, evidence was stale, a promised log assertion was missing, and the documentation contained unrelated formatting changes. It also enforced the course’s branch and issue-link conventions.

I recorded the deviations, added the log assertion, narrowed the documentation diff, refreshed test evidence, renamed the branch to fix/50-batch-file-exceptions, and used Closes #50 in the PR description. The final gate passed. I pushed the branch and opened PR #73 through Claude Code.

The implementation is recorded in commits `356784d` and `9154c6b`.

**Toolkit**

- Tool: Claude Code, ChatGPT, Git, voice guard, and ship-gate/pr-precheck.
- What it produced: The implementation, regression tests, review reports, two commits, and a checked PR.
- Difficulties: Saved files and earlier reports sometimes described different states. The gate also caught gaps between the plan, tests, evidence, and description.
- What changed: I reviewed actual diffs, separated historical results from current evidence, checked file hashes, and resolved the gate findings before opening the PR.

---

## Pull Request

**PR Link:** https://github.com/brunothinker/FallByte/pull/73

**PR Description:** The PR links to issue #50 and explains how per-file exception handling prevents one failed file from stopping the batch. It describes cancellation cleanup, explicit batch errors, six-controller error handling, regression tests, and architecture updates. It discloses AI assistance and the existing macOS/FFmpeg test failures.

**Maintainer Feedback**

- **November 18, 2026:** The maintainer merged PR #73 and suggested moving the repeated error-handling block across the six controllers into a shared `BaseController` helper in a future PR.
- **My response:** I kept the PR monitor’s thank-you draft private and recorded the suggestion for future work. The maintainer didn’t request changes before merging.

**Status:** Merged

**Toolkit**

- Tool: voice guard, ship-gate/pr-precheck, Claude Code, and GitHub CLI.
- What it produced: A checked PR description, a passing final ship-gate verdict, and upstream PR #73.
- Difficulties: Early gate runs rejected missing deviation notes, stale evidence, a missing log assertion, documentation formatting noise, and course naming conventions.
- What changed: I addressed the findings, disclosed the platform limitations, and opened the PR only after the final gate passed.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned that passing tests do not automatically prove the behavior is correct. The original test expected a crashed file not to increase the failure count. Changing it to use three files exposed whether processing continued and whether the results matched the intended behavior.

I also learned to distinguish ordinary file failures, user cancellation, and batch-level errors. A file failure should be counted while the batch continues. Cancellation should stop processing and trigger cleanup. A setup or progress-reporting error needs an explicit error result rather than being mistaken for a failed media file or normal completion.

**Challenges Overcome**

The full suite failed on macOS because the existing FFmpeg binary manager did not support that platform. Comparing results with the recorded baseline helped separate that limitation from the contribution, while keeping the unverified behavior visible.

Keeping the plan, implementation, test evidence, and PR description consistent also required several revisions. The ship gate caught missing evidence and undocumented changes that focused test runs alone did not reveal.

**What I'd Do Differently Next Time**

I would check issue availability immediately before starting setup, keep evidence tied to the current code state as I work, and record plan deviations when they happen. I would also investigate unexpected file rewrites earlier so formatting and saved-code differences do not repeatedly interrupt validation.

---

## Resources Used

- [FallByte issue #50](https://github.com/brunothinker/FallByte/issues/50): the reported behavior, acceptance criteria, and maintainer’s approval to proceed.
- [Contribution guide](https://github.com/brunothinker/FallByte/blob/main/docs/CONTRIBUTING.md): environment setup, test locations, Ruff requirements, and PR expectations.
- [Project configuration](https://github.com/brunothinker/FallByte/blob/main/pyproject.toml): pytest and Ruff settings.
- [My pull request #73](https://github.com/brunothinker/FallByte/pull/73): the submitted implementation, regression tests, documentation changes, and validation limitations.
- CodePath Unit 7 starter instructions and installed plan-check, pr-precheck, voice-guard, and ship-gate materials: planning, evidence review, wording checks, and submission checks.
- Claude Code and ChatGPT: implementation assistance, explanations, draft preparation, and review. I personally reviewed the final code and test results.
