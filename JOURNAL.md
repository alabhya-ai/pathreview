## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/146

**Issue title:** PII scrubber fails to redact parenthesized US phone numbers

**Tier:** [x] Tier 1 [ ] Tier 2 [ ] Tier 3

**Problem summary:**
The `PIIScrubber` class in `safety/pii_scrubber.py` contains a regex pattern for US phone numbers that uses `\b` (word boundary) as a leading anchor. Because `\b` only matches at the boundary between a word character (letter or digit) and a non-word character, it cannot anchor before a `(` in formats like `(555) 555-1234` — the opening parenthesis is itself a non-word character, so no word boundary exists there. Additionally, the separator group `[-.]?` between digit clusters does not allow for a space, meaning the common `(555) 555-1234` format (paren, space, digits) will never match. The fix involves relaxing the leading anchor and widening the separator to include spaces, then adding test cases that cover the parenthesized format in `tests/unit/test_pii_scrubber.py`.

**Branch name:** fix/146-pii-scrubber-parenthesized-phones

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

---

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/ascherj/pathreview/commit/a16077c05c6b2fd97c2e681634c99a8d95ae31fa

**Reproduction summary:**
I added four targeted failing tests to `tests/unit/test_pii_scrubber.py` covering parenthesized formats: `(555) 555-1234` mid-sentence, at the start of a string, without a space after the closing paren, and with a `+1` country code prefix. All four tests fail against the current regex, confirming the bug is reproducible and precisely located at the `phone_us` pattern in `safety/pii_scrubber.py` line 15.

**PLAN.md link:** [https://github.com/alabhyapahari/pathreview/blob/fix/146-pii-scrubber-parenthesized-phones/PLAN.md](https://github.com/alabhyapahari/pathreview/blob/fix/146-pii-scrubber-parenthesized-phones/PLAN.md)

**Walkthrough video (recommended):** N/A

**Blockers or open questions:**
Need to confirm the Python 3.11 environment is stable before running the full test suite to verify no regressions after the regex fix.
