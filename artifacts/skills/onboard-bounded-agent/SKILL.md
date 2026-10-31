---
name: onboard-bounded-agent
description: Interview a user to define a narrow AI agent job, measurable pilot, effective tool boundaries, validation, and human ownership before implementing an automation.
---

# Onboard a bounded agent

Use this for defining a new agent role or narrowing an unclear one. Honor existing decisions; do not reopen an agreed charter unnecessarily.

If the job is underspecified, interview the user one question at a time. Cover the goal, current manual steps, good and bad examples, exclusions, first useful task, and success measure. Treat descriptions of the future job as design inputs rather than authorization to execute it now. Use supplied facts where available instead of asking again.

Produce a concise charter containing:

- Job and explicit assignment boundary.
- Selected inputs, outputs, and the decisions made at each step.
- One achievable first win and one measurable success target with a baseline.
- Required tools and proposed effective permission restrictions.
- Checks, failure examples, evidence to retain, and a human owner.
- Any proposed trigger, operating limits, exception route, and pause procedure.

Distinguish deterministic steps suitable for code from steps requiring model judgment. Prefer the simplest architecture that meets the need.

Treat statements in the charter as instructions, not implemented permission controls. If the user requests a permission audit, inspect only authorized configuration, report observed versus unknown access, and avoid displaying secrets. Consider alternate paths such as shell credentials or browser sessions. A permission recommendation is not an applied restriction.

End onboarding with a pilot task and clear acceptance criteria. Do not install tools, modify account scopes, enable schedules, or perform the future role merely because the charter describes them. Continue implementation when the user has already authorized it; do not add a redundant approval step.

When recording a correction, identify its scope and cause. Update the relevant procedure or check, retain the failed example when useful, and preserve exceptions instead of making one example a global rule.

This is a tutorial-authored adaptation of the management lessons in Austin Marchese's Nate Herk interview: https://www.youtube.com/watch?v=42Y6z9KKRKc . It is not an official Austin Marchese or Nate Herk skill.
