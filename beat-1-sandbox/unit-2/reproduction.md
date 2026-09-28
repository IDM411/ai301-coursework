# Unit 2 - Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

IDM411

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5861758532

I'd like to take issue #69. The crash happens in `_parse_json_output` in `rag/generator/output_parser.py`. When the model's output comes back as a Python list instead of a dict, the code calls `.items()` on it, which throws `AttributeError: 'list' object has no attribute 'items'`. I'll proceed with reproducing it locally using the existing `test_json_array_fallback` test in `tests/unit/test_output_parser.py`. I will update this thread as I go, including potential issues as they arise. I'll also remove the `xfail` marker on that test (manifest H-02) as part of the fix, per CONTRIBUTING.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5862187159

Environment:
- OS: Nobara Linux (Fedora-based)
- Commit: 2f4e82f52efbcfcc57d65b3fa5348672163ca088
- Python 3.11.15 (via uv venv, since the system default Python was 3.14)
- Docker Compose v5.5.1, Postgres on port 5433, Redis remapped to 6380
  (local port conflict with an unrelated project on this machine, not
  related to the app itself)
- vector-db (Chroma) container fails to start due to a NumPy 2.0
  incompatibility baked into the pinned image, but nothing in the app
  code connects to it, so this doesn't block reproduction
- Clean baseline confirmed before any changes: 375 passed, 53 xfailed,
  0 failed across the full unit suite; lint/format/typecheck all green

Steps to reproduce:
1. Clone and set up the repo per docs/SETUP.md (see environment notes
   above for the Python version workaround needed)
2. Run: .venv/bin/pytest "tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback" -v --runxfail --tb=long

Observed behavior:

````text
============================= test session starts ==============================
platform linux -- Python 3.11.15, pytest-9.1.1, pluggy-1.6.0 -- /home/id411/Documents/pathreview-ai301-fa26-s3/.venv/bin/python
cachedir: .pytest_cache
hypothesis profile 'default'
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: /home/id411/Documents/pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: cov-7.1.0, pytest_httpserver-1.1.5, hypothesis-6.168.2, anyio-4.15.1, platformdirs-4.12.0, asyncio-1.4.0, benchmark-5.3.0
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 1 item

tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback FAILED [100%]

=================================== FAILURES ===================================
__________________ TestOutputParser.test_json_array_fallback ___________________

self = <tests.unit.test_output_parser.TestOutputParser object at 0x7f5ffb10ab50>

    @pytest.mark.xfail(
        strict=True,
        reason="issue #69 (manifest H-02): output parser calls .items() on a JSON array fallback",
    )
    def test_json_array_fallback(self):
        """Test handling of JSON array (not dict)."""
        raw_output = json.dumps(["First feedback item", "Second feedback item"])
    
>       result = parse_review_output(raw_output)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

tests/unit/test_output_parser.py:149: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

raw = '["First feedback item", "Second feedback item"]'

    def parse_review_output(raw: str) -> list[FeedbackSection]:
        """Parse LLM output into structured feedback sections.
    
        Args:
            raw: Raw LLM output string
    
        Returns:
            List of FeedbackSection objects
        """
        # Seeded defect: this accumulator is never appended to or returned, so
        # parsed sections are lost. Retained on purpose as course material.
        sections = []  # noqa: F841
    
        # Try JSON in code fence first
        json_match = re.search(r"```(?:json)?\s*\n(.*?)\n```", raw, re.DOTALL)
        if json_match:
            json_str = json_match.group(1)
            try:
                data = json.loads(json_str)
                return _parse_json_output(data)
            except json.JSONDecodeError:
                logger.warning("json_parsing_failed_in_fence", json_snippet=json_str[:100])
    
        # Try raw JSON
        try:
            data = json.loads(raw)
>           return _parse_json_output(data)
                   ^^^^^^^^^^^^^^^^^^^^^^^^

rag/generator/output_parser.py:48: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

data = ['First feedback item', 'Second feedback item']

    def _parse_json_output(data: dict) -> list[FeedbackSection]:
        """Parse structured JSON output.
    
        Args:
            data: Parsed JSON dict
    
        Returns:
            List of FeedbackSection objects
        """
        sections = []
    
        # Handle both single-level and nested structures
>       for key, value in data.items():
                          ^^^^^^^^^^
E       AttributeError: 'list' object has no attribute 'items'

rag/generator/output_parser.py:68: AttributeError
=========================== short test summary info ============================
FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
============================== 1 failed in 0.12s ===============================
````

AttributeError: 'list' object has no attribute 'items'
rag/generator/output_parser.py:68

Root cause:
parse_review_output() at output_parser.py:47 calls json.loads(raw) and passes
the result directly into _parse_json_output(), which is type-annotated
as expecting a dict and immediately calls .items() on it. When the raw
output is a top-level JSON array rather than an object, json.loads()
successfully returns a Python list, so no JSONDecodeError is raised and
the plaintext fallback path is never reached. The .items() call then
fails, since lists don't have that method.

