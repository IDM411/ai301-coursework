# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Last 5 default-branch commit dates and author names (Repo facts) | At least 1 commit within 90 days of the bundle's capture date, authored or merged by a human — not solely automated dependency-bot commits (e.g. Dependabot, Renovate) with no human author in the last 5. | required |
| repo-in-use | Archived flag; latest release date; last push date; last 5 default-branch commit dates (Repo facts) | Not archived, AND (release within 365 days of capture date OR push within 180 days of capture date), AND no gap of 4+ months between any two consecutive commits among the last 5. | required |
| bounded-scope | Issue body + full comment thread | Fails only if one of: the issue is a tracking/umbrella whose sub-items are meant to be taken as separate work by different contributors (signals: explicit "tracking issue" framing, a checklist of independently claimable items, or a thread where contributors are splitting parts off into their own PRs); the thread shows a design debate no maintainer has settled; a maintainer says the fix touches core internals; it is a pure usage/support question; or it requests new user-facing behavior whose essential inputs or product decisions are still undecided (signals: a required asset, name, or design marked TBD/undecided; the opener is not a maintainer and no maintainer has endorsed the request; the outcome is stated as a wish rather than a specification) — implementing or documenting a decision the project has already made is not this. Otherwise passes. A detailed proposal that lists several files to change or breaks its plan into headed subsections is still one deliverable — an implementation outline is a sign of a well-specified issue, not an umbrella. Judge whether one contributor would naturally do all of it in one PR, not the file count, the number of subsections, or the length of the writeup. | required |
| not-claimed | Assignees, linked PR state, comment thread, label-event dates | No current assignee, no open linked PR actively addressing it, no other contributor's standing claim comment. If the sidebar and the thread disagree, the thread wins. | required |
| policy-compatible | Repo facts "contribution policy" line; CONTRIBUTING.md / AI_POLICY.md; PR/issue template disclosure checkboxes | Fails only on an outright ban on AI-assisted contribution. Disclosure/review/testing conditions are terms to follow, not a reject. Silence (no policy found) passes. An AGENTS.md file is a positive signal, not a ban. | required |
| newcomer-labeled | `good-first-issue` / `help-wanted` label presence | Label is present. (Signals maintainer intent, not availability — never a substitute for `not-claimed`.) | preferred |
| maintainer-responsive | Maintainer first-response latency; author_association on comments (Repo facts) | Maintainer (Owner/Member/Collaborator) has replied to at least one recent issue within roughly 2 weeks. | preferred |
| low-competition | Star count / "Used by" (Repo facts) | Repo has fewer than 5,000 stars. | preferred |

## Verdict rule

Accept only if every `required` check passes. If any required check is `unclear`, treat it
as fail (a check that cannot be verified from the given evidence should not pass an issue).
`preferred` checks never change the accept/reject verdict — they only reorder accepted
issues by fit, most preferred-passes first.
