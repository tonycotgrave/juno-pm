
# AI PRD · Juno

> Module 3 · Harness / AI PRD  
> Product: Juno – Decision Bottleneck Finder  
> Harness archetype: Tool-Using Copilot  
> Status: Proposed requirements – subject to validation

## Problem & user

RocketShip is a growing B2B SaaS company experiencing Signal Collapse. As the volume of customer requests, Jira issues and escalations increases, Product Managers are finding it difficult to identify outstanding decisions and understand what is holding up delivery.

Product Managers currently need to review multiple tickets and conversations to understand which issues require decisions, what information is missing and who needs to take action.

Juno's Decision Bottleneck Finder aims to reduce this manual effort by identifying potential decision bottlenecks, presenting supporting evidence and recommending next steps.

**Primary users:** Product Managers at RocketShip.

**User problem:** Important decisions can remain unresolved because they are buried within large volumes of delivery information.

**Hypothesis:** If Juno can identify outstanding decisions, missing information and blockers from Jira issues, Product Managers will be able to understand what needs attention more quickly.

**Harness archetype:** Tool-Using Copilot.

This approach allows Juno to retrieve information, analyse issues and draft recommendations while keeping Product Managers responsible for final decisions.

**Current evidence:** In Module 2, I built a Lovable prototype using five fictional Jira issues and predefined analysis rules. The prototype demonstrated the intended workflow, but did not test live AI analysis or integration with Jira.

---

## Assumptions & Decisions to Validate

RocketShip is a fictional company used for this project. Where specific business, technical or operational information hasn't been provided, I've made working assumptions to support Juno's design.

These assumptions are not confirmed requirements or proven capabilities. They would need validation with the relevant stakeholders before implementation.

| Area | Working assumption | Validation needed |
|---|---|---|
| Business context | Jira is the primary system for tracking delivery issues and outstanding decisions. | Confirm with Product Management and Engineering. |
| Supporting information | Approved product guidance and decision-making responsibilities are maintained in Notion. | Confirm ownership, availability and accuracy of the information. |
| User access | Product Managers have appropriate access to Jira projects within their areas of responsibility. | Review permissions with IT and Security. |
| Harness archetype | A Tool-Using Copilot is appropriate because PMs need evidence and retain responsibility for decisions. | Validate through user testing and technical review. |
| Source freshness | Jira changes should be reflected in a new analysis within five minutes. | Confirm integration capabilities and business requirements. |
| Loop limits | Five model turns and three retrieval attempts provide a reasonable balance between reliability and cost. | Test using realistic workflows and failure scenarios. |
| Performance | A response-time target of 10 seconds at the 95th percentile is appropriate for an interactive dashboard. | Validate through performance and usability testing. |
| Cost | A maximum model processing cost of $0.15 per standard analysis is commercially reasonable. | Test actual model usage and confirm operating budgets. |
| Memory | Analysis results can be retained for 30 days and review status for 90 days. | Confirm retention requirements with Data Governance and Security. |
| Permissions | Juno should have read and draft capabilities only, with system changes and external communications blocked. | Confirm governance and operational requirements. |
| Verification | Every finding should have traceable evidence and unsupported conclusions must be flagged. | Define and test evaluation criteria with Product Managers. |

### What we've validated so far

The Module 2 prototype demonstrated that:

- Five fictional Jira issues could be presented and classified using predefined rules.
- The prototype identified two issues needing decisions, one technical blocker, one issue with insufficient information and one progressing normally.
- Supporting evidence was displayed alongside the findings.
- Recommendations could be marked as reviewed.
- The interface prevented roadmap approval.

Peer testing showed that the prototype was understandable but highlighted improvements needed around initial guidance, the purpose of rerunning analysis, issue ordering and scalability.

We have not yet demonstrated that an AI model can reliably analyse live Jira data or that Juno reduces Product Manager workload.

---

## The harness

| Surface | Proposed specification |
|---|---|
| 01 Context | Retrieve current, authorised Jira information and relevant approved Notion guidance. |
| 02 Tools | Search and read issues, retrieve guidance, analyse bottlenecks and draft recommendations. |
| 03 Loop | Maximum five model turns per analysis, with up to three retrieval attempts. |
| 04 Memory | Retain analysis history, source references and review status at the appropriate team scope. |
| 05 Permissions | Read: Auto; Draft: Auto; Write: Blocked; Send: Blocked. |
| 06 Verification | Validate supporting evidence, source references, uncertainty and decision-making boundaries before presenting findings. |

