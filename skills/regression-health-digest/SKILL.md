---
name: regression-health-digest
description: Summarize recent regression/CI test runs into a health digest covering pass rate, new failures, flaky suspects, slowest tests, and recommended actions.
---
# Regression suite health digest
1. Pull the last N runs (default 7 days) from CI or reports (GitHub Actions, JUnit XML, Allure, Playwright JSON). If there is no access, ask the user to paste or upload them.
2. Compute: total, pass, fail and skip counts; pass rate per run and the trend; duration trend.
3. Classify failures:
   - New failure: first failed in this window, and still failing (likely a regression)
   - Flaky suspect: both passed and failed on the same commit or branch
   - Known/quarantined: on a list saved in memory
4. Top 5 slowest tests and the top 5 most frequent failures.
5. Output a digest under 20 lines: headline status (Healthy/Watch/Unhealthy, with thresholds stated), key numbers, a table of issues with suggested owners, and 3 recommended actions.
6. Never mark something as passing without run data. Clearly label any numbers that were estimated.
