---
name: framework-scaffolder
description: Scaffold or refactor a web UI test framework in Playwright, Selenium 4, Cypress, or WebdriverIO, with page objects, BDD (Cucumber/Behave) if wanted, config/env handling, and Allure reporting.
---
# Framework scaffolder
1. Ask for the language, framework, BDD yes/no, CI, and reporting. Default: Playwright + TS, or pytest-playwright for Python teams.
2. Bootstrap commands:
   - Playwright TS: `npm init playwright@latest` | Python: `pip install pytest-playwright && playwright install --with-deps`
   - Selenium 4: `pip install selenium pytest` (Selenium Manager fetches drivers automatically, so no webdriver-manager is needed) | Java: `org.seleniumhq.selenium:selenium-java` + TestNG/JUnit 5
   - Cypress: `npm i -D cypress && npx cypress open`
   - WebdriverIO: `npm init wdio@latest .`
3. Layout:
```
tests/          specs, grouped by feature
pages/          page objects (locators + actions, no asserts)
fixtures/       setup/teardown, auth state, data factories
config/         env files: base URL, timeouts (no secrets)
utils/          api clients, helpers
reports/
```
4. Page objects: expose user intent (`login(user)`), not raw clicks. Return the next page or component. Keep assertions in tests.
5. Auth: log in once through the API or UI and reuse the state. Playwright: `storageState` in a setup project. Selenium: inject cookies.
6. BDD (only if business people read the specs): Cucumber (JS/Java) or Behave (`pip install behave`; `features/*.feature` + `features/steps/`; run `behave`). Keep steps thin, have them call page objects, and avoid UI wording in Gherkin.
7. Reporting with Allure:
   - pytest: `pip install allure-pytest`, then `pytest --alluredir=allure-results`, then `allure serve allure-results`
   - Playwright: `npm i -D allure-playwright`, then reporter `['allure-playwright']`
   - Attach screenshots, traces and logs on failure.
8. Config: retries are 0 locally and 1–2 in CI, but report them. Run in parallel. Tag smoke vs regression (`@smoke`, `-m smoke`, `--grep @smoke`).
9. Deliver the scaffold as files plus a README with run commands, then offer a CI job (see ci-cd-and-test-data).
