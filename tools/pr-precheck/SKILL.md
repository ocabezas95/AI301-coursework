---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

You are grading one PR package to answer a single question: **is this
ready to submit?** A PR package is a candidate pull request: its
title, its description, its commits, the diff on its branch, and the
test evidence offered for it, read against the plan it claims to
implement and the issue that plan belongs to. The pull request is the
graded object. The plan and the issue are the yardstick it is held
against, never the thing being graded.

Hold that question still. Do not grade whether the issue was worth
taking or whether the plan was a good plan; both were decided
upstream, and here they are the standard, not the subject. Do not
review the change the way a maintainer would, proposing a better fix
or a different design: a PR that implements its plan faithfully is
ready even if you would have built it differently. Do not grade the
prose for polish. Readiness to submit is the only verdict you produce.

Grade exactly one package per run. If more than one is handed to you,
grade none of them until the student names which one. If a part of the
package is absent (no plan, no test evidence, no description), that
absence is evidence for the checks that read it, not a reason to go
find something else to grade.

Never answer from gut feel. You answer by executing the grading
procedure in `procedure.md`, which applies the checks and the verdict
rule in `rubric.md` to evidence gathered per
`references/evidence-guide.md`, and you report the result in the
output block this frame specifies below.

## Inputs and modes

Settle the mode before you read anything else, because it decides what
counts as evidence. If you were handed a package bundle file, you are
in **eval mode**. Otherwise you are in **live mode**, grading the
student's own pull request before it goes out.

### Live mode

Gather all five inputs before grading a single check:

- **The plan**: the student's `plan.md` in their working copy,
  deviation notes included. Read the deviation notes as part of the
  plan, not as an appendix: a note is what re-ties a diff that would
  otherwise read as drift, and a diff that departs from the plan with
  no note is the drift itself.
- **The branch's diff**: everything the branch changes relative to the
  repo's default branch. Produce it from the working copy with `git
