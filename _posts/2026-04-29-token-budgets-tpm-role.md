---
layout: post
title: "From Story Points to Token Budgets: AI Is Redefining the TPM Role in 2026"
date: 2026-04-29
categories: ai
excerpt: "How agentic coding tools, token budgets, and context debt are changing sprint planning and the TPM role."
image: /assets/images/tpm-agent-workflow.png
---

![The Agentic TPM Workflow]({{ site.baseurl }}/assets/images/tpm-agent-workflow.png)

The year 2026 has brought a fundamental shift in how we build software. For years, TPMs measured progress using Story Points but recently Story Points are slowly losing their dominance. In a world where tools like Claude Code, GitHub Copilot (Agent Mode) can generate, test, and refactor thousands of lines of code in a single "loop," measuring human hours is no longer enough.

We are entering a new era, an era of Token-Aware Sprint Planning.

## 1. Velocity Reimagined: The Agentic Throughput
In the traditional Scrum framework, velocity was a metric tied directly to human effort, capacity planning, and story points. In 2026, velocity has fundamentally shifted: it is now a measure of **Orchestration Efficiency**. 

> **The TPM Paradigm Shift:** We have moved entirely past the legacy question, *“How long will it take a developer to write this code?”* Instead, the modern TPM asks, *“How many reasoning loops — **Think (Planning) → Act (Implementation) → Observe (Validation)** — will the agent require to hit our Definition of Done (DoD)?”*

As the engineering bottleneck moves from the syntax of writing code to the architecture of verifying logic, our communication methods and tracking metrics must evolve in tandem to support this agentic workflow.

---

### The New Input: Documents as System Prompts

When code generation is offloaded to an autonomous agent like Claude CLI, product requirements and system architectures become the new programming languages. Writing documentation is no longer administrative overhead; it is **functional prompt engineering at production scale**. 

#### Jira Stories as Execution Prompts
A user story is no longer just a reminder to have a conversation. To an AI agent, it is an execution prompt with strict context boundaries. If a Jira ticket is vague or lacks sharp acceptance criteria, it acts exactly like a syntax error in traditional programming. The agent will not pause to ask clarifying questions; it will attempt to execute anyway, burning through token budgets guessing human intent and adding delay to the completion of the Jira story.

#### Confluence Pages as Context Injection
Technical specifications must serve as clean, structured vector spaces for the agent. A pristine Confluence page explicitly maps repository boundaries, architectural guardrails, and third-party dependencies. When our documentation is immaculate, the agent’s initialization phase is flawless, drastically reducing the time spent in downstream debugging.

---

### The Workflow in Action: Asynchronous Orchestration

To see how this infrastructure functions, consider a standard engineering task where a local Claude CLI agent is spun up to address a complex Jira issue:

1. **Context Gathering (MCP Layer)** Claude CLI leverages the Atlassian Rovo MCP server to securely pull full context directly from the assigned Jira ticket and its linked Confluence technical specification. *(Duration: Seconds)*

2. **Architectural Planning & HITL Review** Recognizing that the implementation spans multiple repositories, Claude designs a plan to break the monolithic ticket into smaller subtasks to ensure clean, isolated Merge Requests (MRs). Because the developer might be working asynchronously, Claude CLI posts this structured breakdown as a comment on the main Jira ticket, tagging the developer for review. *(Duration: Minutes)*

3. **Cross-Agent Collaboration (A2A Bus)** Once the developer drops an approval comment on the ticket, Claude CLI coordinates with the Rovo AI agent via the Agent-to-Agent (A2A) protocol to automatically generate, map, and link the new subtasks inside the platform. *(Duration: Automated)*

---

### The New Outputs: Agentic Engineering Metrics

With an autonomous, multi-agent infrastructure driving execution, tracking engineering progress requires a brand-new set of telemetry data.

