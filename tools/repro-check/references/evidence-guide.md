# Evidence guide: where proof lives in a reproduction package

This is the map the rubric's checks read. For each family: where to look
in an eval bundle, where to look in live mode, and what good looks like
when you get there.

Two rules that apply everywhere:

- **The package is what the drafts contain or quote — plus the issue they
  answer.** Other files in the working directory are not evidence, and
  neither is anything that lives only in your head. But the issue body is
  on the reader's screen next to your comment, so a report may build on
  what the issue already supplies. Grade what a reader of that thread
  would see.
- **Live drafts are markdown files in the working directory** (e.g.
  `claim.md`, `repro.md`, or similarly named drafts). Read the draft file
  as the candidate comment. In eval mode, the bundle's candidate sections
  are the drafts and nothing is fetched.

## Environment

**Where it lives.** _Eval bundle:_ the environment record inside the
candidate repro report — usually a block near the top listing OS,
runtime, and versions; the target to reconcile against is in the issue
context (the issue body, often under an "Environment"/"Version" template
field) and the repo-facts block. _Live:_ the same record in the repro
draft file; the target comes from the issue body on GitHub, and the
repo's current version from the repo itself (release tag, `package.json`,
lockfile, or default branch).

**What good looks like.** Three concrete values are present: the OS, the
language/runtime version, and the version of the package or repo under
test (a release tag, a commit SHA, or a branch plus date — "latest"
alone is not a value). Those values either match the target the issue
states, or the report names the difference out loud ("issue reports 3.11;
I ran 3.12 and still see it"). **If the issue states no target at all**,
there is nothing to reconcile: a concrete recorded environment is
sufficient, and the mismatch clause does not apply. Do not fail a package
for a gap the issue's own author left.

## Steps

**Where it lives.** _Eval bundle:_ the numbered or bulleted reproduction
steps in the candidate repro report, plus any setup the report describes
in prose before them. _Live:_ the same in the repro draft file; the
issue's own steps (if it has any) are the comparison point, in the issue
body.

**What good looks like.** Read them the way a reader of the thread does:
the report **plus the issue it answers**, both on screen. The steps state
a starting state (which repo, which branch or version, what was
installed), then carry through to the trigger with every command, file,
input, and config change the run actually depended on. The test: could
someone on that thread land on the same outcome without asking you a
question? A step that says "set up the project as usual" or that relies
on a data file, environment variable, or config that appears **nowhere in
the report or the issue** is a gap. Length is not the measure — three
complete steps beat twelve vague ones.

**Reuse by reference is complete, not lazy.** When the issue already
carries a runnable reproduction, a report that says "ran the issue's
script verbatim" and then names the parameters it used (versions, offsets,
options, which shapes) has supplied everything: the reference resolves to
one unambiguous thing, and re-pasting the reporter's own input adds
nothing. Do not fail such a report for lacking a verbatim copy. The
reference fails only when it is ambiguous — "used the repro from the
issue" where the issue has several, with no indication which, or where
the report silently changed a parameter it never names.

## Behavior shown

**Where it lives.** _Eval bundle:_ the artifacts in the candidate repro
report — fenced output blocks, stack traces, log excerpts, test output,
or a described screenshot; read them against the error or symptom stated
in the issue context (the issue body, and any follow-up comment in the
thread that sharpens it). _Live:_ the artifacts quoted in the repro draft
file, against the issue body on GitHub.

**What counts as an artifact.** Three forms:

- **Verbatim output in a fenced block** — terminal output, stack trace,
  or log lines copied as-is. This is the primary form.
- **A screenshot or recording whose content the report describes** — for
  UI and visual issues, where the description makes the claim checkable
  in text. In an eval bundle, the description is the evidence; live, the
  image link in the draft plus its description.
- **A failing test run with its output quoted** — a test the reporter
  wrote or ran that fails on the issue's behavior.

A paraphrase is not an artifact. "It threw a type error" is prose; the
type error's text is evidence. Bisect output, `git log`, and traces are
_cause_ evidence — they belong to Honesty, not here.

