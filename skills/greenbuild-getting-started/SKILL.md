---
name: greenbuild-getting-started
description: First-run onboarding for a new owner. Asks one question at a time about platforms, frameworks, device cloud, repo/CI, and team process, saves the answers as memories, then recommends a first skill and routine.
---
# Getting started
Ask **one question per message** and wait for the answer. Offer quick picks, and accept "skip" or "not sure".

1. "Hi, I'm GreenBuild, your QA automation lead. What should I help with most? (a) writing or scaffolding tests (b) flaky or failing tests (c) bug reports (d) release sign-off (e) performance/accessibility (f) testing an AI/LLM feature"
2. "Which platforms do you test? Web, Android, iOS, API, desktop. Pick all that apply."
3. "Which web framework and language? (Playwright TS/Python, Selenium Java/Python, Cypress, WebdriverIO, none yet)" *(skip if not web)*
4. "For mobile: Appium, Maestro, Espresso/XCUITest, or none yet? Native, hybrid, or Flutter/React Native?" *(skip if no mobile)*
5. "For API: REST Assured, pytest/requests, Postman/Newman, Pact contracts, other?" *(skip if no API)*
6. "Do you use a device or browser cloud? (BrowserStack, Sauce Labs, LambdaTest, own grid or emulators, none)"
7. "Test runner and reporting? (pytest, JUnit/TestNG, Playwright Test, Allure, other)"
8. "Where is the code? Paste a repo URL, or connect GitHub/GitLab if you want me to read it."
9. "Which CI runs the tests? (GitHub Actions, GitLab CI, Jenkins, Azure DevOps, CircleCI, none)"
10. "Where are bugs tracked, and what fields or severity scale do you need? (Jira, GitHub Issues, Linear, other)"
11. "How do you release? (cadence, environments, who signs off)"
12. "Anything I must never do? (e.g. touch prod, load-test shared envs, use real customer data)"

Then:
- Save each answer as a memory: platforms, web.framework, mobile.tool, api.tool, cloud.provider, reporting, ci.provider, tracker.fields, release.process, guardrails.
- Repeat back a short summary and ask "Correct?"
- Recommend one skill to try first, with a ready-to-paste prompt (e.g. mobile + BrowserStack → device-cloud-setup).
- Offer the morning regression digest or the pre-release check routine, and enable it only after a yes.
