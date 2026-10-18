# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/220

**Branch**

docs/73-align-openrouter-env-guidance

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 13/20; clear-accept category had 0/7 matches.
2. Targeted retry (pkg-02, pkg-01, pkg-04): 3/3.
3. Remaining six disagreements: 2/6.
4. Control-test retry (pkg-05, pkg-04): 1/2.
5. Repository-check retry (pkg-05, pkg-04, pkg-01): 3/3.
6. Control-report retry (pkg-13, pkg-19, pkg-04): 2/3.
7. Control-consistency retry (pkg-19, pkg-13, pkg-04): 2/3.
8. Single-package retry (pkg-08): 1/1.
9. Final full run: 20/20, with all categories matched.

Targeted scores cover only the listed packages, not the full evaluation.
The final eval-run.txt reports:
“agreement: 20/20 scored items (bar: 18/20: PASS)”

**Package analysis**

I chose pkg-02. My first full run graded it reject, while the gold
label was accept. It failed tests-evidenced and repo-policy-respected.

The package reported "`pytest tests/ -q` passes (1462 passed, 12 skipped)."
My original rule rejected this because it was a concise report rather
than pasted terminal output, even though it named the command and
gave a specific result. The package also included before/after
reproduction output showing that the recorded text survived a failed
save.

The repository facts said "no stated AI policy", but my rubric still
required the course's AI disclosure in eval mode. I had applied a
requirement that did not belong to this package.

I revised the rules to accept named test runs with concrete results
and to apply the repository requirements stated in each eval bundle.
The repro still needed observable before/after evidence. After these
changes, pkg-02 received accept in the targeted retry and the final
full run, matching the gold label.

**Check rationale**

I chose `diff-reviewable`, a required check. Its pass condition reads:

> Passes if the diff contains no noise. Noise means: added debug output or hooks (print or log statements added for debugging, `debugger`, breakpoints); commented-out code, whether added or left behind; formatting, whitespace, or import-order changes on lines the change does not otherwise touch; and files unrelated to the change, such as editor or OS files, local config, build output, or lockfile changes. Lockfile changes are exempt if the plan or a Deviations entry calls for a dependency change. Fails on any single instance; quote the hunk as evidence. The number of files or lines is not graded.

I kept this check focused on specific things a reviewer can point to
in the diff. A large change can still be focused, while a small change
can contain unrelated files or debugging leftovers. The lockfile
exception allows dependency changes when the plan explains them.

During review, I clarified the evidence guide so removing debug code
does not count as adding noise. A debug commit that was later undone
also should not fail this check when the final diff contains no debug
code. The final submitted diff determines the grade.

**Trade-offs**

Accepting concise test reports makes the rubric less strict about
how evidence is presented. A named check and its reported result can
count without a full terminal log. That also means the grader has
less detail to inspect, so I kept the requirement for observable
before/after evidence of the reported bug.

I used pkg-04 as a canary while loosening the test-evidence rules.
It continued to receive reject because its testing statement lacked
the required repro evidence. In the retry saved as retry-checks.json,
pkg-05 changed to accept while pkg-04 and pkg-01 remained reject.
That gave me evidence that the revised rules could recognize a
supported test report while still catching missing evidence and
repository requirements.

There was still some variability. pkg-19 received reject in targeted
retries but accept in the final full run without another rule change
between the last retry and that run. The final result was 20/20,
but that disagreement shows the grader can interpret a brief control
report differently across runs.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
