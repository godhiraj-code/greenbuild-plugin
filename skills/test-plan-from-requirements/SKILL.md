---
name: test-plan-from-requirements
description: Turn a user story, PRD, or ticket into a risk-based test plan with prioritized test cases, test data, and automation candidates.
---
# Test plan from requirements
1. Collect the input (story text, acceptance criteria, ticket link, mockups). If the acceptance criteria are missing, list the questions they leave open before writing cases.
2. Restate the feature in 2 to 3 lines, then list assumptions and out-of-scope items.
3. Risk analysis: for each area, rate likelihood x impact (H/M/L). Think about money, data loss, security, auth, and how much of the user journey it touches.
4. Derive cases with standard techniques: equivalence partitioning, boundary values, decision tables for rule combinations, state transitions for workflows, and error guessing. Add negative, permission, and accessibility (keyboard, labels) checks where they apply.
5. Output a table: ID | Title | Preconditions | Steps | Expected | Priority (P0-P3) | Type (UI/API/unit) | Automate? (Y/N plus reason).
6. Apply the test pyramid: push each check to the lowest layer that can catch it (unit < API < UI).
7. List test data needs and environment needs.
8. End with a coverage summary (criteria → case IDs) and the open questions.
