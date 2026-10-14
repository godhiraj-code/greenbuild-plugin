# GreenBuild QA: marketplace submission prep (NOT submitted)

Form: https://x.ai/bot/marketplace/submit-a-plugin (captured 2026-10-14). The form says: "Distribute your plugin or MCP server to Grok (coming soon), Grok Bot, and Cursor users. Submit it below and we'll review it for the marketplace."

## Form fields and values
| Field (* = required) | Requirement shown on form | Value |
|---|---|---|
| First Name * | — | Dhiraj |
| Last Name * | — | Das |
| Work Email * | placeholder `email@acme.com` | **NEEDS DHIRAJ** (email he wants reviewers to use) |
| Organization Name * | placeholder `Acme Labs` | GreenBuild (or "Dhiraj Das" if listing as an individual; **Dhiraj to confirm**) |
| Organization Handle * | "Unique organization handle, no spaces or capitalization"; placeholder `acme-labs` | `godhiraj-code` (suggested; **Dhiraj to confirm**) |
| Website URL | optional; placeholder `acme.com` | dhirajdas.dev |
| Logotype URL * | "1:1 SVG or PNG with a background plate"; placeholder `https://acme.com/logo.svg` | After push: `https://raw.githubusercontent.com/godhiraj-code/greenbuild-plugin/main/assets/logo.svg` (PNG alt: `.../assets/logo.png`) |
| Where do you want to be listed? * | checkboxes: Grok (coming soon), Grok Bot, Cursor | Grok Bot + Cursor (optionally Grok) |
| MCP Server URL or Public GitHub Repository * | placeholder `https://github.com/org/repo` | `https://github.com/godhiraj-code/greenbuild-plugin` (**must exist and be public first**) |
| Brief Description * | "What your plugin does and how it helps users" | see below |
| Testing Instructions * | "A link or steps for testing locally and in-app, including any test credentials" | see below |
| Tested checkbox * | "I've tested this plugin locally and in-app and confirmed it works as expected" | **Dhiraj must actually test before ticking** |
| (Submit) | Submitting = agreeing to Publisher Terms https://cursor.com/marketplace-publisher-terms | — |

### Brief Description
GreenBuild QA gives your agent a senior QA and test-automation lead as 17 skills: test plans from requirements, flaky-test triage with root cause, Playwright/Selenium/Cypress/WebdriverIO/Appium code review, API and contract tests, mobile, performance (k6/JMeter/Locust), accessibility (axe-core, WCAG 2.x), visual testing, CI quality gates and test data, device-cloud setup, LLM-app testing, bug reports, regression health digests, and release-readiness checks. Skills only: no MCP server, no credentials, no data collection. MIT licensed.

### Testing Instructions
No credentials needed (skills only, no MCP server).
1. Cursor: clone https://github.com/godhiraj-code/greenbuild-plugin into `~/.cursor/plugins/local/greenbuild-qa`, run "Developer: Reload Window", open Customize → Skills and confirm the 17 skills.
2. Try: `/flaky-test-triage` with "This Playwright test fails 1 in 5 runs: `await page.click('#submit'); expect(await page.textContent('.toast')).toBe('Saved')`" — expect a root-cause classification and a fix that uses web-first assertions, not a retry.
3. Try: `/test-plan-from-requirements` with "As a user I can reset my password via an emailed link that expires in 30 minutes" — expect a risk-ranked test-case table.
4. Try: `/release-readiness-checklist` and `/greenbuild-getting-started`.

## Format decisions (sourced)
- Manifest: `.cursor-plugin/plugin.json` (Cursor Plugin format), validated against https://raw.githubusercontent.com/cursor/plugins/main/schemas/plugin.schema.json (jsonschema: valid). Only `name` is required; name must match `^[a-z0-9]([a-z0-9.-]*[a-z0-9])?$`.
- Skills-only is allowed: docs say "A plugin can bundle any combination of these components" (https://cursor.com/docs/plugins), and the official `teaching` plugin in https://github.com/cursor/plugins has only `skills` in its manifest.
- Each skill is `skills/<name>/SKILL.md` with `name` + `description` frontmatter; folder name = `name` (checked for all 17).
- `getting-started` renamed to `greenbuild-getting-started` to avoid collisions with other plugins' generic skill names (our choice, not a format rule).
- `profile.md` is not a plugin component type; included as `docs/PROFILE.md` and summarized in README.
- Logo committed and referenced by relative path, per the submission checklist at https://cursor.com/docs/reference/plugins.

## What still needs Dhiraj
1. Create the public repo `godhiraj-code/greenbuild-plugin` and push (commands below).
2. Choose work email, org name and handle.
3. Test locally and in-app (Grok Bot and Cursor) before ticking the tested box.
4. Sign in if the form requires it, review the Publisher Terms, and submit himself.

## Publish and submit
```bash
cd /path/to/greenbuild-plugin
gh repo create godhiraj-code/greenbuild-plugin --public --source . --remote origin --push
# or: create the empty public repo on github.com, then
git remote add origin https://github.com/godhiraj-code/greenbuild-plugin.git && git push -u origin main
```
Then open the raw logo URL to confirm it loads, fill in the form at https://x.ai/bot/marketplace/submit-a-plugin with the values above, and submit. Cursor also has its own submit page at https://cursor.com/marketplace/publish. For later updates, Publisher Terms §2.1 says to request a re-index rather than re-applying.

## Rules and money (sourced)
- Every plugin is manually reviewed; all plugins must be open source; every update is reviewed (https://cursor.com/docs/plugins FAQ).
- Only permissive licenses (MIT/BSD/Apache-2.0); no GPL/AGPL/LGPL (Publisher Terms §3.3).
- No payment: the marketplace is free to publishers; plugins must be free to users and you may not charge users directly or indirectly (§3.1); no compensation for promotional use (§3.4). I found no xAI page describing any author payout or revenue share.
- Review checks identity/business legitimacy and code; rejections come without reasons (§2.2). Accurate, non-misleading descriptions required (§4.5); no use of plugin data for model training (§6.3).