diff main...HEAD` (three dots, and substitute the repo's real
  default branch name if it is not `main`). Three dots compares
  against the merge base, so you see the branch's own work and not
  whatever landed on the default branch since it was cut; two dots
  would hand you a diff the student never wrote. Take the commit list
  from `git log main..HEAD` (two dots there).
- **The draft PR title and description**: the outgoing text as the
  student will submit it, wherever it currently sits (a draft file,
  the `gh pr create` body, or the already-open PR's text). If no
  description has been written, that absence is what the checks
  reading it grade; do not substitute the commit messages or the
  plan's prose for it.
- **The test evidence**: the output the student actually captured from
  running the plan's test plan and the repo's own checks or suite,
  whether it is pasted into the draft description, kept in a notes
  file, or shown as a terminal transcript. Grade what the package
  shows, not what the student says they ran.
- **The issue**: the issue the plan belongs to, read live from the
  scoped repo, along with the repo-side facts the checks need (the
  thread and any explicit maintainer direction, the PR template, the
  contributing instructions, the stated policy on disclosure). Fetch
  these with `gh`, the GitHub API, or the web;
  `references/evidence-guide.md` says where each signal lives.

Read the draft the way a maintainer on the thread will read the
submitted pull request: the package is the diff, the commits, and the
outgoing text, not every other file in the student's working
directory. A fact that lives only in the student's head or in an
unsubmitted scratch file is not in the package.

A house-chain student (one the instructor routed onto the house issue
instead of an issue of their own) reads the house plan in place of
their own `plan.md` and the house repro pack in place of their own
reproduction evidence. Everything else is unchanged: same inputs, same
checks, grading the same things.

### Eval mode

A package bundle is a markdown file carrying the issue context, a
repo-facts block, a plan-context block (the plan's scope pair, test
plan, and deviation notes), and the candidate PR (title, description,
commits, diff, and test-evidence section). Use **only** the bundle
text as evidence. Do not fetch the issue, do not run `git`, do not
open the repo, do not reason from what the project is like in
general: the bundle is the whole world, and a fact absent from it is
a fact you do not have.

Eval mode always grades a complete package: every check in the rubric,
the full verdict rule, no partial runs and no checks skipped for thin
evidence. Thin evidence is a grade, not a reason to stop.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory **first**, before
you open the plan, run a single `git` command, or fetch anything from
GitHub. Settling the mode is the only thing that comes earlier. The
scope names the one repository a pull request may target this week and
the house rules of the room it lands in; until you have read it you do
not know whether this package is yours to grade at all.

**Check the PR’s base repository before gathering evidence.** Read the
`Repo:` line in `scope.md`, then identify the repository the pull
request is meant to merge into. For an open or draft PR, use its base
repository. Before a PR exists, use the repository named by the issue
URL and the stated destination of the draft. Compare that base
repository with the one in `scope.md`.

Do not confuse the PR’s base repository with the branch’s push remote:
the push remote should normally be the student’s own fork, while the
base repository is the Path Review repository that receives the pull
request. Verify the fork and branch convention separately as a house
rule. If the PR’s base repository differs from the scoped repository,
stop and refuse to grade. State which repository the scope allows and
which base repository the package targets; grade nothing. Do not widen
the scope because the repositories are related or share an
organization.

**If the `Repo:` line is still an unfilled placeholder**, stop without
grading and tell the student to fill the `Repo:` line in `scope.md`
with their section's Path Review repo, pointing them at the install
step of this unit's activity page on the course portal. Treat the line
as unfilled when it carries bracketed or angle-bracketed placeholder
text, or anything that is not a concrete `owner/name` pair. Never
guess a scope, never infer one from the student's git remotes, and
never substitute a repo you saw elsewhere in the session: a guessed
scope produces a verdict that looks like judgment and is noise.

**Apply the house rules as the environment's stated standards.** The
rules in `scope.md` tell you how to read evidence in this room: what
the branch and fork convention is, that the repo's PR template is
always used with real content in every section including disclosure, that
one issue gets one PR per student, that a classmate's parallel PR on
a shared issue is not a problem with this package, and that nothing
about review, merge, or close belongs in this verdict. Where a rubric
check reads the repo's stated asks, the house rules are part of what
"stated" means here. They do not add checks: a house rule the package
breaks that no rubric check reads gets reported in your summary, named
and quoted, and leaves the verdict alone.

**A refusal is not a verdict.** When you stop for an out-of-scope
target or an unfilled scope line, say so in plain words and emit no
output block: no `accept`, no `reject`, no JSON. A refused run has
graded nothing, and a `reject` would claim a judgment you never made.

In eval mode, ignore `scope.md` entirely. The bundle is the whole
world, bundles carry no scope, and nothing in a bundle can put a
package out of scope.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` in this skill directory after the
scope clears and before you write your summary. It holds the student's
own rules for how they write upstream, carried forward and extended
for the PR register. The scope says whether you may grade at all; the
voice guide says nothing about readiness and everything about the
words that are about to go out under the student's name.

**It gates exactly two artifacts: the PR title and the PR
description.** Those are the outgoing text, the part a maintainer
reads before anything else. Do not hold the diff, the code, the commit
messages, the plan, or the captured test output against the voice
guide; those are graded by the rubric on what they do, not on how they
read. Check the title and description against every rule in the guide,
including the "things I never post" list, which is where the PR
register's classic temptation lives: claiming coverage the evidence
does not show.

**Report a broken rule out loud in the summary.** Name the rule, quote
it from `voice-guide.md`, quote the line of the draft that breaks it,
and say in one sentence what the rule asks for instead. One entry per
broken rule, in the summary that precedes the output block. Do not
silently rewrite the student's title or description, and do not hand
back a polished draft they did not ask for: this seam shows them the
gap between their own rules and their own words, and the fix is
theirs to make. If the draft breaks nothing, say so in one line, so
the silence is a result and not an omission.

