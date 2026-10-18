# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Determine the mode before reading evidence. A package bundle means
   eval mode; otherwise, grade the student's own PR in live mode.

2. In live mode, read `scope.md` first. If its Repo line is unfilled or
   the PR's base repository is outside scope, refuse before gathering
   evidence. In eval mode, ignore `scope.md` and use only the bundle.

3. Read `rubric.md` and `references/evidence-guide.md` before grading so
   the checks, evidence sources, and verdict rule are fixed in advance.

4. Read the issue context first: in live mode, read the issue thread,
   PR template, and `docs/CONTRIBUTING.md`; in eval mode, read the
   bundle's issue and repo-facts sections.

5. Read `plan.md`, including `## Deviations`, before opening the diff.
   For a house-chain package, read the house plan and house repro pack
   in their place. Note the planned code and test-file modifications,
   test plan, and every disclosed deviation.

6. Read the branch diff and commits: in live mode, use
   `git diff main...HEAD` and the branch's commit list; in eval mode,
   use the candidate diff and commits in the bundle. Compare the
   changed hunks with the plan notes from the previous step.

7. Read the captured test evidence against the plan's test plan and
   the repository checks. Note the command or steps, observed outcome,
   and any honestly reported non-passing or empty result.

8. In live mode, read `voice-guide.md`. Then read the draft PR title
   and description last, comparing each claim with the diff, test
   evidence, deviations, issue requirements, and voice rules. In eval
   mode, read the bundle's title and description last and ignore the
   voice guide.

## Evidence gathering

Gather everything first and grade nothing yet. The output of this
stage is an **evidence record**: five ledgers (A–E below), one per
rubric check. Every entry in a ledger holds four things: the source
and its location (bundle section, or live file / command / URL), the
verbatim quote, hunk, or output, what that entry establishes, and a
status of `found`, `not established`, or `conflict`. When a
source is absent, write it down as `not established` and name the
place you looked. Do not fill the gap with what the student probably
meant. When two sources disagree, record both and mark the entry
`conflict`. Check execution resolves it, not this stage.

Where each fact comes from depends on the mode:

| Fact                      | Eval mode (bundle section)          | Live mode                                                                          |
| ------------------------- | ----------------------------------- | ---------------------------------------------------------------------------------- |
| Planned modifications     | plan-context block: scope pair      | `plan.md` in the working copy                                                      |
| Test plan                 | plan-context block: test plan       | `plan.md`, test plan section                                                       |
| Deviations                | plan-context block: deviation notes | `plan.md`, `## Deviations`                                                         |
| Diff                      | candidate PR: diff                  | `git diff main...HEAD` (and `--stat` for the file list)                            |
| Commits and their order   | candidate PR: commits               | `git log --format='%h %ad %s' --date=iso main..HEAD`                               |
| Title and description     | candidate PR: title, description    | `pr_draft.md`, `gh pr create` body, or `gh pr view --json title,body`              |
| Test evidence             | candidate PR: test-evidence section | captured output in the description, notes file, or transcript                      |
| Branch name               | candidate PR header, if given       | `git branch --show-current`                                                        |
| Repo-required checks      | repo-facts block                    | `docs/CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md`                         |
| Template sections, policy | repo-facts block                    | `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, `scope.md` house rules |
| Issue thread              | issue context                       | `gh issue view <n> --comments`                                                     |

In eval mode, use only the middle column. If a fact isn't in its
bundle section, it is `not established`. Do not look for it anywhere
else.

### A. Plan ↔ diff (for `plan-fidelity`)

1. **Plan ledger.** From the plan, list every code or test-file
   modification it commits to, one row each: the file, function, or
   module named, and the plan line quoted. Leave out plan steps that
   only _run_ an existing test or command. Those go to ledger B.
2. **Deviation ledger.** List every `## Deviations` entry verbatim.
   Mark each one `qualifies` if it names both what differs from the
   plan and why, and `does not qualify` otherwise. Hedges such as "may
   need to adjust X" do not qualify. If the plan has no `## Deviations`
   section, record that fact. If there is no plan at all, record `no
plan.md in package` and skip to ledger C. The verdict rule fixes
   the grade for both cases.
