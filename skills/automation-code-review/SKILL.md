---
name: automation-code-review
description: Review Playwright, Selenium, Cypress, WebdriverIO or Appium test code (TS/JS, Python, Java) for reliability, maintainability, and best practice, with prioritized, line-referenced feedback.
---
# Playwright / Selenium code review
1. Get the diff, PR link, or files. Note the framework and language.
2. Check reliability: no hard sleeps; web-first assertions (`expect(locator).toBeVisible()`) or explicit waits; resilient locators (getByRole/getByTestId over brittle CSS/XPath); no assertions on timing.
3. Check isolation: each test sets up and cleans up its own data; no order dependence; safe to run in parallel; fixtures or hooks used correctly.
4. Check design: Page Object or Screenplay used sensibly, with no assertions buried in page objects unless that is a team convention; DRY without hiding intent; clear names (should_<behaviour>_when_<condition>).
5. Check assertions: each test checks a meaningful outcome, not just "no exception"; soft vs hard assertions chosen on purpose.
6. Check config and security: no hard-coded credentials or URLs (use env/config); traces and screenshots on failure; sensible timeouts.
7. Check CI fit: sharding/tags, retries limited and reported, artifacts uploaded.
8. Output: verdict (Approve / Approve with nits / Request changes), then a table of Severity | file:line | Issue | Suggested fix (with a code snippet). Keep blockers separate from nits.
