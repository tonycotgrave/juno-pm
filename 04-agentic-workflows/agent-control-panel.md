
# Agent Control Panel · Juno

> Module 4 · Deploy Agentic Systems and Workflows.
> Product: Juno – Decision Bottleneck Finder
> Status: Proposed configuration, subject to validation.

## What Juno Is Doing

Juno helps Product Managers at RocketShip identify decisions that are holding up delivery.

The focus is on the **risk flagging pillar**, specifically identifying outstanding decisions, technical blockers and missing information across Jira issues.

**Autonomy level:** Level 2 – Semi-Autonomous.

**Reasoning pattern:** ReAct (Reason, Act, Observe).

Juno can independently retrieve relevant Jira information, analyse potential decision bottlenecks and prepare recommendations without requiring instructions for every step.

However, Juno cannot approve decisions, change roadmap priorities or instruct engineering teams to change their work.

The Product Manager remains accountable for decisions and approvals.

The aim is to reduce the effort involved in identifying outstanding decisions while maintaining human oversight.

## Four Levers

### Stop Conditions (Loop)

Juno must stop when:

- It has completed the analysis and verified its findings.
- It reaches a maximum of five model turns per analysis.
- It has attempted to retrieve missing information three times without success.
- The required evidence remains insufficient or contradictory.
- The task requires a product decision or approval outside its authority.
- The analysis fails verification.

If Juno reaches a stopping condition before completing the analysis, it must explain why and identify what information or human action is required.

Juno must not retry indefinitely or continue making unsupported assumptions.

The five-turn and three-attempt limits are proposed settings from Module 3 and require testing.

### Structured Tool Outputs (Tools)

Every tool response must follow a consistent structure so Juno can distinguish successful retrievals from incomplete or failed requests.

Each Jira retrieval should return:

- **Status:** Success, Partial or Failed.
- **Issue ID:** The Jira ticket reference.
- **Source:** Jira issue or comment.
- **Retrieved at:** Timestamp of the information retrieved.
- **Evidence:** Relevant issue details or comments.
- **Missing information:** Any unavailable or incomplete information.
- **Error details:** The reason for a failed or partial retrieval, where known.

A failed retrieval must never be treated as a successful result.

Juno must not produce a complete analysis when some relevant issues could not be retrieved.

### Confidence Thresholds (Verification)

Juno should only identify an outstanding decision when the available Jira information provides supporting evidence.

Before presenting a finding, Juno must:

- Identify the source Jira issue.
- Provide evidence supporting the classification.
- Distinguish confirmed information from interpretation.
- Check whether the evidence is contradictory or incomplete.
- Avoid inventing decision owners, metrics or approval statuses.

Where evidence is insufficient, Juno must label the finding as **Insufficient Information** rather than guessing.

Juno should not use an unsupported numerical confidence percentage to justify its findings.

Any conclusion requiring human judgement must be referred to the Product Manager.

### North Star (Context)

**Juno's purpose is to make outstanding decisions visible so Product Managers can identify what is holding up delivery and determine what needs attention.**

At every stage, Juno should remain focused on:

- Identifying potential decision bottlenecks.
- Providing traceable supporting evidence.
- Highlighting missing information and uncertainties.
- Suggesting appropriate next steps.
- Keeping product decisions and approvals with authorised humans.

Juno must not drift into automatically reprioritising work, approving roadmap changes or directing engineering teams.

The success of Juno is measured by whether it helps Product Managers understand decision bottlenecks, not by how many decisions it makes independently.

## Rules of Engagement

### Agency Permission

**Level 2 – Semi-Autonomous.**

Juno may independently:

- Search authorised Jira projects.
- Retrieve relevant issues and comments.
- Investigate potential bottlenecks.
- Analyse available evidence.
- Generate findings and recommendations.

Juno must not independently:

- Modify Jira issues or statuses.
- Approve roadmap changes.
- Change product priorities.
- Commit to delivery dates.
- Send instructions or customer communications.

**Permission tiers:**

- Read: Auto, within authorised access.
- Draft: Auto, subject to verification.
- Write: Blocked.
- Send: Blocked.

These restrictions must be enforced through technical permissions, not solely through prompts.

### Access Control

Juno may only be used by authorised RocketShip Product Managers and other explicitly approved users.

Users may only analyse Jira projects and information they already have permission to access.

Juno must not bypass existing Jira access controls or retrieve restricted customer information.

The initial version is user-triggered and does not continuously monitor Jira.

### Fallback Protocols

If Juno encounters missing information, failed retrievals, contradictory evidence or a verification failure, it must:

1. Identify the problem.
2. Retry only within the agreed limits.
3. Stop the affected investigation if reliable evidence remains unavailable.
4. Clearly label incomplete findings.
5. Present verified findings separately where appropriate.
6. Explain what information or human action is needed to continue.

Juno must not hide failures, invent missing information or present incomplete findings as a complete analysis.

**Example:** If Juno is asked to analyse 20 Jira issues but can only retrieve 15, it must explicitly report that five issues were not analysed.

### Checkpoints

Human review is required when:

- Juno cannot establish whether a decision is genuinely outstanding.
- Supporting evidence is contradictory or insufficient.
- The requested action requires a product decision.
- A user asks Juno to change priorities or approve a roadmap item.
- Verification fails or the permitted analysis limits are reached.

A Product Manager must review recommendations before making decisions or taking action.

Marking a recommendation as reviewed does not constitute approval of the underlying decision.

Juno must never treat a claimed job title or authority as verified permission to perform a restricted action.

## Key Failure Mode

The main failure mode identified for this pilot is **Silent Failure**.

Juno could fail to retrieve or analyse some Jira issues but still produce a report that appears complete.

The control panel addresses this through:

- Structured tool outputs that identify unsuccessful retrievals.
- Explicit stop conditions.
- Evidence requirements before findings can be presented.
- Visible reporting of incomplete analyses.
- Human review where evidence is missing or uncertain.

Other risks include infinite loops, reasoning drift and unnecessary latency. These are addressed through bounded retries, a clear North Star and limits on model turns.

## Assumptions to Validate

This configuration is based on our Module 3 AI PRD and includes proposed settings rather than proven production behaviour.

Before implementation, we would need to validate:

- Whether Level 2 autonomy provides additional value over the current user-triggered prototype.
- Whether five model turns and three retrieval attempts are sufficient.
- Whether the structured tool outputs capture the information needed to detect failures.
- Whether the evidence requirements reliably distinguish decision bottlenecks from other blockers.
- Whether the control panel prevents unauthorised actions.
- Whether Product Managers find the resulting analysis trustworthy and useful.


