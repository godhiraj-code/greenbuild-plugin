---
name: visual-testing
description: Add visual regression testing with Playwright screenshots, Applitools, Percy, or open-source diffing, with stable baselines and low false positives.
---
# Visual testing
1. Decide what a screenshot should prove: layout and branding of key pages and components, not dynamic data.
2. Playwright built-in:
```ts
await expect(page).toHaveScreenshot('checkout.png', {
  mask: [page.getByTestId('timestamp')], maxDiffPixelRatio: 0.01, animations: 'disabled' });
```
   Update baselines on purpose with `npx playwright test --update-snapshots`, and review the diff in the PR.
3. Make it stable: fixed viewport, fonts and locale; seeded data; mocked time; masked or hidden dynamic regions; generate baselines on the same OS/browser as CI (e.g. in the Playwright Docker image).
4. Hosted tools: Applitools Eyes (Visual AI match levels), Percy (BrowserStack), Sauce Visual. Use them for cross-browser coverage and a review workflow.
5. Python/Selenium: compare against reviewed baselines with a diff library (e.g. Visual Guard, see tools-and-libraries).
6. Review flow: a diff fails the build → a human accepts the new baseline or files a bug. Never auto-accept.
