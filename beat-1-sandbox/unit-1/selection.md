# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69

**Verdict output**

````
All three URLs are in `codepath/pathreview-ai301-fa26-s3`, the repo set in `scope.md`, so all
three are in-scope candidates. Graded each independently against the rubric, then ranked the
accepted ones by the fit profile.

Repo facts (measured against today, 2026-09-20): not archived; 2 stars; last push 2026-09-16
(4 days ago); last 5 default-branch commits all by `Aburke225`, a human COLLABORATOR, dated
2026-09-16 x3 and 2026-08-24 x2 (largest gap 23 days); no releases at all; maintainer response
sample — `Aburke225` replied on #52 and #43 at 6 days each.

Policy: there is no `CONTRIBUTING.md` at the root, but `docs/CONTRIBUTING.md` exists — the "one
click away" case the evidence guide warns about. Read it in full plus
`.github/PULL_REQUEST_TEMPLATE.md`: neither mentions AI or generative tooling anywhere. Silence,
so `policy-compatible` passes for all three rather than being graded `unclear`.

Ranked read-out — all three accepted:

1. #69 — Output parser crashes on a top-level JSON array (3/3 preferred)
Best fit. It puts you in a subsystem you haven't touched (the RAG generator) while the work
itself is plain Python `list`/`dict` handling in your strongest language. The bug is named
precisely (`AttributeError: 'list' object has no attribute 'items'`), and an `xfail`-marked test
already specifies "done" — you don't have to decide what correct looks like. That is exploration
without pre-existing expertise, which is the fit profile almost exactly.

2. #58 — Bias detector patterns too narrow (2/3 preferred — no `good first issue` label)
New territory in the safety module, and nine named failing tests pin the target, so you are not
making independent judgment calls about what counts as bias. Caveat worth naming against the "no
specialized domain knowledge" line: generalizing regexes over bias phrasings is the most
domain-flavored task of the three, and it is the only candidate the staff did not label
`good first issue`.

3. #73 — README and `.env.example` disagree (3/3 preferred)
The safest and quickest (staff estimate 1–2 hours) and fully specified — `core/config.py` defines
both keys, so it is a two-file reconciliation. Ranked last only because it is docs/config and
breaks the least new ground for someone who asked to explore.

