---
name: flaky-test-triage
description: Diagnose a flaky or intermittently failing automated test from logs, traces, and code, classify the root cause, and propose a fix (not just a retry).
---
# Flaky test triage
1. Gather the failing test code, error and stack trace from 2+ failures, a passing run if there is one, CI job info, and a Playwright trace, screenshot or video if available.
2. Confirm it is flaky: same commit, different results? If it fails every time, treat it as a real failure and stop triage.
3. Classify the cause using the usual taxonomy:
   - Timing/async: hard sleeps, missing auto-wait, animations, network races
   - Selector fragility: positional XPath, dynamic IDs, text that changes with locale
   - Test-order dependence and shared state: leaked data, shared accounts, parallel collisions
   - Environment: slow CI runners, third-party services, timezone or locale, resource limits
   - Test data: non-unique or expiring data
   - Real product race condition (this is a bug, so file it)
4. Point to the evidence for the chosen cause (log line, trace step, code line).
5. Propose a concrete fix with a code diff. Prefer web-first assertions and role/test-id locators in Playwright, and explicit waits (WebDriverWait) in Selenium. Never propose Thread.sleep or a blanket retry as the fix.
6. Give a verification step, e.g. run 50x with `--repeat-each=50` (Playwright) or `pytest --count=50` (pytest-repeat).
7. If quarantining is needed, recommend it with an owner and an expiry date, and ask before changing anything.
