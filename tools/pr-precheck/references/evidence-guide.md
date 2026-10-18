# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

Two checks read this evidence. `plan-fidelity` compares the plan with
the diff (procedure ledger A). `description-truthful` compares the
description with the diff (procedure ledger D).

### Where it lives

**In an eval bundle:**

- **Plan-context block, scope pair.** The code and test-file changes
  the plan commits to: the files, functions, modules, and tests it
  names.
- **Plan-context block, deviation notes.** Each recorded departure
  from the plan.
- **Candidate PR, diff.** The hunks that actually changed.
- **Candidate PR, description.** The sentences that say what the PR
  changed.

If one of these is missing from the bundle, it is `not established`.
Do not rebuild the plan from commit messages, or the description from
the title.

**In live mode:**

- **`plan.md` in the working copy**, including its `## Deviations`
  section. A house-chain student reads the house plan instead.
- **`git diff main...HEAD`**, with three dots so the diff holds only
  the branch's own work. Add `--stat` to get the list of changed
  files.
- **The draft PR description**, wherever it currently sits: a draft
  file, the `gh pr create` body, or `gh pr view --json body`.

Commit messages are neither the plan nor the description. Do not use
them in place of either.

### What good looks like

- **Plan → diff:** every code or test-file change the plan commits to
  shows up as a hunk in the file, function, or test the plan names. If
  it doesn't, a `## Deviations` entry says it was changed or dropped.
- **Diff → plan:** every hunk that changes behavior traces back to a
  planned change or to a deviation entry. Lines that change no
  behavior belong to diff quality, not here.
- **Description ↔ diff:** every claim about what changed matches a
  hunk. Every deviation and every shortfall is stated in the
  description, which says plainly what was not done.

**An honest deviation re-ties a mismatch.** It counts only if it names
what differs from the plan and why. "Dropped the planned test for the
empty-input case; the parser already rejects empty input upstream, so
the case cannot reach this function" counts. "May need to adjust the
parser" and "some refactoring along the way" do not.

### What silent drift looks like

- **More than the plan:** a hunk that changes behavior in a file or
  function the plan never names, with no deviation entry. The typical
  case is a "while I was here" fix to a neighboring function.
- **Less than the plan:** a planned change or test that is missing
  from the diff, with no deviation entry.
- **A description that drifts from the diff:** it claims "adds tests
  for X" but no test file changes; it says "fixes the bug" while a
  deviation records a partial fix; or it leaves out a deviation the
  plan records.

**These are not drift:**

- A shortfall recorded in `## Deviations` and stated plainly in the
  description. That is honest disclosure, and it passes.
- A plan with no `## Deviations` section, as long as the diff matches
  the plan in both directions.

## Test evidence (harness category: not-tested)

`tests-evidenced` reads this evidence (procedure ledger B). It holds
the test evidence against three things: the plan's test plan, the
repro re-run, and the checks the repo requires before a PR.

- **Control cases supported by a suite run.** Test assertions in the
  diff can show the control's input and expected outcome. Pair them
  with a reported passing run of the suite that includes that test,
  using the package's evidence to establish the connection. Check
  for any indication that the test was skipped. Assertions alone
  do not prove execution.
- **Concise control-run reports.** Look in the test evidence for the
  reported control outcome, and in the plan or repro context for
  its input and expected behavior. A report such as "no-ampersand
  control unchanged" counts when those details identify the control
  unambiguously and the report describes a check on the branch.
  The plan alone does not prove execution.

### Where it lives

**In an eval bundle:**

- **Plan-context block, test plan.** Each check the plan promised to
  run, with its command or steps, its input, and the observable
  outcome it expects.
- **Plan-context block, deviation notes.** Any entry that revises the
  test plan. Apply it before pairing evidence with the test plan.
- **Candidate PR, test-evidence section.** The pasted commands, the
  output, and the before/after for the repro.
- **Repo-facts block, required checks.** The checks the repo requires
  before a PR, and which of them it says run only in CI.

Evidence that is not in the test-evidence section is `not
established`. A claim in the description such as "tested locally" is
not evidence.

**In live mode:**

- **`plan.md`**: its test plan, as revised by `## Deviations`. A
  house-chain student reads the house plan instead.
- **The captured output**, wherever the student kept it: a notes file
  such as `test_evidence.md`, the draft description, or a terminal
  transcript.
- **The repro's before/after evidence**: the trigger run before the
  fix, which shows the bug, and the same trigger run after, which
  shows it gone. A house-chain student uses the house repro pack.
- **The PR template and `docs/CONTRIBUTING.md`**, for the checks the
  repo requires and which of them run only in CI.

Grade the output the package shows, not what the student says they
ran.

### What good looks like

- **Named commands and concrete results.** A test run may be evidenced
  by captured output or a concise report naming the command and its
  concrete result, such as "pytest tests/ -q: 1462 passed, 12 skipped".
  A vague "all tests pass" does not count. The repro still requires
  observable before/after evidence showing the reported behavior.