#### 1. The "Verification" Metric & Reasoning Loops
In 2026, burndown charts no longer track simple "To Do" to "Done" transitions. Instead, they track automated **Verification Loops**. When an agent pushes code, the CI/CD pipeline runs instantly. If a ticket was poorly thought through during the creation phase, the agent will get trapped in an expensive reasoning loop—repeatedly editing code only to watch the pipeline tests fail. When this threshold is breached, the work item automatically re-opens ("burns up"), signaling to the TPM that a vague prompt has derailed the agent and requires immediate human intervention to clarify the scope.

#### 2. Rovo Insights: The HITL Delay
Jira’s native analytics now include a **Human-in-the-Loop (HITL) Delay** metric. This tracks the time high-velocity agentic output sits idle in a terminal or code review queue waiting for human validation. By monitoring the HITL delay, TPMs can pinpoint the team’s true modern bottleneck: the hand-off friction between AI execution speed and necessary human oversight.

## 2. Budgeting for AI: Tokens as a Capital Expense (CapEx)

For years, TPMs have managed traditional cloud budgets like AWS and Azure. Now, we have to treat AI token usage the same way—as a direct infrastructure cost.

Think of token consumption as the new "Cloud Bill." If an AI agent gets stuck in an infinite "reasoning loop" trying to figure out messy, circular code logic, it can easily burn through hundreds of dollars in API costs in a single hour.

### Token-Aware Planning

During sprint planning and grooming, the new standard question is: Is this task "Agent-Ready"?

**The Issue:** If a codebase is disorganized (high Context Debt), the AI agent has to read through way more files just to understand what's going on before it can write code.

**The Goal:** TPMs need to focus on cleaning up the codebase, making sure it isn't just readable for humans, but highly efficient for AI models to parse.

### The Core Problem: Context Rot and Overload
Even though modern AI models have massive context windows (like 200k+ tokens), a production codebase is still too massive to fit all at once. The agent has to scan the repo and choose what to look at.

**Context Rot:** If your code has redundant files, deep folder structures, or confusing names, the agent fills its limited active memory with useless "noise."

**The Result:** The agent gets distracted, loses track of the actual goal, or forgets project standards halfway through the task because its memory window is entirely full of irrelevant code.

### The TPM’s New Grooming Checklist

As a TPM, you aren't just grooming user stories anymore; you are managing the Context Infrastructure. Your cleanup checklist focuses on three main areas:

### A. Cutting Down "Context Debt"

Humans are great at ignoring background noise; AI agents are not. A human engineer can easily spot and ignore a 500-line "deprecated" file. An AI agent will read it, assume it is the active project standard, and accidentally build brand-new features on top of dead code.

- **TPM Action:** Prioritize engineering tickets that explicitly delete dead code and flatten deep, messy folder structures. The cleaner the repository, the fewer tokens an agent wastes trying to find the right file.

### B. Writing "Machine-Readable Specs"
AI agents work best when they have dedicated, localized instruction files right in the repository root (like a CLAUDE.md or AGENTS.md). Treat these as "hot memory" cheat sheets for the AI.

- **TPM Action:** Make sure these files include clear Architectural Decision Records (ADRs). If the AI agent instantly reads why your team chose one specific protocol over another, it won't waste expensive reasoning loops trying to build the wrong solution.

### C. Cleaning Up the "Retrieval Path"
Agents use tools (like Model Context Protocol servers) to search your codebase. If your function names are generic (like checkUser()), a search returns dozens of confusing results that bloat the prompt. If they are hyper-specific (like validateUserCheckoutSession()), the tool grabs the exact right snippet instantly.

- **TPM Action:** Enforce strict, descriptive naming standards across the team. This slashes the number of search loops the AI has to perform, saving the company direct money on token costs.

## Conclusion

The shift from Story Points to Token Budgets isn’t just a change in metrics; it is a fundamental evolution of the TPM role. In the agentic era, our value is no longer in manual status updates or “nudging” tickets through a workflow.

The 2026 TPM is a Context Architect. Our success is defined by how well we groom our technical environment—reducing Context Debt and enforcing semantic standards—so that our hybrid teams of humans and agents can operate with maximum clarity. By treating tokens as a capital expense and codebase structure as infrastructure, we aren’t just shipping code faster; we are building a more predictable, scalable, and intelligent development lifecycle.