3. **Hunk ledger.** Walk the diff hunk by hunk. For each hunk, record
   the file, the line range, and one tag: `behavior` (it changes what
   the code does, or adds or changes a test), or `non-behavior` (it
   changes nothing a run would observe).
4. **Pair plan → diff.** For each row in the plan ledger, record the
   hunk(s) that deliver it. If none do, record the qualifying
   deviation that drops or changes it. If neither exists, record
   `missing, no entry`.
5. **Pair diff → plan.** For each `behavior` hunk, record the plan
   row or qualifying deviation it traces to. If it traces to neither,
   record `untraced` and quote the hunk.

### B. Test evidence ↔ test plan and repo checks (for `tests-evidenced`)

1. **Test-plan items.** List each check in the plan's test plan, with
   any `## Deviations` entry that revises it already applied. For
   each item, record the command or steps it names, its input, and the
   observable outcome it expects.
2. **Repro re-run.** Record the repro trigger from the issue context
   (live: the issue body, or your repro pack) as one more item. Its
   expected outcome is that the reported behavior no longer occurs.
3. **Repo-required checks.** From the repo-facts block (live:
   `docs/CONTRIBUTING.md` and the PR template), list every check the repo
   requires before a PR. For each one, record whether the repo says it
   runs only in CI.
   Record the required scope only when explicitly stated; do not
   infer a whole-repository run from a generic command list.
   When pairing checks with evidence, accept an unambiguous check
   name and reported outcome, such as "fmt and clippy clean",
   according to the rubric. Exact command spelling and numeric
   counts are required only when the plan or repository asks for them.
4. **Pair each item with its evidence.** For every item from steps
   1–3, find its evidence block in the test evidence and record:
   - whether it is actual output, a concise report naming the command
     and concrete result, or only a vague assertion ("all tests pass",
     "verified locally"). The first two count as run evidence; the repro
     still requires observable before/after evidence;
   - the command, input, and observable the output shows, and whether
     each one matches what the item named;
   - the result the output shows, next to the result the package
     _reports_ for it, quoted from both places;
   - for a non-passing or empty result, the stated reason (quoted), or
     `no reason stated`;
   - for a CI-only check, whether the package claims it passed.
   - for a control case supported by an automated test, record the
     assertions in the diff and the reported passing suite run.
     Record how the package connects that test to the suite, and
     whether it shows the test was skipped. Test code alone does
     not establish that the control was run;
   - for a concise control-run report, quote the reported outcome
     and the package text identifying the control's input and
     expected behavior. Confirm that the report describes a check
     on the branch. The plan alone does not prove execution, and
     "unchanged" without an identifiable control is insufficient;

   An item with no evidence block gets `not established`.

5. **Ordering.** Only if the package shows commit times or ordering:
   record when each evidence block was captured, or the commit it ran
   against. Also record the last commit that touches the diff. If the
   evidence comes before that commit, record `evidence predates later
change`. If the package gives no ordering information, record that
   and move on.
6. If the package has no test evidence at all, record `no test evidence
quoted`.

### C. Diff noise sweep (for `diff-reviewable`)

1. Read every added and removed line of the diff, including lines in
   hunks that ledger A tagged `behavior`. Record each instance of the
   rubric's noise categories, with the hunk quoted:
   - debug output or hooks (debug `print`/log lines, `debugger`,
     breakpoints);
   - commented-out code, added or left behind;
   - formatting, whitespace, or import-order changes on lines the
     change does not otherwise touch;
   - unrelated files: editor or OS files, local config, build output,
     lockfiles.
2. For a lockfile change, record the plan row or deviation that calls
   for a dependency change. If there is none, record `no dependency
change planned`.
3. If the sweep finds nothing, record `no noise found` together with
   the list of files swept, so the pass has evidence behind it.

### D. Description ↔ diff and evidence (for `description-truthful`)

1. **Split the description into claims.** Record each sentence that
   says what changed, what was tested, or what the outcome was, as its
   own row. Leave out the `Closes #<n>` line.
2. **Pair claim → package.** For each claim, record the hunk from
   ledger A or the evidence block from ledger B that supports it. If
   nothing does, record `not shown`. A claim the package contradicts
   is recorded with both quotes.