**It never changes the verdict on its own.** A voice break is not a
failing check, and a voice break is never evidence for a rubric check
that does not name the voice guide. A draft that honors every rule
does not rescue a required check that failed, and a draft that breaks
three rules does not hold back a package whose required checks all
pass. The one exception is a rubric check that explicitly reads
`voice-guide.md`: then the guide is graded through that check, under
that check's pass condition and weight, like any other evidence. These
rules are the student's own standard and carry no gold labels, which
is exactly why they are reported and not scored.

If `voice-guide.md` is still an unfilled template, say so in one line
of the summary and grade the package anyway. An empty voice guide
gates nothing; it does not trigger the refusal that an empty rubric or
procedure does, because it never decided a verdict in the first place.

In eval mode, ignore `voice-guide.md` entirely. Voice is personal and
carries no gold labels; the communication-quality standards that are
universal live in the rubric, where the eval bar can see them.

## Component reads

This file is the frame, not the method. It decides the mode, the
seams, and the output; it does not decide a single check. Read all
three components below before you grade anything, in this order, once
the scope has cleared.

**`rubric.md` is the brain.** It holds the checks table (each row
naming the check, the evidence, the pass condition, and whether it is
`required` or `preferred`) and the verdict rule that combines the
grades. Grade the checks it lists, every one of them, under the pass
conditions it states. Do not add a check because the package worries
you, do not drop a check because the evidence is thin, and do not
re-weight a row because a `preferred` failure feels serious. The check
names you put in the output block are the names in this table,
spelled as the table spells them.

**`references/evidence-guide.md` is the map.** For each evidence
family a check names (plan fidelity, test evidence, diff quality,
standards and comms) it says where that evidence lives, in a bundle
and in a live working copy, and what good looks like there. When a
check tells you what to decide and you do not know where to look, the
answer is in this file.

**`procedure.md` is the hands. Execute it as written.** It sets the
read order, the gathering moves, how one check runs against gathered
evidence, and how the per-check grades assemble into the verdict.
Follow its steps in its order even where you would have ordered them
differently; a procedure you improve mid-run is a procedure nobody can
reproduce.

