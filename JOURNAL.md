## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/151

**Issue title:** Bias detector patterns are too narrow to match common phrasings

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Selection notes (checklist reasoning):**
I used the "Is this right for me?" checklist before committing to this issue.
- Is it actually open? Yes. I checked the Development section on the issue page and confirmed there are no linked branches or pull requests yet, so nobody has already solved it.
- Is the scope clear? Yes. The issue names the exact file (`safety/bias_detector.py`) and lists 9 specific failing unit tests in `tests/unit/test_bias_detector.py`, so I know exactly what "done" looks like.
- Is it the right size? Yes. This is a regex and pattern matching fix inside one file, which matches a Tier 1 scope.
- Is the maintainer active? Yes. The maintainer (ascherj) opened this issue only 3 days ago and tagged it live.
- Does it match where I am? Yes. I'm comfortable with Python string and pattern logic from past projects, and this issue doesn't require touching the RAG pipeline or agent system, so I'm not stacking an unfamiliar codebase on top of an unfamiliar concept.

This is my first time contributing to a codebase this large, so I picked Tier 1 to keep the learning curve limited to just the codebase, not the codebase plus a hard problem.

**Problem summary:**
The bias detector in `safety/bias_detector.py` is supposed to flag biased language in generated reviews, like dismissing a candidate's education or making assumptions based on age. Right now the regex patterns only catch near word-for-word phrasings of that bias, so more natural, real world ways of saying the same thing slip through undetected. Nine unit tests in `tests/unit/test_bias_detector.py` already define what the detector should catch, and all nine currently fail. A successful fix means broadening the patterns so the detector correctly flags these natural phrasings, without breaking any tests that currently pass.

**Branch name:** fix/151-bias-detector-patterns

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Fidan222/pathreview/commit/c81dd43

**Reproduction summary:**
I reproduced the issue by running `BiasDetector.detect_bias()` directly on the exact phrase from the issue ("The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education") and confirmed it returned `(False, '')` instead of flagging it as biased. I also ran `pytest tests/unit/test_bias_detector.py -v` and confirmed 9 tests fail while 23 pass, matching the count described in the issue exactly. The failures show a consistent pattern: the detector catches some phrasings of bootcamp/age/demographic bias but misses natural variations like "can't write production code," "can't handle complex systems," and "aren't equal to."

**PLAN.md link:** https://github.com/Fidan222/pathreview/blob/fix/151-bias-detector-patterns/PLAN.md

**Walkthrough video (recommended):** (not recorded — optional, not graded)

**Blockers or open questions:**
Not yet sure how loose I can make the regex before it starts flagging clean/positive feedback as biased — I'll need to test carefully against the 23 currently-passing tests while I fix the 9 failing ones.

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
I implemented the fix in `safety/bias_detector.py`, broadening the regex patterns per my PLAN.md: added support for plural nouns (developers/programmers, not just singular), can't/won't phrasing, removed the unnecessary "is" requirement before "lacks," and added a new pattern for "attendance means inadequate training" phrasing. All 9 previously-failing tests in `tests/unit/test_bias_detector.py` now pass, with no regressions to the 23 that were already passing.

**Next steps:**
Open a draft PR, run it through `make check` and the full test suite one more time, get a classmate or mentor to review it in Slack, then finalize and submit.

**Blockers:**
None currently.

---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/609

**Branch:** fix/151-bias-detector-patterns

**What you built:**
Broadened the regex patterns in `safety/bias_detector.py` to catch natural phrasings of biased language that the original patterns missed — plural nouns, "can't/won't" phrasing, and a new pattern for "attendance means inadequate training."

**Tests added or updated:**
No new test files — the issue included 9 pre-written failing tests in `tests/unit/test_bias_detector.py` that define the expected behavior. All 9 now pass, with no regressions to the 23 that were already passing.

**Self-review confirmation:** [x] make check passes  [x] make test-unit passes

**Draft PR feedback received from:** None — per the Week 9 lecture, no code review feedback is provided this term; completed self-review against the seven conditions checklist instead.