**What good looks like.** The artifact carries the issue's specific
signature: the same error message, exception type, status code, or
described symptom the issue reports — not merely a failure somewhere in
the same subsystem. Matching the area while missing the signature is the
classic adjacent-bug repro, and it is the thing this family exists to
catch. **Cannot-reproduce:** the report shows the artifact of what
happened _instead_ — the command completing, the expected output, the
test passing — under the issue's steps and a reconciled environment. That
is a real result with real evidence, and it reads as good here.

## Honesty

**Where it lives.** Everywhere claims and backing meet: each assertion in
the candidate claim comment and repro report, read against the artifacts
that same package shows. _Eval bundle:_ both candidate sections against
the report's own artifacts. _Live:_ the claim and repro draft files
against their own quoted artifacts. The backing must be **inside the
package** — a claim supported only by something in your working directory
or in your head is unbacked to the reader.

**What good looks like.** Apply the trace test: take each assertion the
package makes and point at the line of the package that shows it. An
assertion with nothing to point at is an overclaim, however measured its
tone. The recurring shapes:

- **Cause stated as fact** — "this is caused by the cache layer" with no
  trace, bisect, or experiment behind it. A hypothesis is fine when it is
  marked as one.
- **Untested scope** — "affects all versions", "happens on every
  platform", when one environment was run.
- **Effort claims** — "should be a simple fix", from someone who has not
  changed the code.
- **A confirmed reproduction with no matching artifact** — the strongest
  version of the failure, and the one this family shares with Behavior.

An honest, evidenced cannot-reproduce passes: stating exactly what
happened is the behavior this check rewards. Do not read confidence as
dishonesty or hedging as rigor — a flat statement backed by a quoted
artifact is honest, and a hedged claim with nothing behind it is not.

## Comms

**Where it lives.** _Eval bundle:_ the repo's stated rules in the
repo-facts block, read against the candidate claim comment and repro
report. _Live:_ `CONTRIBUTING.md` (and any doc it links for issue
participation), an AI-use or AI-disclosure policy wherever the repo keeps
it, and required fields in `.github/ISSUE_TEMPLATE` or a comment
template — read against the draft files. Also read `scope.md` for house
rules in effect.

**Three sources count as stated rules**, and only these can fail a
package: the repo-facts block / `CONTRIBUTING`, an AI-use disclosure
policy, and required fields in a `.github` issue or comment template.
Optional template sections never fail the check. If a rule is not stated
somewhere the package can be checked against, it is not a rule here.

**What good looks like.** Every applicable stated rule is satisfied: a
required AI-use disclosure is present and truthful; a required template's
required fields are filled with real content rather than left as
placeholder text. Tone, length, and phrasing are explicitly out of scope
for the policy check — a blunt comment that follows the rules passes.

**Reading a disclosure requirement.** This is the rule most often missed,
because it is conditional and the package text cannot prove AI was used.
Read it as present-or-absent anyway: a package that reaches this skill is
AI-assisted, so where the repo requires disclosure, a package with no
disclosure line fails — it is not `unclear`, and the grader does not get
to reason "I cannot verify that AI was involved." **This bites only where
the repo actually requires disclosure.** A policy that constrains AI
output without mandating disclosure — "only submit code you fully
understand and have tested", "low-quality AI content is closed
immediately" (Prettier's shape) — is not a disclosure requirement, and a
package without a disclosure line passes under it. Read the policy for
what it obliges, not for whether it mentions AI. Where the policy also
states *what* to disclose (Ghostty's AI_POLICY, for instance, asks for
the tool used and the extent of the assistance), a bare "AI was used
here" does not satisfy it; the named elements have to be there. Look for
the disclosure in both candidate comments — a repro report that discloses
does not cover a claim comment that does not, if the policy covers all
comments.

**Specificity (preferred, never changes the verdict).** Good comments say
things only someone who actually ran this could say: this issue's
commands, paths, values, timings, observations. Boilerplate is text that
would read identically pasted onto any other issue — "I'd love to work on
this, please assign me", "Thanks for the great project!" — and adds
nothing a maintainer can use.

**House rules (live, Path Review).** Per `scope.md`, a classmate's claim
comment does **not** make an issue claimed; several students claiming the
same issue is expected, and a package is not held for arriving second.
What the house rule does not excuse is piggybacking: "same as above, can
confirm" is not a reproduction, and a claim that only restates the issue
title is not a claim in the claimant's own words.
