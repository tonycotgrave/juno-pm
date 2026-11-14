# Prototype · Juno

> Module 2 · Prototype. Juno – Decision Bottleneck Finder.

## Prototype link

[Juno – Decision Bottleneck Finder](https://juno-evidence-review.lovable.app)

Built using Lovable with five fictional Jira issues.

---

## What it demonstrates

The Decision Bottleneck Finder is designed to help Product Managers at RocketShip identify decisions that are holding up delivery.

**Who:** Product Managers at RocketShip.

**Where:** A simple dashboard showing outstanding decisions and blockers, with the potential to integrate into Jira in the future.

**What it takes off my plate:** Manually searching through Jira tickets to identify outstanding decisions, understand what's blocking progress and work out who needs to take action.

**Hypothesis:** If Juno can identify outstanding decisions, missing information and blockers from Jira issues, Product Managers should be able to understand what needs attention more quickly.

The prototype allows a Product Manager to:

- Analyse a small set of sample Jira issues.
- Identify issues requiring decisions, technical blockers and issues with insufficient information.
- View the evidence behind each finding and the suggested next steps.
- Mark recommendations as reviewed.
- See that roadmap changes require human approval.

The prototype uses fictional data and predefined rules rather than live AI or Jira integration. It demonstrates the intended workflow, not a fully functioning AI agent.

---

## Debrief

### What worked

- Lovable generated a working prototype from the initial prompt without requiring coding.
- The prototype classified five sample issues: two requiring decisions, one technical blocker, one with insufficient information and one progressing normally.
- Supporting evidence was visible, allowing users to understand why an issue had been flagged.
- The review function worked and updated the number of recommendations reviewed.
- The roadmap approval restriction worked, preventing unauthorised changes.
- My classmate was able to navigate the prototype, view the Jira issues and understand the purpose of the bottleneck analysis.

### What broke / felt like a toy

- My classmate wasn't initially sure whether to select a ticket or click Analyse bottlenecks.
- The purpose of Analyse again wasn't immediately clear.
- The issues were displayed in ticket number order, but it wasn't obvious whether this represented a priority ranking.
- The current layout could become difficult to use with hundreds of tickets.
- My classmate noticed that author, role and date information were missing from the sample tickets.
- It wasn't clear whether PMs would need to visit the dashboard or whether findings could be surfaced through Slack or Teams.
- The prototype uses five predefined scenarios rather than live Jira data or AI analysis.
- We haven't yet demonstrated whether it saves Product Managers time.

### What I'd change next pass

**1. Improve the starting experience**

Make it clearer what users should do first and explain when to rerun an analysis.

**2. Make outstanding decisions easier to find**

Introduce filtering and searching so users can focus on issues requiring decisions. Clearly distinguish the display order from business priority.

**3. Improve the supporting evidence**

Include ticket metadata such as author, role and dates when available.

**4. Fit into existing workflows**

Explore a read-only Jira integration and consider whether important bottlenecks could be surfaced through Slack or Teams.

**5. Validate the value**

Test with Product Managers using a larger and more realistic set of issues. Compare the time taken to identify decision bottlenecks manually against using Juno.

---

## What I learnt

The prototype helped turn an idea into something that could be tested rather than just discussed.

The classmate feedback highlighted that although the analysis was understandable, the starting workflow and some interactions weren't immediately obvious.

It also raised useful questions about how the product would scale and how it would fit into a Product Manager's normal working day.

The most important learning is that Juno should help make decisions visible, not make the decisions itself.

The prototype demonstrates a possible solution, but further testing is needed to establish whether it genuinely reduces the effort involved in identifying and resolving decision bottlenecks.
