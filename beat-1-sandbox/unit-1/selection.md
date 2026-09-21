# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60

**Verdict output**

Issue #60: Faithfulness checker crashes when a context chunk has text: None

Check 1: Active maintainer
- Grade: PASS
- Evidence: Commits from Sept 16, 2026 by Aburke225 (human-authored), within 90 days

Check 2: Repository in use
- Grade: PASS
- Evidence: Repository not archived; last push Sept 16, 2026; active development within 180 days

Check 3: Manageable scope
- Grade: PASS
- Evidence: Handle None values in context chunk .get() call—one clearly bounded fix

Check 4: Issue available
- Grade: PASS
- Evidence: No assignee, no open PR, no claims within past 30 days

Check 5: Compatible contribution policy
- Grade: PASS
- Evidence: No explicit ban on AI-assisted contributions in CONTRIBUTING.md

```json

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
  "checks": [
    {"name": "Active maintainer", "grade": "pass", "evidence": "Commits Sept 16 by Aburke225 within 90 days"},
    {"name": "Repository in use", "grade": "pass", "evidence": "Last push Sept 16, 2026; not archived"},
    {"name": "Manageable scope", "grade": "pass", "evidence": "Handle None values in chunk.get()—single bounded fix"},
    {"name": "Issue available", "grade": "pass", "evidence": "No assignee, no open PR, no claims within 30 days"},
    {"name": "Compatible contribution policy", "grade": "pass", "evidence": "No AI ban in CONTRIBUTING.md"}
  ],
  "verdict": "accept"
}

```
---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Initial three-issue smoke test: 2/3.
2. Rerun of issue-01 after the first scope clarification: 1/1.
3. First full evaluation: 17/20.
4. Rerun of issue-01 and issue-04: 1/2; issue-01 rejected, issue-04 accepted.
5. Diagnostic rerun of issue-01, saved to results.json: 0/1.
6. Rerun of issue-01 and issue-04 after clarifying rejection conditions: 2/2.
7. Rerun of issue-01, issue-04, issue-15, and issue-19 after adding the abandoned-attempts rule: 3/4.
8. Rerun of those four issues after distinguishing required work from suggestions: 4/4.
9. Final full evaluation: "agreement: 20/20 scored items  (bar: 18/20: PASS)"

**Issue analysis**

I analyzed issue-01. In the first full run, my rubric returned reject while the gold label was accept. The diagnostic run described the listed documentation work as “explicit evidence of excessive scope.”

The edits affected several pages, but they all supported the same documentation goal. I clarified that related edits across multiple files should not fail the scope check just because there are several of them.

In the final full run, my rubric returned accept, matching the gold label.

**Check rationale**
```text
| Manageable scope | Issue body and comment thread. | The contribution has one clearly defined, bounded outcome. Coordinated edits across multiple files or documentation pages can pass when they serve that same outcome; file count or a checklist alone does not make an issue an umbrella task. Several related instances of the same change can count as one bounded task. For terse issues, maintainer authorship and a good-first-issue label support a bounded-scope interpretation, unless the body or thread shows one of the rejection conditions listed below. The number of documentation topics, related files, or optional supporting edits alone is not a rejection condition. Distinguish explicitly required changes from additional suggestions; suggestions alone do not expand the mandatory scope unless the body or thread makes them requirements. Multiple causes of one reported bug do not automatically make it an umbrella issue. A good-first-issue label is supporting evidence, not a requirement; its absence alone does not cause rejection. Reject umbrella/tracking issues containing independent tasks intended to be split, unresolved design debates affecting the requested work, maintainer-confirmed core-internals changes, and pure usage questions. A short description or missing reproduction steps alone does not cause failure. Also reject an issue open for at least 2 years when the supplied history documents at least 2 separate attempts abandoned or ended for inactivity. Age alone does not cause failure, and a PR marked closed without evidence of its outcome does not by itself count as an abandoned attempt. | required |

```

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