3. **Pair package → description.** Go the other direction. List every
   `## Deviations` entry (ledger A step 2), every planned modification
   marked missing (ledger A step 4), and every non-passing or empty
   result (ledger B step 4). For each, record the description line that
   states it. A shortfall counts as stated only if the line says what
   is not done or not fixed. If no line does, record `omitted`.

### E. Description and branch ↔ stated rules (for `repo-policy-respected`)

Record one row per rule, each with the rule's source quoted and the
package fact that meets it or breaks it:

1. **AI-use disclosure.** First record whether disclosure is required:
   always in live mode; in eval mode, only when the bundle requires it.
   If not required, record `not applicable`. If required, inspect the
   location and content specified by the applicable rule. Live mode
   uses Notes for Reviewers; in eval mode, if no location is specified,
   inspect the whole PR description. Quote the disclosure, or record
   `absent` if missing.
2. **Issue link.** Record the `Closes #<n>` line, and whether `<n>` is
   the number of this plan's issue.
3. **Template sections.** List every section the template defines
   (repo-facts block; live: `.github/PULL_REQUEST_TEMPLATE.md`). For
   each one, record its content in the description and one tag:
   `specific`, `placeholder`, `empty`, `N/A with reason`, or `N/A
without reason`.
4. **Branch name.** Record the branch name and whether it matches
   `<type>/<issue-number>-<slug>` with an allowed `<type>`. Also
   record whether the issue number in the name matches this issue. If
   the bundle does not give the branch name, record `not established`.
5. **Process rules.** For each process rule the repo states (for
   example, discuss before opening a PR), record the thread comment or
   other package fact that satisfies it, or record that none exists.
6. **Maintainer requests.** Read the issue thread for maintainer
   comments directed at this contribution. Record each request with
   the PR fact that follows it, or with the description line that
   explains the departure. If there are none, record `no maintainer
request in thread`.
7. **House rules (live only).** For each house rule in `scope.md`,
   record the package fact it reads, so check execution can grade any
   rule a rubric check reads. Check execution reports the rest in the
   summary.

Leave this stage only when every ledger row has a status. The test is
simple: if a check would make the executor go back into the package
to look for something, a row is missing here.

## Check execution

1. **Start from a complete record.** Confirm every row in ledgers A–E
   has a status and a quote. If a row is missing, go back to Evidence
   gathering for that row only, record it, and return here. That is
   the only reason to reopen the package. Never re-read the whole
   package to grade a check.

2. **Run the checks in rubric order:** `plan-fidelity`,
   `tests-evidenced`, `diff-reviewable`, `description-truthful`,
   `repo-policy-respected`. Grade all five, even after one has failed.
   The output lists every check.

3. **Grade each check from its own ledger.** `plan-fidelity` reads
   ledger A, `tests-evidenced` reads B, `diff-reviewable` reads C,
   `description-truthful` reads D, and `repo-policy-respected` reads E.
   Do not grade a fact under a check the rubric sends it away from:
   - non-behavioral noise belongs to `diff-reviewable`, not
     `plan-fidelity`;
   - plan steps that only run a command belong to `tests-evidenced`;
   - the `Closes #<n>` line belongs to `repo-policy-respected`, not
     `description-truthful`.

   One fact can still count in two checks under two different
   conditions. A planned change missing with no entry fails
   `plan-fidelity` (ledger A). If the description also leaves it out,
   it fails `description-truthful` too (ledger D).

4. **Apply the verdict rule's fixed readings first.** If one applies,
   it sets the grade, and the steps below do not override it:
   - `no plan.md in package`: `plan-fidelity` is `fail`.
   - `no test evidence quoted`: `tests-evidenced` is `fail`.
   - No `## Deviations` section, and ledger A pairs every row in both
     directions: `plan-fidelity` is `pass`.
   - AI-use disclosure `absent`: grade `repo-policy-respected` as `fail`
     only when disclosure is required in the current mode. Live mode
     always requires it. Eval mode requires it only when the bundle
     states that requirement.
   - A non-passing or empty result with its reason quoted: it does not
     fail part (c) of `tests-evidenced`. Keep grading the rest.
   - `no maintainer request in thread`: the maintainer-request rule
     passes. Keep grading the other rules.

