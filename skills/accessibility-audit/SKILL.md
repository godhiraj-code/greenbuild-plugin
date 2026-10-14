---
name: accessibility-audit
description: Run automated axe-core accessibility checks in Playwright, Selenium, or Cypress against WCAG 2.x A/AA, triage violations, and list the manual checks automation cannot cover.
---
# Accessibility audit
1. Scope: the pages and states (modals, errors, logged in), plus the target level (default WCAG 2.1/2.2 AA).
2. Playwright TS (`npm i -D @axe-core/playwright`):
```ts
import AxeBuilder from '@axe-core/playwright';
test('home is accessible', async ({ page }) => {
  await page.goto('/');
  const r = await new AxeBuilder({ page })
    .withTags(['wcag2a','wcag2aa','wcag21a','wcag21aa'])
    .exclude('#third-party-widget').analyze();
  expect(r.violations).toEqual([]);
});
```
3. Python options: `axe-playwright-python` (Playwright) or `axe-selenium-python` (Selenium). Cypress: `cypress-axe` (`cy.injectAxe(); cy.checkA11y()`).
4. Triage each violation by rule id, impact (critical/serious/moderate/minor), the affected nodes and the fix hint. Group violations by component, because one fix in a shared component often clears dozens.
5. Gate CI on critical and serious violations first. Keep a dated baseline for the rest and burn it down instead of ignoring it.
6. Automation catches only part of WCAG. Always add manual checks: keyboard-only navigation and focus order, visible focus, screen reader pass (NVDA/VoiceOver), zoom to 200% and reflow at 320px, meaningful alt text and link text, captions, and errors announced.
7. Output: a summary table (rule | impact | count | pages | fix), a manual checklist, and a suggested test to add.
