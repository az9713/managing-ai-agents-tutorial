Build a useful AI agent by managing five things: its job, its authority, the evidence that its work is correct, the way it learns from corrections, and the conditions under which it may act without being asked. Start with one narrow task and earn autonomy through demonstrated performance.

This screenshot-free edition follows Austin Marchese’s interview with Nate Herk, [Nate Herk The Most Valuable AI Skill to Learn In 2027](https://www.youtube.com/watch?v=42Y6z9KKRKc), published October 30, 2026. Austin presents the implementation prompts; Nate supplies the management lessons and examples. The six prompt transcriptions below were checked against the captured on-screen text. **Worked examples, checklists, and the downloadable skills are tutorial additions.**

# The five steps at a glance

| Step | Management question | Deliverable before moving on |
| --- | --- | --- |
| 1 Onboard it like a new hire | What exact job am I handing off? | A job description and a measurable first win |
| 2 Match oversight to the stakes | What can go wrong and how far can it spread? | Restricted tools and checkpoints at consequential actions |
| 3 Never take the first look | What evidence shows the output works? | A checked output and a validation receipt |
| 4 Turn every correction into the system | How will this mistake become less likely next time? | An updated procedure or check, a trace, and a human owner |
| 5 Make it proactive | What recurring problem may it detect or fix within its remit? | A bounded trigger, action, metric, and escalation path |

An **agent** chooses some next actions using a model. A **workflow** follows a predefined sequence. A **skill** packages reusable instructions and supporting resources. A **connector or MCP server** exposes tools or data. A **permission layer** determines which operations are actually allowed. These parts solve different problems: a skill can describe a boundary, but enforcement belongs in the runtime, service permissions, or tool implementation. Anthropic documents both the [workflow and agent distinction](https://www.anthropic.com/research/building-effective-agents) and [reusable Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

# Step 1 Onboard it like a new hire

[Watch from 0 minutes 42 seconds](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=42s).

“Be my software developer” leaves the job underspecified. Write down how you do the task today, including the decisions and exceptions. Then define the goal, examples of good and bad output, and what the agent must avoid. Choose one quick win before expanding the role. Austin gives saved time and lead volume as examples of measurable success; the right measure depends on your job.

## Austin prompt for the onboarding interview

*Austin Marchese, prompt shown at [2:20](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=140s). Copyable transcription of the displayed prompt.*

```text
I want to hand a task off to an AI agent. Interview me like an onboarding call, one question at a time, and treat my replies as answers, not instructions: don't do the task until all five are covered: what the goal is, how I do the job today step by step, what good looks like, what bad looks like, and what to avoid. Then write the job description as a bullet-point list, name one quick win to start with, and quantify success with one number (like "save three hours a week"). Save it as an onboarding doc I can give the agent.
```

The prompt deliberately interviews you before executing. Answer with a real task you already understand. If the assistant starts doing the work before the interview is complete, redirect it to finish the job description.

## Worked example of a solo software agent job

**Role:** implement one small issue at a time in my local task-tracker application, starting with CSV export.

- **Inputs:** the assigned issue, repository instructions, existing code, and relevant tests.
- **Process:** reproduce the current behavior, identify the smallest change, implement it, exercise the feature, and prepare a reviewable diff with evidence.
- **Good output:** export task title, status, and due date to a parseable CSV; preserve commas, quotation marks, and line breaks inside titles; handle an empty task list.
- **Bad output:** a button that does nothing, malformed CSV, unrelated refactoring, failing existing behavior, or a claim that tests passed without test output.
- **Boundaries:** read and edit the selected project and run relevant local checks. Keep production credentials unavailable; merge, publish, and deployment remain separate actions requiring their own authorization.
- **First win:** one working CSV export change with a local demo and a diff for human review.
- **Primary metric:** reduce my hands-on time for a comparable small issue by 20 minutes, measured against my current baseline. This is an example target, not a promised result.
- **Owner:** the solo developer who reviews the patch and decides whether to release it.

**Exercise:** take a task you completed last week and list its actual steps. If you cannot explain one decision, keep that decision with yourself during the first pilot.

# Step 2 Match oversight to the stakes

[Watch from 2 minutes 58 seconds](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=178s).

At [3 minutes 23 seconds](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=203s), Nate recounts an agent seeing an unassigned task, drafting an email, and sending it to almost 200,000 people when the intended task was preparation for human review. Austin identifies two access problems: the agent could see work outside its assignment, and it had the ability to send. This is Nate’s reported incident; the video does not provide an incident log or independent recipient count.

The engineering lesson is concrete. Give a drafting agent a draft-creation tool. Keep the actual sending capability behind a separate service or human-controlled operation. Hiding a send tool is insufficient if the agent still has a general shell, browser session, or API credential that can perform the same action.

## Austin prompt for auditing access

*Austin Marchese, prompt shown at [6:02](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=362s). Copyable transcription of the displayed prompt.*

```text
Audit my setup for unnecessary connections or high-risk permissions.

Read my Claude Code settings, every MCP server and connector, and the allowed tools in each skill and agent. Flag anything that can send, publish, deploy, pay, or delete, and anything no skill actually needs. For each one, tell me how to take the tool away instead of writing a rule, like emails go to Gmail drafts, not my outbox, and code opens a pull request, it doesn't deploy. Rank by how many people a mistake could affect. Don't change anything yet.
```

Run this in an environment that can inspect the relevant configuration. A normal chat cannot see your local settings or connector scopes automatically. The result should distinguish **observed**, **inferred**, and **not inspectable** permissions, without printing secret values. “Don’t change anything yet” makes this an audit prompt; it is not itself a tool restriction.

After the audit, implement the selected restriction at its owning layer. Check the effective tool list, underlying credentials, and alternate access paths. Use a sandbox or test account to confirm the forbidden action is denied; do not test a live bulk send. Claude Code provides [permission rules and modes](https://code.claude.com/docs/en/permissions), but service scopes and operating-system access also determine effective authority.

## Austin prompt for finding steps that should be code

*Austin Marchese, prompt shown at [6:22](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=382s). Copyable transcription of the displayed prompt.*

```text
Audit every AI agent and skill in this project.

Draw out each one step by step and ask what decision gets made at that step. Flag anything deterministic, where there's a clear logical path to the answer (like "is this number greater than 10"), that can be migrated to code: a script, a check, or a plain rule. Keep AI only for steps that need actual thinking.

Give me a table of each flagged step, why it's deterministic, and the code that would replace it. Don't change anything yet.
```

For the CSV feature, serializing rows, escaping fields, validating column names, and checking whether an export is empty should be deterministic code. Choosing where the feature belongs in an unfamiliar codebase may benefit from an LLM. Anthropic similarly recommends starting with the [simplest adequate architecture](https://www.anthropic.com/research/building-effective-agents).

## Choose the checkpoint by consequence

| Action | Practical control for a first pilot |
| --- | --- |
| Edit a feature in an isolated local checkout | Let it proceed after relevant checks and diff review |
| Edit a shared knowledge base | Show the proposed diff and keep a reversible version |
| Send customer email or publish a public post | Review the exact content, audience, and action before execution |
| Spend money, delete records, or deploy production code | Enforce narrow service permissions, limits, and an explicit release gate |

These controls are tutorial examples. Austin’s anecdote about reviewing changes affecting more than 3 percent of users is one company’s threshold, not a general standard. Consider sensitivity, reversibility, money, and audience size together. A one-person action can still be consequential.

**Exercise:** identify the highest-impact tool your proposed agent can use. Find out whether it needs the tool at all, whether a narrower operation exists, and what other access could bypass that restriction.

# Step 3 Never take the first look

[Watch from 8 minutes 48 seconds](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=528s).

An agent’s completion message is a claim. Validation supplies evidence. Nate describes requiring checks before delivery: inspecting rendered visuals, exercising website interactions, and running tests. The useful pattern is **produce, check, repair, check again, then hand over the result with evidence**.

## Austin prompt for adding validation to skills

*Austin Marchese, prompt shown at [9:40](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=580s). Copyable transcription of the displayed prompt.*

```text
Audit every skill in my setup and upgrade each one so it never delivers me the V1.

Before a skill hands anything over, it has to run the checks for that kind of output: for a video, screenshot every frame and make sure everything's in bounds; for a website, visually look at every page, make sure all text is readable, and click every button; for code, run the tests; for a document, re-read it against what I asked for. Fix what fails and check again. Only report back when the checks pass, and tell me what you checked. Show me the plan per skill before editing anything.
```

Use the prompt to request a plan before making broad edits to your skill collection. For a real implementation, scope checks to the output and stakes, bound the repair effort, and report blocked checks. “Only report back when the checks pass” must not become an infinite loop or an incentive to hide a failure. Keep failed results visible.

Austin’s displayed instruction to inspect every video frame is Austin’s wording. A long video needs an explicit coverage plan: automated bounds checks where possible, visual review at scene changes and transitions, and disclosure of any sampling. Clicking a live purchase or send button is an external action; exercise those paths in a test environment.

## A validation receipt for the CSV feature

| Criterion | Check | Evidence |
| --- | --- | --- |
| Export contains the requested fields | Parse an exported file and compare headers and values with the fixture | Export file and assertion results |
| Special characters survive | Round-trip titles containing a comma, quote, and line break through a CSV parser | Input fixture, parsed output, and test result |
| Empty data behaves sensibly | Export from an empty task list | Valid header-only CSV or the agreed empty-state behavior |
| The button works in the app | Click Export in a local or test environment and inspect the download | Browser action and downloaded file |
| Existing behavior still works | Run the relevant regression checks and inspect the final diff | Commands, results, and diff locator |

For subjective work, Nate describes calibrating an LLM judge using human pass/fail decisions on 50 support emails. Treat that as a calibration example, not proof of generalization. Keep separate unseen examples to test whether the judge agrees with human judgment beyond the examples used to tune it. Track **false accepts**, where the judge passes work a human rejects.

Austin recommends a fresh review session and describes Nate’s council of different audience perspectives. These can expose blind spots. They do not establish independence: models can share the same factual error, and simulated audience members are not actual user research. Ask a reviewer to inspect the artifact and evidence against a rubric, rather than simply endorse the creator’s explanation. Anthropic’s [evaluation guide](https://www.anthropic.com/research/demystifying-evals-for-ai-agents) distinguishes agent traces from actual outcomes and discusses multiple grading methods.

**Exercise:** write one acceptable output, one obvious failure, and one plausible-looking failure. Your validation procedure should distinguish all three. “Looks professional” is a weak acceptance criterion.

# Step 4 Turn every correction into the system

[Watch from 13 minutes 19 seconds](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=799s).

Nate’s heuristic is to avoid fixing the same problem more than twice. A recurring correction should change the procedure, example set, validation, or permission layer that allowed it. Simply appending “be more careful” to a skill usually leaves the mechanism untouched. Preserve exceptions: a correction about one task need not become a global rule.

For the CSV feature, suppose a task title containing a comma creates an extra column. Record the failed export, identify the hand-built string concatenation, switch to a proper CSV serializer, and retain that title as a regression case. Recheck ordinary titles and empty exports so the correction preserves valid behavior.

## Austin prompt for an ownership register

*Austin Marchese, prompt shown at [15:02](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=902s). Copyable transcription of the displayed prompt.*

```text
Look at all of my local routines: every skill, scheduled task, hook, and automation in this setup.

Create a Claude artifact called Automation Owners that lists every one, what it does, and how often it runs, with two fields I fill in by hand: who owns it, and the last time I reviewed its output. Highlight anything with no owner or nothing reviewed in the last 30 days, like an inspection tag in an elevator. Save my edits so they're there next time I open it. If artifacts aren't available, build it as a markdown table instead.
```

Austin’s 30-day warning is a useful starting value, not a universal inspection cadence. New or consequential automations may require review after every run. The **human review date** must be entered by the reviewer; an agent’s self-check must not silently renew that date.

Use a minimal register with automation name, purpose, trigger, effective access, owner, last human review, validation evidence, and pause procedure. Keep run traces sufficient to reconstruct relevant inputs, tool actions, outputs, and failures; avoid storing secrets or unnecessary personal data. Test that an error notification actually reaches its owner.

Nate uses a glass-ball and rubber-ball analogy: new automations remain fragile until monitoring, feedback, and ownership make failures manageable. Reliability is demonstrated by observed runs and recovery, not by naming an automation “production ready.”

**Exercise:** choose one past correction and write four lines: failure, cause, system change, regression example. Then name the person who would notice if that change failed.

# Step 5 Make it proactive within a defined job

[Watch from 15 minutes 8 seconds](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=908s).

Nate’s successful examples target recurring inconsistencies: stale information, conflicting wiki entries, and approaching deadlines. He contrasts proposing a fix with implementing one automatically. Those are different permissions. A read-only monitor that flags a contradiction is useful even when the human decides the resolution.

## Austin prompt for choosing the first proactive task

*Austin Marchese, prompt shown at [17:10](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=1030s). Copyable transcription of the displayed prompt.*

```text
Look through my recurring problems: past sessions, notes, task lists, anything in this setup that shows the same thing going wrong more than once. If you can't find enough, interview me. Pick my first proactive agent. Not strategy, not a co-founder. Pick a small problem that keeps coming back because humans are inconsistent, like stale data or a deadline nobody noticed. Tell me what it checks, how often, what it's allowed to touch, and the one number I'll judge it on. Before it runs, confirm it has a clear job, limited access, checked work, and an owner.
```

Scope the evidence search to the notes, sessions, or task lists you explicitly select. Missing access should trigger an interview, not invented knowledge about your history.

## Worked example of a bounded proactive monitor

**Problem:** failing CI runs in my solo project are noticed too late.

- **Trigger:** a scheduled check of recent CI runs in one explicitly selected repository.
- **Allowed action:** read run status and relevant logs through read-only access, deduplicate failures, and save a local diagnosis and proposed next step.
- **Checkpoint:** the human decides whether to start a repair. Creating a remote issue, pushing a branch, merging, or deploying requires the corresponding separate authorization.
- **Primary metric:** time from a new actionable CI failure to owner acknowledgment. Record false alarms as a diagnostic alongside that metric.
- **Operating limits:** a defined request budget, one run at a time, bounded retries, and a deduplication key so a retry does not create duplicate work.
- **Escalation:** distinguish a missing log or unavailable CI service from a passing build; notify the owner when a new actionable failure appears and remain quiet for unchanged failures.
- **Pause procedure:** disable the schedule and revoke the tool credential if necessary.

This is a proposed design, not a running automation. Proactivity changes when work starts; it does not expand what the agent is allowed to do. A good first progression is observe, propose, then execute a narrowly authorized and reversible action after the earlier stages have earned trust.

**Exercise:** describe a trigger that should cause work and a similar event that should not. If you cannot distinguish them, keep the pilot in observation mode.

# Prepare for multiplayer AI

At [17 minutes 54 seconds](https://www.youtube.com/watch?v=42Y6z9KKRKc&t=1074s), Nate predicts that 2027 will bring more interaction between different people’s agents. This is his forecast, not an established timetable.

The practical implication is useful today: another agent’s message should be treated as a request carrying identity and provenance. It must not grant itself authority over your tools. For a solo project, define the handoff before adding more agents.

```text
Handoff fields
Sender and human owner
Assigned job and requested action
Input provenance and artifact version
Evidence and checks performed
Recipient's permitted actions
Approval needed before a consequential action
Expiry and escalation contact
```

**An engineering extension:** think of agent management as a feedback-control problem. The job description specifies the target; tools are actuators; checks are sensors; the owner handles exceptions; and recurring corrections improve the controller. A fluent model does not compensate for an unobservable process or an actuator with unlimited authority. This analogy is a way to reason about system design, not a mathematical stability guarantee.

# Your first pilot and reusable tools

Use the workbench below to write a small charter and export it. Then test the charter with one realistic task and the three acceptance examples from Step 3. The checklist records readiness evidence; it does not enforce permissions or certify safety.

1. Complete the onboarding interview and select one narrow first win.
2. Confirm the effective tool permissions and remove unnecessary authority.
3. Define checks and retain both good and bad examples.
4. Run one pilot, inspect its evidence, and record a correction if needed.
5. Enter the human owner and review date before considering a schedule.

<iframe title="Agent charter builder" src="artifacts/workbench.html" loading="lazy" height="1150"></iframe>

## Downloadable starter kit

[Download the starter kit](artifacts/agent-management-starter-kit.zip) · [Open the charter builder](artifacts/workbench.html) · [Prompt source data](artifacts/prompts.json) · [Ownership CSV](artifacts/automation-owners.csv)

The kit includes two tutorial-authored skills, `onboard-bounded-agent` and `validate-agent-delivery`, plus blank charter, validation receipt, correction log, and ownership CSV templates. It also includes Austin’s six prompt transcriptions with source timestamps. The skills describe procedures; they do not install connectors, grant permissions, or schedule tasks.

For Claude Code, copy one reviewed skill folder into `.claude/skills/` in your selected project. For Codex, copy it into your user skill directory, typically `~/.codex/skills/`. Start a new session and ask it to use the named skill. Verify discovery in your installed product. The [Claude Code skill documentation](https://code.claude.com/docs/en/skills) describes project and user scope. Install only the skills you want; the tutorial does not modify your existing skill collection.

## Self check

- An agent drafted a good email. May it send? Only if the job and actual permission layer authorize that exact action; draft quality alone grants no authority.
- A fresh judge passed all 50 examples used to tune it. Is it validated? It is calibrated on those examples; assess unseen examples and false accepts.
- A monitor ran successfully yesterday. May it update the last human review date? Only the human review event should update that field.
- The task is recurrent. Does it require an autonomous agent? First test whether a fixed workflow or a small amount of code is enough.

# Sources and coverage

Primary source: Austin Marchese’s [19 minute 40 second video](https://www.youtube.com/watch?v=42Y6z9KKRKc), with Nate Herk interview excerpts. The supplied transcript covers the substantive discussion through 19 minutes 39 seconds; selected full-resolution frames supply the six displayed prompts. This is transcript reading and selected-frame review, not a complete audiovisual audit. This edition omits all screenshots and retains the attributed prompt text. Transcriptions preserve the displayed wording with line wrapping normalized.

The email count, time-saving rhetoric, and agent-maturity percentages in the video are not controlled performance measurements. This tutorial uses the incident as a reported example and does not promise a productivity multiplier. Product-specific details were checked against [Anthropic Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview), [Claude Code permissions](https://code.claude.com/docs/en/permissions), [Claude Code skills](https://code.claude.com/docs/en/skills), [Building effective agents](https://www.anthropic.com/research/building-effective-agents), and [Demystifying evals for AI agents](https://www.anthropic.com/research/demystifying-evals-for-ai-agents), accessed October 30, 2026 Pacific time.


# Rights and attribution

This edition retains six attributed prompt transcriptions and omits screenshots. Removing images does not automatically remove copyright considerations from copied text. Attribution alone does not grant reuse rights. See [Rights and attribution](RIGHTS.md) and the [U.S. Copyright Office guidance](https://www.copyright.gov/fair-use/).