5. **Apply the pass condition to the ledger, check by check.** Grade
   `pass` only when every part of the condition is met by a `found`
   row. Grade `fail` on the first row that meets a fail condition.

   **`plan-fidelity` (ledger A).**
   - Pass: every plan row is paired with a hunk or with a deviation
     tagged `qualifies`, and no `behavior` hunk is `untraced`.
   - Fail: any plan row reads `missing, no entry`, or any hunk reads
     `untraced`. A deviation tagged `does not qualify` counts as no
     entry.
   - A shortfall disclosed in a qualifying deviation passes.

   **`tests-evidenced` (ledger B).** Fail if any of these appears:
   - a test-plan item, the repro re-run, or a repo-required check
     with no evidence block (`not established`);
   - evidence that meets none of the rubric's accepted forms:
     captured output, a named command or check with its result,
     a control test linked to a passing suite run, or an identifiable
     control-run report. Apply the conditions recorded in ledger B
     for each form. Do not require a separate command transcript
     for a qualifying control report. The repro still requires
     observable before/after evidence;
   - a command, input, or observable that does not match what the
     item named;
   - a repro re-run whose output still shows the reported behavior;
   - a reported result that contradicts the output shown;
   - a non-passing result marked `no reason stated`;
   - a CI-only check claimed as passed;
   - `evidence predates later change`.

   A CI-only check with no local evidence and no pass claim is not
   required, so it does not fail. If none of the above appears, the
   grade is `pass`.

   **`diff-reviewable` (ledger C).**
   - Fail: any recorded noise instance. The exception is a lockfile
     change paired with a plan row or deviation that calls for a
     dependency change.
   - Pass: `no noise found`.
   - The size of the diff never enters the grade.

   **`description-truthful` (ledger D).**
   - Fail: any claim marked `not shown`, any claim recorded with a
     contradicting quote, or any deviation, shortfall, or non-passing
     result marked `omitted`.
   - Pass: none of these appears. A shortfall the description states
     plainly passes.

   **`repo-policy-respected` (ledger E).** Walk the rows in ledger
   order. First determine whether each rule applies in the current mode,
   using `rubric.md`. In eval mode, mark rules the bundle does not require
   as `not applicable`; they cannot cause failure. Apply the following
   failure conditions only to applicable rules:
   - disclosure `absent`;
   - a `Closes #<n>` line that is missing or names a different issue;
   - a template section tagged `placeholder`, `empty`, or `N/A
without reason`;
   - a branch name that breaks the rubric's `<type>/<issue-number>-<slug>`
     pattern or names a different issue;
   - a process rule with no fact that satisfies it;
   - a maintainer request neither followed nor explained.

   The rubric spells out the branch types, so grade the branch against
   its list. If a branch passes the rubric but breaks the `fix/` house
   rule in `scope.md`, report the conflict in the summary and leave the
   grade alone. A house rule the pass condition does not name never
   changes this grade; it goes in the summary. If none of the rows
   above fails, the grade is `pass`.

6. **When evidence is absent, resolve it in this order:**
   1. If a fixed reading or the pass condition says the absence fails
      (a check with no evidence of being run, an omitted deviation,
      absent disclosure), grade `fail`.
   2. If the pass condition applies only when the package shows the
      fact, that part does not apply. This covers ordering when the
      package gives none, and a rule "the package can be checked
      against", such as a branch name the bundle never gives. Grade
      the rest, and say in the summary that the part was not
      checkable.
   3. If a ledger row is missing because this procedure has no
      gathering step for it, report that gap in the summary.
   4. Otherwise, grade `unclear`. The evidence line names what is
      missing and what would have settled it.

   Never grade `pass` from silence.

7. **When evidence conflicts:**
   - If the pass condition names the conflict as a failure (a reported
     result that contradicts the output, or a description claim the
     package contradicts), grade `fail` and quote both sides.
   - Otherwise, grade `unclear` and quote both sides.

8. **Grade as stated.** Apply each pass condition literally, even when
   the result feels wrong. Put the disagreement in the summary, not in
   the grade. Do not change a grade because of another check's result
   or the verdict you expect.

