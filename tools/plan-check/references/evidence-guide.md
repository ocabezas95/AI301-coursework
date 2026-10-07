# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

This family feeds two checks, `diagnosis-grounded` and
`targets-cause-not-symptom`. Both hold what the plan says is wrong up
against what the repro showed.

### Where it lives

**The plan's cause.** In eval mode, look in the candidate plan for the
section that says why the bug happens. It's usually called
"Diagnosis", "Root cause", or "Why this happens". If there's no such
section, the cause is the "because" sentence in the approach. Read the
plan comment too, since it often restates the cause in a shorter form.
In live mode, look in the same places in the student's draft `plan.md`
and draft comment.

**Where the change lands.** The files, functions, or layers the plan
says it will edit, found in the scope statement and the approach.
Write down the most specific location the plan names. A function
beats a file, and a file beats "the parser".

**The behavior the cause has to explain.** In eval mode this is the
repro-evidence block. Pull three things out of it: the steps, the
outcome it observed (plus the expected outcome if it gives one), and
every quoted artifact, meaning tracebacks, error messages, log lines,
printed values and version info. In live mode it's the student's
posted repro comment on the issue thread. On the house issue it's the
house repro pack, but only the parts the drafts quote. Don't open the
pack itself. If the drafts quote none of it, there is no repro
evidence.

**Where the evidence puts the fault.** Inside those artifacts: the
deepest stack frame in the project's own code (skip the library and
stdlib frames below it), the step where the output first differs from
what was expected, and the place the bad value first shows up.

### What good looks like

