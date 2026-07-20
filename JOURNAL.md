## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/146

**Issue title:** PII scrubber fails to redact parenthesized US phone numbers

**Tier:** [x] Tier 1 [ ] Tier 2 [ ] Tier 3

**Problem summary:**
The `PIIScrubber` class in `safety/pii_scrubber.py` contains a regex pattern for US phone numbers that uses `\b` (word boundary) as a leading anchor. Because `\b` only matches at the boundary between a word character (letter or digit) and a non-word character, it cannot anchor before a `(` in formats like `(555) 555-1234` — the opening parenthesis is itself a non-word character, so no word boundary exists there. Additionally, the separator group `[-.]?` between digit clusters does not allow for a space, meaning the common `(555) 555-1234` format (paren, space, digits) will never match. The fix involves relaxing the leading anchor and widening the separator to include spaces, then adding test cases that cover the parenthesized format in `tests/unit/test_pii_scrubber.py`.

**Branch name:** fix/146-pii-scrubber-parenthesized-phones

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger
