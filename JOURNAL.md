## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/153

**Issue title:** Faithfulness checker crashes when a context chunk has text: None

**Tier:** [x] Tier 1

**Problem summary:**
The FaithfulnessChecker.check() method builds a joined context string using
chunk.get("text", ""), assuming this default only applies when the "text" key
is missing. However, when a chunk explicitly has "text": None, .get() returns
None rather than falling back to the default, since the key is present. The
subsequent " ".join(...) call then raises a TypeError because it cannot join
a None value into a string. A successful fix ensures None values are
normalized to empty strings regardless of whether the key is missing or
explicitly set to None, preventing the crash on malformed or incomplete
context chunks. This issue is right and relevant to me as i will not be burning myself out in terms of what tier i chose.
I wanted to also work with an issue that involes RAG which this issue does.

**Branch name:** fix/153-faithfulness-checker-none-text

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger
---
## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [https://github.com/GGirishya/pathreview/commit/60c7ecb]

**Reproduction summary:**
Reproduced by running `pytest tests/unit/test_faithfulness_checker.py -k test_none_context_chunk_text -v`, which fails with `TypeError: sequence item 0: expected str instance, NoneType found` at the `.join()` call in `check()`, confirming the root cause described in the issue.

**PLAN.md link:** https://github.com/GGirishya/pathreview/blob/fix/153-faithfulness-checker-none-text/PLAN.md

**Walkthrough video (recommended):** [add if you record one, otherwise leave blank]

**Blockers or open questions:**
Multiple contributors have linked PRs to issue #153, so I may need to compare my fix against theirs before finalizing next week.

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Applied the fix to `rag/evaluator/faithfulness_checker.py` — changed
`chunk.get("text", "")` to `chunk.get("text") or ""` so both a missing
`"text"` key and an explicit `None` value normalize to an empty string.
The previously-failing test `test_none_context_chunk_text` now passes.

**Next steps:**
Run the test suite to confirm no regressions, finalize the PR
description, and submit the PR.

**Blockers:**
None.

---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/884

**Branch:** fix/153-faithfulness-checker-none-text

**What you built:**
Fixed a crash in `FaithfulnessChecker.check()` caused by context chunks
with `"text": None` — `.get()`'s default only applied to missing keys,
not explicit `None` values, so `" ".join(...)` raised a `TypeError`.
Changed the lookup to `chunk.get("text") or ""` to normalize both cases.

**Tests added or updated:**
No new tests were added — the existing test
`tests/unit/test_faithfulness_checker.py::test_none_context_chunk_text`
already covered this exact case and now passes with the fix in place.

**Self-review confirmation:** [x] make test-unit passes  [ ] make check passes

I ran `make test-unit` before and after the fix and confirmed the
targeted test now passes with no new regressions — 3 pre-existing
failures in `test_faithfulness_checker.py` (unrelated scoring-logic
issues) were present identically before and after my change. I did not
run `make check` as its own command; ruff, black, and mypy all passed
via the project's pre-commit hooks on every commit.

**Draft PR feedback received from:** none