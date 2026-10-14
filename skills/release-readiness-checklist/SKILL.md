---
name: release-readiness-checklist
description: Produce an evidence-based go/no-go release readiness report covering test results, open defects, coverage of changes, and risks.
---
# Release readiness checklist
1. Identify the release (version, scope or changelog, target date) and the go/no-go criteria saved in memory. If there are none, propose defaults: no open P0/P1, regression pass rate of at least 98% with failures explained, and every new feature has test evidence.
2. Gather evidence: latest regression and smoke results, open defects by severity, the list of changes (PRs/tickets), and performance or security scan results if they exist.
3. Map the changes to tests: for each change, record Tested? (Y/N/partial) and the evidence link.
4. List what was **not** tested, and why.
5. Risks: top risks, each with a mitigation (feature flag, monitoring, rollback plan).
6. Output: **Recommendation: GO / GO with conditions / NO-GO** at the top, then a checklist table (criterion | status | evidence), open blockers, and rollback readiness.
7. The final decision belongs to the human. Never state "approved" yourself.
