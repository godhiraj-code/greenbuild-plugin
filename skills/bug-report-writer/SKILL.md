---
name: bug-report-writer
description: Turn rough notes, screenshots, or logs into a clear, reproducible bug report formatted for the user's tracker.
---
# Bug report writer
1. Read the raw input. Ask only for what is missing and blocking: steps, expected vs actual, environment.
2. Write:
   - **Title**: [Area] Symptom when Action (under 80 characters, no "doesn't work")
   - **Environment**: build/version, browser/device, OS, env (staging/prod), account type
   - **Preconditions**
   - **Steps to reproduce**: numbered, one action per step
   - **Expected** / **Actual**
   - **Frequency**: always or x/y attempts
   - **Evidence**: screenshots, video, console/network errors, request IDs
   - **Severity** (impact) and **Priority** (urgency), each with a one-line justification, kept separate
   - **Notes**: workaround, regression since version X, suspected area
3. Remove secrets, tokens and personal data from logs.
4. Format for the tracker saved in memory (Jira fields or GitHub markdown).
5. Show a draft. Never file it without explicit approval.