9. **Record each grade with its deciding evidence.** For every check,
   write three things:
   - the check name, spelled as the rubric spells it;
   - the grade;
   - one evidence line, taken from the ledger.

   For a `fail` or `unclear`, quote the hunk, output, or passage that
   decided it, with its location. If several rows fail, quote the first
   one in ledger order and list the rest in the summary. For a `pass`,
   name the ledger fact that closes the condition, for example "4/4
   planned changes in diff (`src/parse.py`, `tests/test_parse.py`); 0
   untraced behavior hunks." Never restate the pass condition, and
   never write "looks fine."

10. **Compare the outgoing text with `voice-guide.md` (live mode
    only).** Do this after all five grades are recorded.
    - Hold the PR title and the PR description against every rule in
      `voice-guide.md`, including its "Things I never post" list.
      Check nothing else against it: not the diff, the commits, the
      plan, or the test output.
    - For each broken rule, write one entry in the human-readable
      summary with four parts: the rule's name, the rule quoted from
      `voice-guide.md`, the line of the title or description that
      breaks it, and one sentence on what the rule asks for instead.
      Do not rewrite the student's text.
    - If nothing breaks a rule, say so in one line of the summary.
    - If `voice-guide.md` holds only its template comments, say in one
      line that it is unfilled, and skip the comparison.
    - A broken voice rule never changes a grade. The one exception is a
      rubric check whose Evidence or Pass condition explicitly names
      `voice-guide.md`. Only that check reads the guide, and it grades
      it under its own condition. No other check counts a voice break
      as evidence, and a voice break never decides a verdict.

    In eval mode, skip this step and do not read `voice-guide.md`.

11. Pass the five recorded grades to Verdict assembly unchanged.

## Verdict assembly

1. **Wait until all five grades are final.** Start only once
   `plan-fidelity`, `tests-evidenced`, `diff-reviewable`,
   `description-truthful`, and `repo-policy-respected` each have a
   grade and an evidence line from Check execution step 9. If one is
   missing, go back and grade it. From this point on, no grade
   changes.

2. **Apply the rubric's verdict rule exactly.** All five checks are
   `required`.
   - **`accept`** only if all five are graded `pass`.
   - **`reject`** if any one is graded `fail` or `unclear`. An
     `unclear` counts the same as a `fail`.
   - A `preferred` check, if the rubric ever adds one, is reported
     but never enters the verdict.

   Nothing else enters the verdict: not voice breaks, house-rule notes,
   checks you disagree with, or your overall impression. If the rule's
   answer surprises you, say so in the summary and keep the answer.

3. **Name the deciding check.** On a `reject`, the deciding check is
   the first required check in rubric order graded `fail` or
   `unclear`. Its evidence line is the quote the summary leads with.
   On an `accept`, there is no deciding check; say that all five
   required checks passed.

4. **Write the human-readable summary, in this order:**
   1. The verdict and, on a `reject`, the deciding check with its
      quote.
   2. Every check in rubric order, passes included, each with its
      grade and the evidence that decided it. For a `fail` or
      `unclear`, quote the hunk, output, or passage. Also list any
      further failing rows from that check's ledger. Every failing
      required check gets reported, not only the deciding one.
   3. The notes that do not change the verdict: broken voice rules
      (live mode, from Check execution step 10), house rules the
      package breaks that no check grades, the rubric-versus-`scope.md`
      branch conflict if it came up, parts of a check that were not
      checkable, gaps in this procedure, and any grade you disagree
      with.

5. **Emit the JSON block last.** Fill the fenced `json` block from
   SKILL.md's "Verdict and output" section:
   - `item`: in live mode, the PR URL, or the issue URL plus the
     branch name if the PR is not open yet. In eval mode, the bundle's
     id exactly as given.
   - `checks`: all five, in rubric order. Each `name` is spelled as
     the rubric spells it, `grade` is `pass`, `fail`, or `unclear`,
     and `evidence` is the same one-line evidence used in the summary.
   - `verdict`: `accept` or `reject`, from step 2.

   Do not add, rename, or reorder fields. The block must be valid JSON
   and the last thing in the reply. Nothing comes after the closing
   fence: no closing thought, no next steps, no offer to help.

6. **A refused run emits no block.** If the run stopped at the scope
   seam, or because `rubric.md` or `procedure.md` is empty, nothing was
   graded. Say why in plain words, and write no verdict and no JSON.
