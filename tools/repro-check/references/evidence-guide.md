# Evidence guide: where proof lives in a reproduction package

Every rubric check names evidence; this guide says where that evidence
sits and what good looks like once you find it. Five families, matching
the five proof families the rubric checks against.

An eval bundle has exactly five top-level sections, and they are the
addresses used throughout this guide: `## Issue`, `## Thread
highlights`, `## Repo facts (captured …)`, `## Candidate claim
comment`, and `## Candidate repro report`. The report's internal
headings vary from package to package — some use `#### Environment`,
some a bare `Environment:` line, some neither — so locate parts of the
report by what they contain, never by an expected heading. In live
mode the same five addresses become: the issue page, its comment
thread, the repo's CONTRIBUTING/AI_POLICY and issue templates, and the
student's two drafts.

A check whose evidence this guide cannot locate is a check nobody else
can execute. If a check needs a source not listed here, add it here
first.

## Environment

**Where it lives.**

| Signal | In the eval bundle | Live |
|---|---|---|
| What the package ran on | `## Candidate repro report`: the environment line or block, wherever it sits — often the first line, sometimes folded into the steps | the student's draft repro report |
| What the issue targets | `## Issue`: the reporter's stated version and platform; `## Thread highlights`: later comments confirming it on other versions, or on `main` | the issue body and thread |
| Which axes matter here | `## Repo facts`: the "bug reports:" line names the fields the template asks for (version, config, platform, driver) | the repo's bug-report template |

**What good looks like.** The record names the tool version and the
platform, plus every axis this particular issue turns on — a
Windows-only issue needs the driver, a locale issue needs the language
setting, a shell-prompt issue needs the shell. The version and platform
tested either match what the issue names as affected, or the report
says plainly how they differ. A terse one-line record carrying those
facts is better evidence than a long block that omits the deciding one.
The failure to watch for is a silent deviation: an older version tested
against an issue confirmed on latest, with nothing said about it.

## Steps

**Where it lives.**

| Signal | In the eval bundle | Live |
|---|---|---|
| The steps themselves | `## Candidate repro report`: the numbered or shell-transcript sequence | the student's draft repro report |
| The trigger they must hit | `## Issue`: the reporter's own reproduction recipe; `## Thread highlights`: any owner note narrowing what actually triggers it | the issue body and thread |
| Whether inputs are shareable | the steps' own text: files created inline with `printf`/`cat`, exact flags, a playground link — versus references to something the reader does not have | the draft, read as a stranger would |

**What good looks like.** A reader with the named environment can go
from a clean machine to the symptom using only the report's text:
starting state given, every input created inline or fetchable, commands
exact enough to paste. The sequence actually exercises the trigger the
issue names — same syntax, same configuration path — rather than a
neighbouring one. A control run that varies one thing and shows the
symptom disappear is the strongest form this family takes. The failure
to watch for is a reproduction that lives somewhere the reader cannot
go: a private repository, an unshared config, "my setup".

## Behavior shown

**Where it lives.**

| Signal | In the eval bundle | Live |
|---|---|---|
| The artifacts | `## Candidate repro report`: fenced output blocks, log excerpts, screenshots, control-run output | the student's draft repro report |
| The symptom to match them against | `## Issue`: the specific error text, exit status, panic, or visible misbehavior the reporter names | the issue body |
| The report's stated outcome | `## Candidate repro report`: whether it says the issue reproduced or says it could not | the draft's conclusion |

**What good looks like.** Read the outcome first, because it decides
what the artifacts have to show. For a claimed reproduction, an
artifact shows the issue's behavior when the thing in the output is the
thing the issue named: the same message, the same exit status, the same
wrong value. Adjacent is not enough — a graceful argument-validation
error is not a capacity-overflow panic, a compile error is not an
invalid-path error, and output proving only that the tool starts and
runs proves nothing about the bug. For a stated cannot-reproduce, the
artifacts show something different and just as checkable: that the
attempt was real and aimed at the right target, with the issue's own
syntax, configuration, and version path visible in what was run.

## Honesty

**Where it lives.**

| Signal | In the eval bundle | Live |
|---|---|---|
| The claims | `## Candidate claim comment`, plus the conclusion or summary sentences in `## Candidate repro report` | both drafts |
| Their backing | the artifacts in that same report — nothing outside the package counts | the draft's artifacts only |
| The conditions departed from | `## Issue` and `## Thread highlights`, against what the report says it actually did | the issue thread against the draft |

**What good looks like.** Each sentence that asserts something is
matched by something shown. Confirmations rest on artifacts, causes
rest on evidence of the cause rather than a plausible story, and claims
about frequency or scope ("every time", "on all platforms") rest on
runs that establish them. Where the package departed from the issue's
conditions, the report is the one that says so, rather than leaving the
reader to notice.

An evidenced cannot-reproduce is a strong package, not a weak one: it
states the outcome plainly, shows the attempt, and names what differed
from the reporter's conditions and what might therefore be required to
trigger it. Judge it as a pass on this family. The failure to watch for
is its opposite — a confident narration laid over artifacts that do not
support it, which reads as more thorough than an honest negative and is
worth less.

## Comms

**Where it lives.**

| Signal | In the eval bundle | Live |
|---|---|---|
| The claim comment | `## Candidate claim comment` | the student's draft claim comment |
| What the repo asks for | `## Repo facts`: the "bug reports:" line for template asks, the "contribution policy" line for CONTRIBUTING/AI_POLICY terms | CONTRIBUTING.md, AI_POLICY.md, the issue templates |
| What AI use was stated | any disclosure sentence in either draft | both drafts |
| The issue being answered | `## Issue` and `## Thread highlights` | the issue page |

**What good looks like.** The comment could not be pasted onto another
issue without becoming false: it names what this author observed, on
what version, and what they intend to do next in this code. It promises
only what the package supports — no delivery date, no guaranteed fix,
no request to reserve the issue. Boilerplate is the failure to watch
for, and it is recognisable by substitution: swap in another issue's
title and if the comment still reads correctly, it carries no evidence.

AI-use policy is the part most easily read too broadly, so read the
policy line for **who it binds and which artifact**:

| The policy line says | Disclosure required on a comment? |
|---|---|
| no AI policy stated | no |
| use AI responsibly, understand what you submit | no |
| comments to maintainers must be human-written in their own words | no — that is an authorship rule; satisfied by a human-voiced comment |
| state the tool and extent **in the pull request** | no — scoped to PRs, not issue comments |
| all AI usage **in any form** must be disclosed | yes |

When disclosure is required, what satisfies it is a sentence naming
that an AI tool was used and how far its help went. When it is not
required, its absence is not a fault and a voluntary disclosure is not
a bonus.