- **The planned input and observable.** The output shows the same
  input and the same observable outcome the test plan named. Output
  from a different command or a different input does not count, even
  if it passes.
- **The repro re-run.** The trigger from the issue, run after the fix,
  shows the reported behavior no longer happens.
- **Every repo-required check reported truthfully.** Each one was run,
  with captured output or a named command and concrete result.
  Any shown output must agree with the reported result. Non-passing
  or empty results still require shown output and a stated reason.
  A CI-only check is not required locally, but the package must not
  claim it passed without evidence.
- **Repository check names and scope.** An unambiguous check name
  with its reported outcome, such as "fmt and clippy clean", counts
  as a concise run report. Require exact command spelling, numeric
  counts, or whole-repository scope only when the plan or repository
  explicitly requires them. A generic command list does not establish
  a whole-repository test requirement.

**A non-passing result is still usable evidence** when its output and
its reason are both shown. Path Review's `make test-integration`
printing "no tests ran", together with a line saying the repo has no
integration tests for this module yet, is a truthful report, and it
passes. Grade the honesty of the report, not the exit status.

### What not tested looks like

- **A summary instead of output:** "all tests pass", "verified
  locally", or "CI is green", with no command and no output.
- **Missing evidence:** a test-plan item or a repo-required check with
  no evidence that it was run at all.
- **The wrong test:** output from a different command, input, or
  observable than the plan named. For example, the full suite passing
  when the plan named a specific failing case to re-run.
- **A result that contradicts its output:** the package reports
  success while the output shows failures or errors, or claims a
  specific test ran when it was skipped. Reported skips alone do
  not contradict a passing suite result.
- **A failure with no reason:** non-passing or empty output that the
  package does not explain.
- **Stale evidence:** where the package shows commit order or times,
  evidence captured before a later commit that changed the diff.

## Diff quality (harness category: unreviewable)

`diff-reviewable` reads this evidence (procedure ledger C). It reads
every added and removed line of the diff and looks for content that
does not contribute to the planned change or a documented deviation.

### Where it lives

**In an eval bundle:**

- **Candidate PR, diff.** The unified diff: every file header, hunk,
  and added or removed line.
- **Candidate PR, commits.** The commit list. Use it to spot a commit
  that exists only to reformat, to add debugging, or to touch an
  unrelated file.

**In live mode:**

- **`git diff main...HEAD`**, with three dots so the diff holds only
  the branch's own work. Add `--stat` to list every changed file,
  including files a reader might scroll past, such as `.DS_Store` or a
  lockfile.
- **`git log main..HEAD`**, with two dots, for the branch's commits.

Grade the diff the branch will submit, not the working tree. A
debugging change that was never committed is not in the package.
Removing debug code is acceptable. If one commit adds debug code and a
later commit takes it out, that is not a failure, as long as no debug
code remains in the final `main...HEAD` diff. The commit list helps
you find where to look, but the final diff decides the grade.

### What good looks like

The diff contains the intended change and nothing riding along with
it. A reviewer reading it top to bottom sees only lines that do one of
three things: implement the plan, implement a documented deviation, or
add or change the tests for them. The size of the diff is never
graded. A large diff with no noise passes, and a one-line diff with a
stray `print` fails.

**Lockfiles:** a lockfile change (`package-lock.json`, `yarn.lock`,
`poetry.lock`, `Cargo.lock`, and similar) is acceptable only when the
plan or a `## Deviations` entry calls for a dependency change.
Without one, it is an unrelated file.

### What unreviewable looks like

Any single instance fails. Quote the hunk.

- **Debug output or hooks:** `print`, `console.log`, or logger lines
  added for debugging, plus `debugger`, `breakpoint()`, or `pdb.set_trace()`.
- **Commented-out code:** old code commented out with `#` or `//` and
  left in place, or new code added already commented out.
- **Formatting or import churn on untouched lines:** reindentation,
  whitespace or line-ending changes, quote-style changes, or
  reordered imports on lines the change does not otherwise touch.
- **Unrelated files:** editor or OS files (`.vscode/`, `.idea/`,
  `.DS_Store`), local config or `.env` files, build output (`dist/`,
  `__pycache__/`, `*.pyc`), and lockfile changes with no planned
  dependency change.

A drive-by edit that changes behavior, such as a fix to a neighboring
function the plan never names, is silent drift. Grade it under plan
fidelity as an untraced hunk, not here.

## Standards and comms (harness category: standards-wall)

`repo-policy-respected` reads this evidence (procedure ledger E). Each
requirement has two places to look: where the rule is stated, and
where the PR shows it was met. Record both for every row.

### Where it lives