Before reading the plan's cause, list every behavior the repro pins
down, one line each. Include the negative results too ("works with
input A, fails with input B"). Then go down the list with the stated
cause in hand and mark each line explained, not explained, or
contradicted.

A grounded diagnosis names a mechanism and points at something in the
repro that backs it up. For example: "the traceback ends in
`parse_date()` with `KeyError: 'tz'`, so the dict reaches it without
that key." Every line on the list is explained and none is
contradicted.

It isn't grounded when:

- You could delete the repro block and the diagnosis would read the
  same, because it never cites a step or an artifact.
- The cause explains the headline error but not a second artifact,
  like a warning logged before the crash or a result that only shows
  up on one version.
- The artifacts rule it out. The plan blames the network layer but
  the traceback never leaves the parser, or the plan says "all inputs"
  and the repro shows only one input failing.

For cause versus symptom, put the plan's edit location next to where
the evidence puts the fault. If the edit lands there, it targets the
cause. It's a symptom fix when the evidence points further up and the
plan catches the exception at the crash site, adds a `None` check
where the value gets displayed, or special-cases the exact input from
the repro. Sometimes the evidence stops short: the trace ends inside a
dependency, or there's no trace at all, only wrong output. In that
case the plan only has to edit the deepest layer the evidence does
show and say plainly that the rest of the cause is unconfirmed.

**No repro quoted means both checks are `fail`.** Record the evidence
as "not established", but don't grade it `unclear`. The rubric fixes
this reading, and it overrides the general "absent means unclear" step
in `procedure.md`.

When sources disagree, the repro-evidence block wins over the plan's
paraphrase of it. If the plan quotes an error or a step differently
from the repro block, grade against the repro block and note the
mismatch. If the plan and the plan comment give different causes,
grade the plan's cause here and leave the mismatch to
`unknowns-stated-honestly`.

## Scope

This family feeds `scope-bounded`. It holds every change the plan
proposes up against the behavior the issue reports.

### Where it lives

**What the plan says it will change.** In eval mode, read three parts
of the candidate plan: the scope statement (often "In scope" or
"Changes"), every file, module or function it names, and the approach.
Read the test plan too. A "while I'm at it" change sometimes hides
there, like moving the whole suite to a new test runner. In live mode,
look in the same places in the student's draft `plan.md`.

**What the plan says it won't change.** The "Not in scope" or "Out of
scope" line, if there is one, plus any follow-up the plan mentions
("I'll open a separate issue for X"). Write these down, but they don't
count as evidence for passing. The check grades the changes, not the
section.

**The behavior the fix is for.** In eval mode this is the issue
context: the title, the body, and what the reporter says is wrong. In
live mode it's the issue body on GitHub. Use the reported behavior,
not the plan's retelling of it. A plan can widen its own scope by
describing the bug more broadly than the reporter did.

### What good looks like

List every change the plan proposes, one line each, pulled from the
scope statement, the approach, and the test plan together. If a change
appears in the approach but not in the scope statement, it still goes
on the list. Then, for each line, ask: does this fix the reported
behavior, or does the plan show that the fix depends on it? Mark it
needed, depends (with the plan's reason quoted), or extra.

A bounded plan has no extras. Here's an example, for an issue that
says the date parser crashes on timestamps with no timezone. "Default
a missing `tz` to UTC in `parse_date()` and add a test for a
timestamp without one" is bounded. Add "rename `parse_date` to
`parse_datetime` for clarity, reformat the module, and bump
`dateutil`" and it's a drive-by rewrite, unless the plan explains why
the fix can't happen without the bump.

Mark a line extra when it's any of these and the plan gives no reason
the fix depends on it:

- a refactor, rename or style cleanup
- a dependency bump
- a new feature or option nobody in the issue asked for
- a fix for a nearby bug, even a real one

Mentioning a nearby problem as a separate follow-up is fine. Folding
it into this change is not. Wording like "while I'm in there", "also
clean up" or "might as well" usually marks the spot.

A "Not in scope" section doesn't save a plan whose approach does the
thing it ruled out, and missing that section doesn't sink a plan whose
changes are all needed. When the scope statement and the approach
disagree, the approach is what will get built, so grade the approach.

Changes the plan comment promises but the plan doesn't contain aren't
scope evidence. Leave that mismatch to `unknowns-stated-honestly`.

A bounded audit, grep, or inspection is not extra scope by itself when it only looks for the same failure pattern and the plan commits to reporting or opening a follow-up rather than modifying additional sites. Judge the planned code changes, not limited investigation needed to confirm their boundary.

## Executability

This family feeds `executable-by-stranger`. Read the plan as someone
who has the repo but has never talked to the author, and check whether
they could start the work.

### Where it lives

**What will be done.** In eval mode, read the candidate plan's
approach, the files, modules or functions it names, and any numbered
steps or order of work. In live mode, look in the same places in the
student's draft `plan.md`. The plan comment doesn't count here. A
stranger building the fix works from the plan.

**What makes a location unambiguous.** A plan doesn't always need to
spell out the path. In eval mode, check the issue context and the
repo-facts block. If the issue says the crash is in `utils/dates.py`
and the plan says "in the date parser", the location is settled. In
live mode, check the issue body and the repo itself. If the plan
names a file or function, confirm it exists on the default branch. A
path that isn't there is a path a stranger can't open.

**Knowledge only the author has.** Look for references to things the
package doesn't contain: "the approach we discussed", "like my local
fix", "the helper I wrote earlier", "same pattern as last time", a
branch nobody else can see, or a conversation with a classmate.

### What good looks like

Break the approach into its edits, one line each. For every edit that
changes code, write down two things: where (a file, module or
function, or a place the issue or repo-facts block pins down) and what
(the change itself, specific enough to type). An edit with both filled
in is executable. Leave test edits to `test-plan-decisive`.

"In `parse_date()`, when the input has no `tz` key, set it to UTC
before building the datetime" is executable, because a stranger can
open the file and make that edit. These aren't:

- "Fix the parser." There's a location but no change.
- "Improve error handling." There's neither.
- "Handle the timezone edge case properly." It doesn't say which case
  or what "properly" means.
- "Apply the fix from my branch." It depends on something only the
  author has.

The check fails when any code edit is missing its where or its what,
or when any step leans on author-only knowledge. Order of work only
matters when one step depends on another. If step 2 needs something
step 3 creates, a stranger following the plan in order gets stuck at
step 2. That counts as a step they can't start, and it fails the
check.

If the plan comment describes a different approach from the plan,
grade the plan here and leave the mismatch to
`unknowns-stated-honestly`.

## Test plan

This family feeds `test-plan-decisive`. It checks whether the plan
says how anyone will know the bug is gone, using the same input and
the same observable the repro used.

### Where it lives

**The plan's checks.** In eval mode, read the candidate plan's test
plan, usually called "Test plan", "Testing", "Verification" or "How
I'll know it's fixed". Also scan the approach for steps like "add a
test that...", since some plans put their test there. In live mode,
look in the same places in the student's draft `plan.md`. Testing
promises that only appear in the plan comment don't count.

**The trigger and the artifact.** These come from the repro evidence,
found where the Diagnosis section says to look. The trigger is the
input or steps that make the bug happen. The artifact is what showed
it: the error, the wrong output, the bad value. Copy both down word
for word.

### What good looks like

For each check in the test plan, write one line with four parts: the
input, the outcome it looks for, what that outcome is on current code,
and what it is after the fix. The repro is what tells you the current
result. It already ran this input on current code and got the
artifact, so a check that looks for the artifact to be gone fails
today. If a check uses a different input, you can't know what it does
today, and it doesn't count.

The check passes when at least one line has the repro's input (or a
re-run of the repro steps), names an outcome you could see or assert,
fails now and passes after the fix. Two that pass:

- "Add `test_parse_date_without_tz`, which calls
  `parse_date("2024-01-01T00:00:00")` (the input from repro step 2)
  and asserts it returns a datetime with UTC tzinfo instead of raising
  `KeyError: 'tz'`."
- "Re-run repro steps 1 to 3 and expect `2024-01-01 00:00 UTC` to be
  printed where the traceback was."

These don't pass:

- "Run the test suite", "verify it works" or "make sure nothing
  breaks". None of them names an outcome.
- "Add a test that `parse_date()` handles timestamps with a timezone."
  That already passes on current code.
- "Test that the new `--tz` flag works." That's a different behavior
  from the reported bug.

Vague items next to a decisive one don't fail the check. "Run the full
suite to catch regressions" is fine as long as one line passes on its
own.

A check that only asserts "doesn't raise" passes when the repro's
artifact was that exception. Say in the summary that it's weak,
though. A symptom fix that swallows the exception would pass it too,
and asserting the returned value would catch that.

If the package quotes no repro evidence, there's no trigger or
artifact to tie a check to, so grade this `unclear`. The rubric doesn't
fix this reading the way it does for the diagnosis checks, but it's
the same logic: a test plan can't match a repro that isn't there.

## Honesty

This family feeds `unknowns-stated-honestly`. It checks every claim
the package makes against what the package can back up. It's also
where the earlier sections send their mismatches between the plan and
the plan comment.

### Where it lives

**The claims.** They can be anywhere, so read the whole candidate plan
and the whole plan comment. Most land in a few predictable spots: the
diagnosis ("this is the only caller"), the approach ("no other
behavior changes"), a "Risks", "Unknowns", "Open questions" or
"Assumptions" section, and the comment's last paragraph, where
effort and timeline promises show up ("PR by Friday"). In live mode,
look in the student's draft `plan.md` and draft comment.

**What can back them up.** Only the package's own evidence: in eval
mode, the issue context, the repro-evidence block and the repo-facts
block. In live mode, it's the issue thread, the student's posted
repro comment and the repo's docs. Don't go and check a claim yourself
by searching the repo. A claim that happens to be true but isn't
evidenced in the package still has to be marked as unconfirmed.

**Deviations.** In live mode, after the build has started, look for a
"Deviations" or "What changed" section in `plan.md`. It should say
what changed from the posted plan and why. Its claims get read like
any others.

### What good looks like

List every claim about cause, side effects, other callers,
compatibility, effort or timeline, one line each. For each, mark it
evidenced (quote the evidence), hedged (quote the hedge), or stated
as fact with nothing behind it. One line in the last group fails the
check.

A hedge has to be visible in the sentence. "I think", "likely", "I
haven't confirmed whether", "Open question:" and "assuming X" all
count. Words that signal confidence the package can't back up:
"only", "no other", "won't affect", "always", "never", "just a
one-line fix", "will be done by". Effort and timeline phrased as an
intent or an estimate ("I'm aiming to open the PR this week") is
hedged. Phrased as a promise ("PR by Friday"), it's a fact claim with
no evidence.

A narrowly stated operational consequence can be evidenced by direct inference from the package’s stated mechanism and evidence. For example, limiting a response cache to an existing 24-hour interval supports “one extra request per channel per 24 hours.” “No risks identified beyond [that specific consequence]” does not claim that no other risk can exist; it records the risk identified from the available evidence.

A stated risk can also be an honest open question when the plan names the behavior it has not confirmed and commits to checking a specific existing reference before implementing or testing it. For example, “I will match whatever the leaf level does for out-of-range negative indices today and add a test” is hedged: it promises investigation and consistency, not a currently unproven outcome.

"`parse_date()` is called from two places in the repo-facts block's
module list, and I haven't checked whether either one passes
timezone-free input" passes. "This is the only caller, so nothing
else changes" fails unless the package shows that.

A plan with no "Unknowns" section isn't automatically dishonest. It
passes if every claim it makes is evidenced. A long "Risks" section
doesn't save a plan that states something unevidenced as fact
somewhere else.

Then compare the plan comment to the plan. Fail it if the comment
promises anything the plan doesn't contain, or states a cause,
approach or test the plan doesn't. Those are the mismatches the
Diagnosis, Scope, Executability and Test plan sections passed along.
A comment that says less than the plan is fine.

A deviation recorded in `plan.md` with a reason is honest work and
passes on its own terms. If the plan comment still describes the old
approach after the plan records a deviation, that's a mismatch, and
it fails.

## Comms

This family feeds two checks, `thread-aware` and
`repo-policy-respected`. Both read the plan comment and the plan
against what the people running the repo have already said, first in
the thread and then in the repo's written rules. The voice guide isn't
part of this family. Report what it finds separately, and never let it
change a grade here.

### Where it lives

**Maintainer signals.** In eval mode, these are the thread highlights
in the issue context: maintainer comments, labels, and any direction
someone suggested or ruled out. In live mode, read the whole issue
thread on GitHub. To tell a maintainer from everyone else, use the
author association on each comment (`gh api
repos/<owner>/<repo>/issues/<n>/comments` returns it).
`OWNER`, `MEMBER` and `COLLABORATOR` are maintainers. In Path Review
that means staff. Classmates show up as `CONTRIBUTOR` or `NONE`, and
their comments are not maintainer signals.

**Labels.** Only count a label as a signal when it limits the
approach, like "needs discussion", "docs only" or "no new
dependencies". Labels such as "bug" or "good first issue" don't
point anywhere.

**The repo's rules.** In eval mode, this is the repo-facts block:
contributing asks, comment or PR templates, contribution policy and
AI-use disclosure policy. In live mode, read `CONTRIBUTING.md`, the
templates under `.github/`, any AI-use policy file the repo has, and
the house rules in `scope.md`.

**The disclosure line.** Look for it in both the plan and the plan
comment. The rubric only asks that the package carry one.

**The branch name.** After build begins in live mode, inspect the student's fork for the branch. Before build, no branch is required for plan readiness.

### What good looks like

For `thread-aware`, list every direction a maintainer suggested or ruled out, one line each with its quote. Then compare the plan’s approach to each direction. The plan passes when its approach follows the direction, even without quoting it, or explicitly explains why it departs and what it will do instead. It fails when it conflicts with or ignores a direction, including an approach the thread ruled out.

If no maintainer has given a direction, the check passes with the
evidence `no maintainer direction in thread`. Don't grade that
`unclear`.

Thread-aware looks like "@maintainer suggested handling this in the
serializer. I'm fixing it in `parse_date()` instead, because the
traceback shows the bad dict is built there, before the serializer
runs." Boilerplate looks like a comment that would read the same on
any issue, and it fails when the thread had a direction to answer.
"Same approach as above" fails as piggybacking. A classmate posting
their own plan on the same issue doesn't fail anything (house rule,
`scope.md`).

For `repo-policy-respected`, list every rule from the repo's rules
that applies to a plan comment or to the planned work. A
"discuss before opening a PR" rule applies now. A PR title format
doesn't apply until week 4. Mark each rule met (with a quote) or
broken. One broken rule fails the check. Tone, length and phrasing
are out of scope here.

AI-use disclosure is present or absent, never `unclear`. Where the
repo requires it, the package needs a line that names the tool and
says how much it helped, such as "I used Claude Code to help draft
this plan and comment. The repro and diagnosis are my own."
"AI-assisted" with no tool named fails, and so does a tool named with
no extent. If the rules say nothing about AI, this part doesn't apply.

After the build begins, when this check runs live against the student's fork and a branch is available, it must be named `<type>/<issue-number>-<slug>`, where `<type>` is one of `fix`, `docs`, `feat`, `test`, `refactor`, `perf`, or `chore`. For example, `fix/42-parse-date-missing-tz` passes and `my-fix` fails. Before a branch is created, its absence does not affect initial plan readiness. In eval mode, skip the branch rule.