This matches issue #69's description exactly. The failing test is
test_json_array_fallback, under the xfail marker for manifest H-02.

## Eval iterations

**Run history**

Two full 20-package runs, in order:

1. Full 20-item run - `agreement: 20/20 scored items  (bar: 18/20: PASS)`
2. Full 20-item run, repeated with the rubric unchanged to test grade stability,
   written with `--save-run eval-run.txt` - `agreement: 20/20 scored items  (bar: 18/20: PASS)`

The final score matches the agreement line in the committed `eval-run.txt` exactly:

```
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

**Package analysis**

`pkg-15` (source `laurent22/joplin#16205`, category `no-evidence`). My rubric returned
`reject`; the gold label is `reject`. They agree.

The package is a confident root-cause diagnosis with no artifact anywhere in it. The
report explains a debounce race between the Rich Text editor's save timer and the
note-switch handler, then states "I verified this race condition is the cause" and closes
with "I am confident this is the correct diagnosis." It shows no transcript, no log, no
screen recording, and no steps beyond the environment line "Joplin 3.7.10, Windows 11."

My rubric read it through the honesty check, whose fail condition names this case
directly: the package "asserts a confirmation, a root cause ... its artifacts do not
show." Nothing in the report backs the causal story, so honesty fails. `behavior-shown`
fails for the same absence, since there is no artifact to match against the issue's
symptom of lost keystrokes. The claim comment compounds it by promising "a fix approach
to discuss soon" on the strength of a diagnosis the package never evidences.

The trap in this package is that it reads as expert. It is fluent, technically plausible,
and names a real mechanism. A rubric that graded polish would accept it. Grading the
artifacts rather than the prose is what produces the correct `reject`.

**Check rationale**

The `honesty` check in `tools/repro-check/rubric.md` currently reads:

> Pass when each claim is backed by a shown artifact and every departure from the issue's stated conditions is named by the report itself. A report stating it could not reproduce PASSES this check when it shows the attempt and names what differed from the issue's conditions: a failed reproduction, honestly reported and evidenced, is a pass and never a fail. Fail when the package asserts a confirmation, a root cause, a frequency ("every single time"), or a scope its artifacts do not show, or when it narrates an artifact as the issue's symptom while the artifact shows something else.

It reads that way because of what it caught in my own reproduction package for issue #69.

My first draft of `repro.md` failed this check on two counts. It closed with the sentence
"the failing test (test_json_array_fallback, xfail marker manifest H-02) reproduces it
reliably" - a reliability claim resting on a single run whose output the draft never
showed. It also carried a Root cause paragraph asserting a specific call chain: that line
47 calls `json.loads(raw)`, that the result is passed into `_parse_json_output()`, that no
`JSONDecodeError` is raised, and that the plaintext fallback is therefore never reached.
Not one of those four steps was backed by anything in the package. The draft contained no
fenced block and no traceback at all, so the mechanism was narrated rather than shown.

The check's phrase "each claim is backed by a shown artifact" is what failed it, and the
words "nothing outside the package counts" in the evidence guide are why being right was
not enough. The diagnosis was correct. I had verified it locally. But the reader of a
GitHub thread sees only the comment, and the comment proved none of it.

The fix followed the check rather than arguing with it. I pasted the real `--runxfail`
pytest output into a fenced block and dropped the reliability claim. The traceback shows
`raw = '["First feedback item", "Second feedback item"]'` going in, the source context at
line 47, the handoff at `output_parser.py:48`, then `data = ['First feedback item',
'Second feedback item']` - the visible proof that `json.loads` returned a list - followed
by `def _parse_json_output(data: dict)` and the failure at line 68. Every step I had
asserted became a step a stranger can see. The re-run passed honesty, and the package
moved from `reject` to `accept`.

This is the same failure `pkg-15` embodies, which is why I kept the fail condition
explicit about root causes rather than folding them into a general "be accurate" rule. A
vague honesty check would have let both packages through.

**Trade-offs**

The check-level grades are not fully stable, and the eval run does not show this.

I ran the full 20-package suite twice with the rubric, the evidence guide, and the model
pinning unchanged. Both runs scored `agreement: 20/20 scored items`, and all 20 verdicts
were identical. Underneath those identical verdicts, 5 of the 120 individual check grades
moved between the two runs (20 packages x 6 checks = 120 grades).

The verdicts were stable because the rubric's verdict rule only asks whether every
required check passed, so a check flipping between `fail` and `unclear` on a package that
was already rejected for other reasons changes nothing visible. The stability I can honestly
claim is verdict-level, not check-level.

The limitation this imposes is on measurement. A rubric edit that changes fewer than about
5 check grades cannot be distinguished from run-to-run noise at n=20, so I cannot use this
harness to verify a small wording change did what I intended. I would need repeated runs
per variant, or a larger package set, to resolve anything at that scale. I am accepting
that: the check wording above was validated by the concrete failure it caught on my own
draft, which is direct evidence, rather than by a delta in the aggregate score.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
