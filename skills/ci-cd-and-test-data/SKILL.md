---
name: ci-cd-and-test-data
description: Wire tests into CI/CD (GitHub Actions, GitLab CI, Jenkins, Azure DevOps) with quality gates, sharding, artifacts, and a reliable test-data strategy.
---
# CI/CD and test data
1. Pipeline stages: lint/unit → API tests → UI smoke on each PR → full regression nightly or pre-release → perf/a11y on a schedule.
2. GitHub Actions (Playwright example):
```yaml
jobs:
  e2e:
    runs-on: ubuntu-latest
    strategy: { matrix: { shard: [1, 2, 3, 4] } }
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 22 }
      - run: npm ci && npx playwright install --with-deps
      - run: npx playwright test --shard=${{ matrix.shard }}/4
        env: { BASE_URL: ${{ vars.STAGING_URL }} }
      - uses: actions/upload-artifact@v4
        if: ${{ !cancelled() }}
        with: { name: report-${{ matrix.shard }}, path: playwright-report/ }
```
   GitLab: `parallel:` + `artifacts: when: always`. Jenkins: `junit 'results/*.xml'` in `post { always }`.
3. Quality gates: fail on any test failure, report retries separately (a pass after a retry means flaky, not green), and use coverage or contract checks where the team agreed.
4. Secrets: CI secret store only. Mask them in logs, and never echo tokens.
5. Test data strategy:
   - Create through the API in setup with unique values (uuid/timestamp suffix), and clean up in teardown.
   - Use factories/builders (Faker, factory_boy) instead of shared fixtures that mutate.
   - Seed known reference data per environment, and use read-only accounts for read-only tests.
   - Isolate per worker for parallel runs. Mask or synthesize PII, and never copy production personal data.
6. Speed: cache dependencies, shard, run only impacted tests on PRs where tooling supports it, and fail fast on smoke.