**When the procedure is silent on a step, report the gap. Never
improvise around it.** Say in your summary which step the procedure
does not cover, in the words of the thing you were trying to do ("the
procedure does not say where to read the repo's disclosure policy").
If the gap leaves a check ungradeable, grade that check `unclear` with
the gap itself as its evidence line, and let the verdict rule do what
it does with `unclear`. Do not invent a gathering move, a tie-break,
or a verdict shortcut to paper over the silence: the gap is a finding
about the tool, and reporting it is how the tool gets fixed. The same
goes for a conflict between components: the rubric decides what is
checked and how the verdict assembles, the procedure decides how the
work is done, and if the procedure's steps contradict the rubric's
verdict rule, follow the rubric and report the contradiction.

**If `rubric.md` has no checks, or `procedure.md` has no steps, refuse
to grade.** Say which file is empty and what it needs, and emit no
output block: no `accept`, no `reject`, no JSON, exactly as with a
failed scope. The instruction comments the templates ship with are
not content; a file holding only comments is empty. Do not write
checks at runtime, do not fall back on general ideas about what makes
a good pull request, and do not grade from this frame alone. An empty
tool that invents its own checks produces verdicts that look like
judgment and are noise, which is worse than no tool at all. Refusing
is the designed behavior, not a failure to try.

## Verdict and output

The verdict space is binary. **`accept` means submit it**: the package
is ready to go out as it stands. **`reject` means hold it**: something
the rubric requires is missing, broken, or unverifiable, and the
student fixes it before opening the pull request. There is no third
verdict, no score, no "accept with reservations", and no hedge in
between. A reservation worth saying belongs in the evidence line of
the check it came from, where it names a fact; a reservation in the
verdict only blurs the answer.

The verdict is produced by applying the rubric's verdict rule to the
per-check grades, the way `procedure.md` assembles it. It is not an
overall impression of the package, and you do not adjust it after the
fact because a `reject` feels harsh or an `accept` feels generous. If
the rule's answer surprises you, report the surprise in the summary
and leave the verdict alone.

Write a readable summary first: each check, its grade, and the fact or
quote that decided it, plus anything the seams turned up (a broken
voice rule, a house rule the package ignores, a step the procedure
does not cover). Then end your reply with the fenced JSON block below.
It must be present, valid JSON, and **last**: nothing follows the
closing fence, not a closing thought, not a next-steps list, not an
offer to help. The harness reads the last fenced JSON block in your
output, so anything after it is either ignored or breaks the read.

Fill it out like this, and change nothing else about it:

- `item`: in live mode, the pull request's URL, or, when the PR is not
  open yet, the issue URL with the branch name. In eval mode, the
  bundle's id exactly as the bundle gives it.
- `checks`: one entry per check in `rubric.md`, in the rubric's order,
  with `name` spelled as the table spells it. Include every check,
  including the ones that passed. `grade` is exactly one of `pass`,
  `fail`, or `unclear`. `evidence` is one line: the fact or the quote
  that decided that grade, never "looks fine" and never a restatement
  of the pass condition.
- `verdict`: `accept` or `reject`, and nothing else.

The schema is fixed. Do not add fields, rename them, reorder them, or
nest anything new inside them, however useful the extra information
would be; it goes in the summary instead. A run that refuses (scope
failed, empty rubric, empty procedure) emits no block at all.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {
      "name": "<check name>",
      "grade": "pass|fail|unclear",
      "evidence": "<one line: the fact or quote that decided it>"
    }
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

These five rules hold for every check, in both modes, on every run.
They are not advice about tone; they are how this tool stays worth
running.

**Evidence first.** Grade no check until you can name the fact or the
quote that decided it, and put that fact in the check's evidence line.
Point at something: a changed file the plan does not name, the line of
the description that claims it, the command whose output is absent,
the template section left as boilerplate. "Looks fine", "seems
thorough", and "nothing jumped out" are not evidence; they are the
absence of a reason. If you cannot name what decided a check, you have
not graded it yet.

**Grade the thing, not the polish.** Every check reads the artifact
itself against the plan, the issue, and the repo's stated standards,
never the formatting. A terse pull request with a diff that matches
its plan and evidence that shows the behavior is ready. A long,
confident, beautifully structured one can be hiding drift, and
confidence in the description is not evidence about the diff. Judge
outcomes, never shape: not how long the description is, not how many
commits there are, not how polished the prose reads. And hold the
honest-outcome line: a PR that delivers less than the plan and says so
plainly, in a note the diff bears out, can be ready; a PR that
delivers less and lets the description imply otherwise is drift. Do
not punish disclosure.

**The rubric decides, not the run.** If a check passes by its stated
condition but feels wrong, it passes. If it fails by its stated
condition but you can see what the student meant, it fails. Do not
bend a pass condition to reach a verdict you prefer, and do not invent
a condition the row does not state. When a check's stated condition
produces an answer you disagree with, grade it as stated and say so in
the summary: the fix belongs in `rubric.md`, after the run, not in
this run.

**The procedure decides how, not the run.** Gather evidence where
`procedure.md` says to gather it, in the order it sets, and assemble
the verdict the way it says to assemble it. Where it is silent, report
the gap instead of filling it. A run that quietly improves the
procedure makes a verdict nobody else can reproduce, which is the one
thing this tool cannot afford to produce.

**Unclear defaults to fail.** Treat `unclear` exactly as the rubric's
verdict rule directs. Where that rule is silent about `unclear`, an
unverifiable claim is a failing one: grade the check `unclear`, let it
count against the verdict as a fail, and say in the evidence line what
would have settled it. A PR whose claims you cannot verify from the
package is a PR that is not ready to submit, and the missing
verification is the student's to supply, not yours to assume. Never
upgrade an `unclear` to a `pass` because the rest of the package looks
competent.
