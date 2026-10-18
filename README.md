# GreenBuild QA

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/godhiraj-code/greenbuild-plugin/blob/main/LICENSE)
[![Skills: 17](https://img.shields.io/badge/skills-17-brightgreen.svg)](https://github.com/godhiraj-code/greenbuild-plugin/tree/main/skills)

**Ask your coding agent "why is this Playwright test flaky?" and get a root cause, not a retry.**

A senior QA and test-automation lead, packaged as an Agent/Cursor plugin of 17 skills. It turns requirements into test plans, triages flaky tests, reviews Playwright and Selenium code, writes bug reports developers can act on, and tells you whether a release is ready to ship.

Skills only: no MCP server, no hooks, no network calls, no credentials, no data collection.

## Quick start (Cursor, local install)

```bash
git clone https://github.com/godhiraj-code/greenbuild-plugin ~/.cursor/plugins/local/greenbuild-qa
```

Run *Developer: Reload Window*, confirm the 17 skills appear under Customize → Skills, then try:

- `/flaky-test-triage` This Playwright test fails 1 in 5 runs: `await page.click('#submit'); expect(await page.textContent('.toast')).toBe('Saved')`
- `/test-plan-from-requirements` As a user I can reset my password via an emailed link that expires in 30 minutes

New here? Start with `/greenbuild-getting-started`. More install options are under [Install](#install).

**Learn more:** [GreenBuild project page](https://www.dhirajdas.dev/project/greenbuild) · [dhirajdas.dev](https://www.dhirajdas.dev)

## Skills
| Skill | What it does |
|---|---|
| `accessibility-audit` | Run automated axe-core accessibility checks in Playwright, Selenium, or Cypress against WCAG 2.x A/AA, triage violations, and list the manual checks automation cannot cover. |
| `api-test-generator` | Generate API test cases and runnable tests (pytest+requests, REST Assured, Playwright request, Postman/Newman) from an OpenAPI spec or endpoint description, plus consumer-driven contract tests with Pact. |
| `automation-code-review` | Review Playwright, Selenium, Cypress, WebdriverIO or Appium test code (TS/JS, Python, Java) for reliability, maintainability, and best practice, with prioritized, line-referenced feedback. |
| `bug-report-writer` | Turn rough notes, screenshots, or logs into a clear, reproducible bug report formatted for the user's tracker. |
| `ci-cd-and-test-data` | Wire tests into CI/CD (GitHub Actions, GitLab CI, Jenkins, Azure DevOps) with quality gates, sharding, artifacts, and a reliable test-data strategy. |
| `device-cloud-setup` | Run web and mobile tests on BrowserStack (Automate, App Automate, Local, reporting/observability), Sauce Labs, or LambdaTest, with capabilities, tunnels, parallelism, and CI secrets. |
| `flaky-test-triage` | Diagnose a flaky or intermittently failing automated test from logs, traces, and code, classify the root cause, and propose a fix (not just a retry). |
| `framework-scaffolder` | Scaffold or refactor a web UI test framework in Playwright, Selenium 4, Cypress, or WebdriverIO, with page objects, BDD (Cucumber/Behave) if wanted, config/env handling, and Allure reporting. |
| `greenbuild-getting-started` | First-run onboarding for a new owner. Asks one question at a time about platforms, frameworks, device cloud, repo/CI, and team process, saves the answers as memories, then recommends a first skill and routine. |
| `llm-app-testing` | Test LLM/AI features and agents with deterministic mocks, eval datasets, rubric/assertion checks, streaming UI tests, and safety/prompt-injection probes. |
| `mobile-automation` | Set up, write, and debug mobile UI tests with Appium (UiAutomator2, XCUITest drivers), Maestro YAML flows, and native Espresso/XCUITest, and pick the right tool per team. |
| `performance-test-starter` | Design and script a first performance test (load, stress, spike, soak) with k6, JMeter, or Locust, with SLO-based thresholds and result interpretation. |
| `regression-health-digest` | Summarize recent regression/CI test runs into a health digest covering pass rate, new failures, flaky suspects, slowest tests, and recommended actions. |
| `release-readiness-checklist` | Produce an evidence-based go/no-go release readiness report covering test results, open defects, coverage of changes, and risks. |
| `test-plan-from-requirements` | Turn a user story, PRD, or ticket into a risk-based test plan with prioritized test cases, test data, and automation candidates. |
| `tools-and-libraries` | Reference of recommended open-source QA tools, including Python libraries by this template's author, with what each solves and when to suggest it. |
| `visual-testing` | Add visual regression testing with Playwright screenshots, Applitools, Percy, or open-source diffing, with stable baselines and low false positives. |

## Install
- **Marketplace (after approval):** search "GreenBuild QA" in Cursor's Customize page or the Grok Bot marketplace.
- **Local test (Cursor):** copy this folder to `~/.cursor/plugins/local/greenbuild-qa`, run *Developer: Reload Window*, and confirm the 17 skills appear under Customize → Skills.

## Usage
Ask in plain language, e.g. "Write a test plan for this user story", "Why is this Playwright test flaky? <log>", "Review this Selenium page object", or invoke a skill directly with `/flaky-test-triage`. Start with `/greenbuild-getting-started` for onboarding.

No configuration is required. The persona and default tool stack are described in [docs/PROFILE.md](https://github.com/godhiraj-code/greenbuild-plugin/blob/main/docs/PROFILE.md); the skills adapt to your stack when you name it.

## When to use GreenBuild (vs a generic coding agent)

A general-purpose coding agent can already answer QA questions. GreenBuild gives it a fixed, reviewable workflow for each task, so the answers are consistent and evidence-first:

- **Flaky tests:** `flaky-test-triage` first confirms the test is actually flaky (same commit, different results). It then classifies the cause (timing/async, selector fragility, shared state, environment, test data, real product race), points to the evidence, proposes a code fix, and gives a verification step such as `--repeat-each=50` or `pytest --count=50`. It never proposes `Thread.sleep` or a blanket retry as the fix.
- **Test plans and release checks:** risk-based test plans and an evidence-based go/no-go report instead of a free-form list.
- **Code review:** prioritized, line-referenced feedback on Playwright, Selenium, Cypress, WebdriverIO or Appium code.

It is not a test runner, a reporting dashboard, or a device cloud. It doesn't execute tests or connect to your CI by itself; it works with the code, logs and evidence you give the agent.

## FAQ

**Does it send my code or logs anywhere?**
The plugin contains only `SKILL.md` instructions: no MCP server, no hooks, no scripts and no network calls. Your prompts are handled by whatever agent and model you already use in Cursor.

**Does it store anything?**
Only `/greenbuild-getting-started` asks your agent to save your onboarding answers (platforms, frameworks, CI, tracker fields, and so on) as memories, so later skills can adapt to your stack. Skip it if you don't want that.

**Do I need Playwright or Selenium installed?**
No, not for the skills themselves. They write and review code for whatever framework you name, and you run the tests in your own environment.

**The skills don't show up after installing.**
Check that the folder is `~/.cursor/plugins/local/greenbuild-qa` and contains `.cursor-plugin/plugin.json`, then run *Developer: Reload Window* again.

## Principles
Evidence over opinion; never invents test results; never hides a failure behind a retry; never posts to trackers or chat without your approval.

## Author
Dhiraj Das · https://dhirajdas.dev · GitHub [@godhiraj-code](https://github.com/godhiraj-code) · X [@dkkkkd](https://x.com/dkkkkd)

## License
MIT. See [LICENSE](https://github.com/godhiraj-code/greenbuild-plugin/blob/main/LICENSE).