Two things to know: the Path Review house rule (classmates' claims do not block) never had to be
applied — all three issues have zero comments, no assignees, and no linked PRs, so they are
unclaimed by any standard. And per `docs/CONTRIBUTING.md`, these are deliberately seeded bugs:
fixing one means also deleting its `@pytest.mark.xfail` marker, or CI fails with `XPASS(strict)`.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 default-branch commits by human COLLABORATOR Aburke225, most recent 2026-09-16 (4 days before today)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: no; no releases, but last push 2026-09-16 is within 180 days; largest gap among last 5 commits is 23 days (2026-08-24 to 2026-09-16)"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single named defect in output_parser.py with a stated exception and an xfail-marked covering test; opened by COLLABORATOR Aburke225, no debate or umbrella framing"},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees: none; timeline shows only 4 'labeled' events, no linked PRs; 0 comments"},
      {"name": "policy-compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no statement on AI or generative tooling - silence passes"},
      {"name": "newcomer-labeled", "grade": "pass", "evidence": "labels: bug, good first issue, rag, tier-1"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) replied on #52 and #43 at 6 days each, within roughly 2 weeks"},
      {"name": "low-competition", "grade": "pass", "evidence": "repo has 2 stars, below the 5,000 threshold"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/58",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 default-branch commits by human COLLABORATOR Aburke225, most recent 2026-09-16 (4 days before today)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: no; no releases, but last push 2026-09-16 is within 180 days; largest gap among last 5 commits is 23 days"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One file (safety/bias_detector.py) with repro steps and 9 named failing tests specifying the intended coverage; opened by COLLABORATOR Aburke225, so the desired behavior is settled, not an undecided product call"},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees: none; timeline shows only 3 'labeled' events, no linked PRs; 0 comments"},
      {"name": "policy-compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no statement on AI or generative tooling - silence passes"},
      {"name": "newcomer-labeled", "grade": "fail", "evidence": "labels: bug, safety, tier-1 - no good-first-issue or help-wanted"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) replied on #52 and #43 at 6 days each, within roughly 2 weeks"},
      {"name": "low-competition", "grade": "pass", "evidence": "repo has 2 stars, below the 5,000 threshold"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last 5 default-branch commits by human COLLABORATOR Aburke225, most recent 2026-09-16 (4 days before today)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: no; no releases, but last push 2026-09-16 is within 180 days; largest gap among last 5 commits is 23 days"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Two named files (README.md, .env.example) reconciled against core/config.py, staff-estimated 1-2 hours; opened by COLLABORATOR Aburke225"},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees: none; timeline shows only 4 'labeled' events, no linked PRs; 0 comments"},
      {"name": "policy-compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no statement on AI or generative tooling - silence passes"},
      {"name": "newcomer-labeled", "grade": "pass", "evidence": "labels: bug, good first issue, docs, tier-1"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) replied on #52 and #43 at 6 days each, within roughly 2 weeks"},
      {"name": "low-competition", "grade": "pass", "evidence": "repo has 2 stars, below the 5,000 threshold"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

Five runs, in order:

1. `--only issue-12,issue-02,issue-01` — `agreement: 2/3 scored items`
2. `--only issue-01,issue-05` — `agreement: 1/2 scored items`
3. `--only issue-01,issue-10,issue-20` — `agreement: 2/3 scored items`
4. `--only issue-20,issue-01,issue-09` — `agreement: 3/3 scored items`
5. Full 20-item run, written with `--save-run eval-run.txt` — `agreement: 19/20 scored items  (bar: 18/20: PASS)`

The final score matches the agreement line in the committed `eval-run.txt` exactly.

**Issue analysis**

`issue-18`. My rubric returned `accept`; the gold label is `reject`.

The decision came from the `not-claimed` check. The bundle's sidebar showed open linked PRs,
which is an active claim and should have failed the check. But the thread also contained
unanswered "can I work on this" comments with no maintainer reply, and my check ends with a
tie-break: "If the sidebar and the thread disagree, the thread wins." The grader applied that
rule symmetrically and used the unanswered thread comments to override the sidebar, grading the
check `pass` with the evidence line: "Sidebar shows open linked PRs, but thread shows unanswered
'can I work on this' asks as recently as 2026-08-02 (vishnukumar650) with no confirmed active
claim; thread wins per rubric tie-break."

Every other required check passed on this issue, so the single wrong `pass` on `not-claimed`
carried the whole verdict to `accept`.

**Check rationale**

The `not-claimed` row as it is currently written in `tools/issue-select/rubric.md`:

> | not-claimed | Assignees, linked PR state, comment thread, label-event dates | No current assignee, no open linked PR actively addressing it, no other contributor's standing claim comment. If the sidebar and the thread disagree, the thread wins. | required |

The tie-break sentence comes from the evidence guide, which notes that "Not every PR gets
formally linked; people often just mention their PR in the comments, so read the thread too, and
when the sidebar and the thread disagree, believe the thread." The intent was one-directional:
the thread is there to *catch claims the sidebar missed* — a PR someone mentions in a comment but
never formally links, or a "working on this" note with no assignee attached.

The grader read it symmetrically instead, and let the thread *subtract* a claim rather than only
add one: an unanswered comment asking to take the issue was treated as evidence that the open
linked PR was not a real claim. That inverts the signal, because a formally linked open PR is the
strongest claim evidence available — stronger than anything in the thread.

**Trade-offs**

Tightening the tie-break so the thread can only add claims and never subtract them would fix
`issue-18`. It was left as written because of the risk to `issue-09`, which the current wording
handles correctly: that issue carries a claim comment from 2022 that the gold label calls stale,
with the maintainer having since invited takers, and my `not-claimed` check correctly passed it
on the "standing claim" reading. A stricter rule that refuses to discount thread evidence could
flip `issue-09` from a correct `accept` to a wrong `reject`, trading one error for another.

With the full run already at `19/20` and above the `18/20` bar with the category floor met, the
cost of being wrong was higher than the value of the one point: re-running the full set costs
about $4, and a worse result would overwrite the saved `eval-run.txt`. The known miss is recorded
here instead.

---

## Selection rationale

**Selection rationale**

1. **Fit to interests and time available.** I picked #69 because it puts me in the RAG
   subsystem, which I have not worked in before, while the fix itself is plain Python — the
   language I am most comfortable in. My fit profile says I want to explore unfamiliar areas
   rather than stick to what I already know, and this is unfamiliar territory that does not
   demand pre-existing expertise to enter. The bug is a named `AttributeError` on a top-level
   JSON array with an `xfail`-marked test already covering it, so "done" is defined for me and
   the work fits the time I have.

2. **What the verdict identified, and what I weighed that the rubric could not.** The skill
   confirmed the things I could not have checked reliably by eye: that the issue is unclaimed
   (no assignee, no linked PRs, zero comments), that the repo's contribution policy is
   compatible with AI-assisted work (there is no `CONTRIBUTING.md` at the root — the real one is
   at `docs/CONTRIBUTING.md`, and neither it nor the PR template mentions AI at all), and that
   the scope is bounded to one file plus its test. What the rubric could not decide was the
   ranking: all three candidates were accepted, and #69 came out above #58 and #73 because of my
   fit profile's lean toward unfamiliar territory, not because of any check. Fit orders accepted
   issues; it never changes a verdict.

3. **Anticipated difficulty in claiming it.** Low. All three candidates had zero comments, no
   assignees, and no linked PRs, so nobody is ahead of me on any of them. The Path Review house
   rule means classmates' claim comments would not block me even if they appeared, and course
   credit attaches to the pull request I open rather than to whether it merges.

---

Related paths: `eval-run.txt` in this directory; the skill's files in `tools/issue-select/`.