**In an eval bundle,** use only the bundle as evidence. Apply the
requirements explicitly defined in `rubric.md`, plus repository-specific
rules stated in the bundle. Do not import additional rules from
`scope.md`, a live repository, or outside knowledge. When information
needed to check a requirement is absent, follow the rubric and
procedure's missing-evidence rules.

| Requirement         | Where the rule is stated                                         | Where the evidence is                                                                      |
| ------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| AI-use disclosure   | repo-facts block's disclosure policy, if required                | candidate PR description, in the specified section or anywhere if no location is specified |
| Issue link          | bundle's stated issue-link requirement, if any                   | candidate PR description, checked against the required format and issue                    |
| Template sections   | repo-facts block, template requirements                          | candidate PR description, section by section                                               |
| Branch name         | bundle's stated branch-naming requirement, if any                | candidate PR branch information, if given                                                  |
| Process rules       | repo-facts block, contributing instructions                      | issue context (thread), candidate PR description                                           |
| Maintainer requests | issue context, maintainer comments directed at this contribution | candidate PR diff or description: the request followed, or the reason                      |

If the bundle gives no branch name, the branch rule cannot be checked
against the package, so it does not fail the check. Record that.

**In live mode:**

| Requirement         | Where the rule is stated                                                       | Where the evidence is                                                |
| ------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------- |
| AI-use disclosure   | course requirement; `.github/PULL_REQUEST_TEMPLATE.md`; `docs/CONTRIBUTING.md` | `pr_draft.md`, Notes for Reviewers section                           |
| Issue link          | course requirement; PR template, issue section                                 | `pr_draft.md`, the `Closes #<n>` line                                |
| Template sections   | `.github/PULL_REQUEST_TEMPLATE.md`; `scope.md` ("the template is always used") | `pr_draft.md`, section by section                                    |
| Branch name         | rubric's `<type>/<issue-number>-<slug>` rule; `scope.md` fork and branch rule  | `git branch --show-current`                                          |
| Process rules       | `docs/CONTRIBUTING.md`; `scope.md` house rules                                 | `gh issue view <n> --comments`; `pr_draft.md`                        |
| Maintainer requests | `gh issue view <n> --comments`, maintainer comments on this contribution       | `git diff main...HEAD` or `pr_draft.md`: request followed, or reason |

If the description is not in `pr_draft.md` yet, read it from wherever
it currently sits: the `gh pr create` body, or `gh pr view --json
body` for a PR that is already open.

### What good looks like

Apply the examples below according to the current mode. The specific
Notes for Reviewers, Closes, and branch-format requirements describe
live mode. In eval mode, use only the bundle's stated requirements,
including its required disclosure content, location, issue-link format,
and branch convention. A requirement the bundle does not state is
not applicable. This also governs the failure examples under
"What a standards wall looks like."

- **Every required section has specific content.** Each section the
  template defines holds text about this PR. "N/A" passes only with a
  reason, such as "N/A: no UI change in this PR".
- **AI-use disclosure is in Notes for Reviewers.** A line there names
  the AI tool and how much it helped, for example "Used Claude Code to
  draft the test cases and review the diff; I wrote the fix and
  checked every line." If the line is absent, the check fails. It is
  never `unclear`.
- **The `Closes #<n>` line is correct.** It sits in the issue section,
  and `<n>` is the number of the issue this plan belongs to.
- **The branch name complies.** It matches `<type>/<issue-number>-<slug>`,
  with `<type>` one of `fix`, `docs`, `feat`, `test`, `refactor`,
  `perf`, or `chore`, and the issue number matches this issue. The
  house rule in `scope.md` names `fix/`. If the branch meets the
  rubric but not the house rule, the procedure reports the conflict
  in the summary and does not change the grade.
- **Process rules are met.** For example, where the repo asks
  contributors to discuss before opening a PR, the thread shows that
  discussion.
- **Maintainer requests are addressed.** Each request a maintainer
  made of this contribution is either followed in the PR, or the
  description says why the PR departs from it. A thread with no such
  request passes, with the evidence line `no maintainer request in
thread`.

### What a standards wall looks like

- **Template boilerplate left in place:** the template's placeholder
  text or HTML comments still standing in for content, an empty
  section, or "N/A" with no reason.
- **Missing or vague disclosure:** no disclosure line in Notes for
  Reviewers, or a line that does not name the tool or the extent of
  assistance ("AI was used").
- **A wrong or missing issue link:** no `Closes #<n>` line, or one
  that names a different issue.
- **A non-compliant branch:** `patch-1`, `my-fix`, or a type outside
  the allowed list, or an issue number that is not this issue's.
- **An ignored ask:** a maintainer request with no matching change
  and no explanation, or a process rule the thread shows was skipped.

**Not graded here:**

- Tone, length, and phrasing. In live mode, the voice guide covers
  those, and it never changes a grade.
- Whether the description's claims match the diff. That is plan
  fidelity, above.
