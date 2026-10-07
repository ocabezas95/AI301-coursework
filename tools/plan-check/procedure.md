# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read `scope.md` to learn the allowed repository and rules.
2. Read `rubric.md`, the evidence guide, and the voice guide to learn how to judge.
3. Read the issue and reproduction information.
4. Read the candidate `plan.md`
5. Read the candidate `comment.md`

## Evidence gathering

1. For each rubric check, consult the evidence guide and identify its required evidence source before evaluating the check.
2. In eval mode, gather evidence only from the supplied package: the issue/repro text, `plan.md`, `comment.md`, and any explicitly included repository excerpts.
3. In live mode, inspect the exact issue-thread locations, repository files, and reproduction steps named by the evidence guide; do not substitute nearby or inferred evidence.
4. Record one evidence entry per rubric check containing:
   - the source and location,
   - the exact quote, observed behavior, or command result,
   - the requirement or behavior it establishes,
   - whether it is direct evidence or an inference.
5. If the required evidence is absent, record it as “not established” rather than filling the gap with assumptions. If sources conflict, record both and defer resolution to check execution.
6. Do not grade a check while gathering evidence; complete the evidence record first, then execute the checks against that record.

## Check execution

1. Evaluate rubric checks in the order they appear in `rubric.md`; do not skip a check because another check already determines the likely verdict.
2. For each check, restate the check’s requirement and compare it with the evidence gathered for that check.
3. Apply the rubric’s stated `pass`, `fail`, and `unclear` criteria literally. Do not award credit for intent, implication, or evidence belonging to another check.
4. When required evidence is absent, record the evidence as “not established” and assign the grade `unclear` when the rubric calls for it; do not infer compliance from silence.
5. When evidence conflicts, preserve the conflict unless the evidence guide defines how to resolve it. If the evidence guide does not define a resolution, assign the grade `unclear`.
6. Record the result for every check with:
   - the grade,
   - the evidence entry or entries used,
   - a concise rationale,
   - any unresolved uncertainty.
7. Grade against the gathered evidence record. Re-read the package only when the evidence record points to a genuine ambiguity or a source that was not fully captured.

## Verdict assembly

1. After every rubric check has been graded, apply the verdict rule to the complete set of check results.
2. Return `accept` only when every `required` check has the grade `pass`.
3. Return `reject` when any `required` check has the grade `fail` or `unclear`.
4. Report every `required` check graded `fail` or `unclear`; do not stop after reporting the first deciding check.
5. Exclude `preferred` checks from the final verdict. They may be reported separately, but their grades must not change `accept` or `reject`.
6. For each reported failed or unclear required check, include the check identifier, its grade, the supporting evidence entry or entries, and a concise rationale.
