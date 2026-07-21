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
