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