## 01 Context · Data Requirements

### Required sources

Juno needs access to:

- Jira issue IDs, descriptions and current statuses.
- Jira comments, dependencies and recorded blockers.
- Assigned owners and decision owners, where available.
- Relevant approved product policies and decision-making guidance stored in Notion.
- Juno's approved skill file, defining its role, constraints and expected output.

Juno must only retrieve information relevant to the current analysis and within the requesting user's authorised access.

### Deliberate exclusions

The initial version will exclude:

- Jira projects the requesting user cannot access.
- Unrelated or unnecessary historical tickets.
- Customer information that is not required for the analysis.
- Informal Slack conversations presented as confirmed decisions.
- Unapproved or unverified product guidance.

These exclusions are intended to reduce unnecessary context, minimise cost and avoid unsupported conclusions.

### SLA of truth

**Proposed requirement:** A new analysis should reflect Jira changes within five minutes of those changes becoming available through the integration.

Juno must retrieve the latest available Jira information when starting a new analysis and display the source retrieval timestamp.

The five-minute target is an assumption that requires technical validation.

### When a source is unreachable

If Jira or another required source is unavailable, Juno must:

1. Identify which source could not be accessed.
2. Explain how this affects the analysis.
3. Provide a partial result only where sufficient verified evidence remains available.
4. Clearly label incomplete findings.
5. Avoid inventing missing information or presenting historical results as current.

Where reliable analysis is not possible, Juno must stop and request human review.

---

## 02 Tools · System Capabilities

### The verb list

| Tool or action | Side-effect class | Purpose |
|---|---|---|
| Search Jira issues | Read | Find relevant issues within authorised projects. |
| Retrieve Jira issue details | Read | Obtain descriptions, statuses, owners and dependencies. |
| Retrieve Jira comments | Read | Identify discussions about blockers and outstanding decisions. |
| Retrieve approved Notion guidance | Read | Understand relevant product policies and decision-making responsibilities. |
| Generate bottleneck analysis | Draft | Identify possible decision bottlenecks, technical blockers and insufficient information. |
| Generate recommendations | Draft | Suggest appropriate investigation steps for Product Manager review. |
| Generate review summary | Draft | Present findings, supporting evidence and missing information. |

Juno must distinguish between:

- Issues requiring product decisions.
- Technical or operational blockers.
- Issues with insufficient information.
- Work progressing without an identified blocker.

All findings must be traceable to supporting evidence.

### Deliberate omissions

Juno will not be given tools to:

- Approve or change roadmap priorities.
- Update Jira issues or statuses.
- Commit to delivery dates.
- Instruct engineering teams to stop or change work.
- Send external communications.
- Approve decisions on behalf of a Product Manager.

These exclusions reflect our decision to prioritise human control over autonomous action.

The interface may allow a Product Manager to mark an analysis recommendation as reviewed. This is a separate, controlled application action and does not change the Jira issue or constitute approval of a product decision.

---

## 03 Loop · AI Costs & Latency

### Turn ceiling

**Proposed requirement:** Maximum five model turns per analysis request.

Juno may make up to three retrieval attempts when information is unavailable. These attempts must remain within the overall model-turn limit.

A model turn is an interaction with the AI model during the analysis workflow.

Juno must not continue retrying indefinitely.

These limits are initial assumptions requiring testing.

### Escalation trigger

Juno must stop and return control to the Product Manager when:

- Required information cannot be retrieved within the permitted attempts.
- Information is contradictory or insufficient.
- A decision owner cannot be verified.
- The requested action exceeds Juno's authority.
- An analysis cannot pass verification.
- The maximum turn limit is reached.

Juno must explain why it stopped and identify what information or human action is required.

### Latency and cost target

Proposed initial targets:

- 95% of standard analysis requests should complete within 10 seconds.
- Model processing costs should not exceed $0.15 per standard analysis.
- No analysis should exceed five model turns.

These are proposed performance and cost budgets rather than measured results.

Engineering testing will be required to assess whether they are achievable and appropriate.

The system should monitor response times, processing costs, failures and incomplete analyses.

---

## 04 Memory · Data Requirements

### What persists, at what scope

| Information | Scope | Proposed expiry |
|---|---|---|
| Current analysis context | Request | Deleted when analysis completes. |
| Temporary interaction context | Session | Deleted when the session ends. |
| Analysis history and source references | Team | 30 days. |
| Recommendation review status | Team | 90 days. |
| Source retrieval timestamps | Team | Retained with the relevant analysis. |
| Confirmed product decisions | Source system | Managed according to the relevant system's retention policy. |

