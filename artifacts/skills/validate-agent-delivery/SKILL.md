---
name: validate-agent-delivery
description: Validate an agent-produced artifact against explicit acceptance criteria, retain observable evidence, and report passed, failed, and blocked checks before delivery.
---

# Validate an agent delivery

Use for checking an actual output before handing it over or for defining a missing validation layer. Scale checks to the artifact and consequence; ordinary reversible edits do not need an elaborate review process.

Recover the user's acceptance criteria and inspect the actual output. Use observable outcomes rather than the creator's completion claim. If crucial criteria are missing, infer routine ones from the request and identify material assumptions.

Select checks that can expose likely failures:

- Documents: inspect requirements, facts, citations, and rendered readability where relevant.
- Websites: exercise meaningful interactions and inspect representative wide and narrow layouts; test consequential actions in a sandbox.
- Code: run relevant existing checks and verify the affected behavior. Add tests only when they provide meaningful coverage.
- Media: define temporal and visual coverage, inspect transitions or frames as appropriate, and disclose sampling and unavailable audio checks.
- Subjective outputs: apply explicit criteria with concrete reasons. If using an LLM judge, distinguish calibration examples from held-out evaluation and identify false accepts.

A fresh review context can expose errors but does not prove independence or correctness. Do not spawn reviewers unless delegation is authorized and warranted. Do not give the creator's rationale priority over artifact evidence.

Repair in-scope failures and rerun affected checks. Stop repeated repair when the same blocker persists or the agreed effort limit is reached; report the unresolved issue rather than hide it, loop forever, or weaken the criterion to obtain a pass.

Return a compact receipt: artifact version or path, criterion, check performed, observed result, evidence locator, failed or blocked checks, and any human decision needed. Keep automated validation time distinct from the last human review date. Do not claim a test ran if it was only proposed, or a screenshot was inspected if it was only captured.

Validation does not authorize sending, publishing, spending, deleting, deploying, or changing permissions. Perform a release action only within the user's established authorization.

This is a tutorial-authored adaptation of the validation lesson in Austin Marchese's Nate Herk interview: https://www.youtube.com/watch?v=42Y6z9KKRKc . It is not an official Austin Marchese or Nate Herk skill.