Analysis history and review status must only be accessible to authorised members of the relevant product team.

Juno should avoid retaining unnecessary customer information or complete copies of Jira records.

### Expiry

Temporary information should expire automatically according to the defined retention periods.

Juno must not assume that historical findings remain accurate. A new analysis must retrieve current source information.

The proposed retention periods require Data Governance approval.

### Write rules

Juno may produce analysis results and recommendations, but cannot independently modify confirmed product decisions.

The application may record that a Product Manager has reviewed a recommendation.

Marking a recommendation as reviewed must not be interpreted as approving a roadmap change.

Where an authorised human decision conflicts with Juno's recommendation, the human decision takes precedence.

Approved decisions must be maintained in the appropriate system of record, not inferred or invented by Juno.

---

## 05 Permissions · AI Risks & Mitigations

| Side-effect class | Tier |
|---|---|
| Read | Auto – authorised sources only. |
| Draft | Auto – recommendations for review. |
| Write | Blocked. |
| Send | Blocked. |

### Justification

**Read – Auto**

Juno can retrieve relevant Jira and Notion information without requesting approval for every query, provided access is restricted to sources the user is authorised to view.

Reading information doesn't modify source records, although privacy and security controls are still required.

**Draft – Auto**

Juno can generate analyses and recommendations automatically because these outputs remain subject to human review.

Incorrect recommendations can be corrected, but they could still influence decisions. Verification is therefore required before results are displayed.

**Write – Blocked**

Juno cannot change Jira issues, roadmap priorities or approved decisions.

An incorrect write action could affect multiple teams and delivery commitments.

The initial version will therefore have no AI-accessible tools that can perform these actions.

**Send – Blocked**

Juno cannot independently send messages, issue instructions to engineering or make commitments to customers.

Incorrect communications may have consequences that cannot easily be reversed.

Any future write or send capability would require additional governance, technical controls and explicit authorisation.

Juno must never treat a claimed job title or conversational statement as verified approval.

---

## 06 Verification · AI Testing & Measurement

### The check before output ships

Before displaying an analysis, Juno must:

1. Confirm that each finding references a valid retrieved Jira issue.
2. Check that supporting evidence exists in the source information.
3. Distinguish confirmed facts from interpretations and recommendations.
4. Avoid inventing issue details, decision owners, metrics or approval statuses.
5. Identify missing or contradictory information.
6. Confirm that recommendations are not presented as approved decisions.
7. Present the issue, supporting evidence, identified blocker, missing information and suggested next step.

### Proposed verification bar

- 100% of displayed findings must include valid source references.
- No invented customer metrics, decision owners or approval statuses are acceptable.
- No unauthorised roadmap changes or external communications are permitted.
- Findings with insufficient evidence must be labelled as such.

These are proposed requirements that must be validated through testing.

### Failure behaviour

If verification fails, Juno must not present an unsupported finding as confirmed.

Instead, it should:

- Identify the affected issue.
- Explain what could not be verified.
- Request additional information or Product Manager review.
- Present verified findings separately where appropriate.
- Clearly label the analysis as incomplete.

The system must not silently drop failed results or hide verification errors.

---

## Eval plan

Module 5 will define a human evaluation rubric and pass threshold in `05-evals/human-rubric.md`.

The evaluation will assess:

- Correct identification of decision bottlenecks.
- Distinguishing product decisions from technical blockers.
- Handling incomplete or contradictory information.
- Accuracy and traceability of supporting evidence.
- Refusal of unauthorised actions.
- Clarity and usefulness of recommendations.

The evaluation will include normal cases, ambiguous cases and attempts to override Juno's constraints.

The final pass criteria will be established during Module 5.

---

## Out of scope

The initial version of Juno will not:

- Make or approve product decisions.
- Change roadmap priorities or Jira records.
- Commit engineering teams to delivery dates.
- Send instructions or customer communications automatically.
- Monitor Jira continuously without a user request.
- Access unauthorised or irrelevant information.
- Automatically rank issues by business priority without sufficient evidence.
- Replace Product Manager judgement.

These exclusions are deliberate product decisions rather than missing functionality.

The initial focus is a controlled, evidence-based copilot that helps Product Managers identify outstanding decisions and understand what needs attention.

Additional automation may be considered in future iterations after reliability, usefulness and appropriate oversight have been demonstrated.
