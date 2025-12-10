# n8n: LLM vs. AI Agent vs. Agentic Competitor Research System  
_AI-driven weekly competitor monitoring built as an AI Product Manager_

The goal of this project is to demonstrate, from an **AI Product Management perspective**, how to:

- Design **LLM-powered workflows** vs **agentic workflows** vs **full AI agents**
- Balance **orchestration vs autonomy** in production systems
- Operationalize **competitive intelligence** with minimal engineering support

---

## 🎯 Product Goal

Build a **repeatable, low-maintenance system** that:

- Runs **weekly** (or on demand)
- Pulls **recent competitor updates** (last 30 days)
- Focuses on:
  - New products / features / capabilities  
  - Pricing / packaging / partnerships / integrations  
  - Credible third-party analysis & press  
  - User complaints / sentiment (e.g., Reddit, forums)
- Delivers:
  - A **standardized markdown report** for archival & reference  
  - A **scannable HTML email summary** for stakeholders  

Primary objective from an AI PM lens:  
> Reduce manual research time, **increase consistency of insights**, and create an evaluable pipeline where model behavior can be measured and improved.

---

## 🧩 Variants (From an AI PM Perspective)

I implemented **three variants**, each representing a different level of AI autonomy:

| Variant | Name              | Autonomy Level | Who Orchestrates?         | When I’d Use It as PM                                      |
|--------|-------------------|----------------|----------------------------|------------------------------------------------------------|
| A      | LLM Workflow      | None           | n8n (workflow-driven)      | Stable, production-ish research; predictable outputs       |
| B      | Agentic Workflow  | Low            | n8n + structured agent     | Richer reasoning with some guardrails                      |
| C      | AI Agent          | High           | Goal-driven AI agent       | Experiments, demos, PM portfolio, early discovery only     |

All three variants share the **same product goal** but expose different **design trade-offs**:

- **A (LLM workflow)** → Favors **reliability and control**  
- **B (Agentic workflow)** → Favors **flexibility with light agency**  
- **C (AI agent)** → Favors **autonomy and exploration**, at the cost of predictability  

---

## 🧠 System Architecture (Shared Concept)

From a high level, all three variants follow the same conceptual pipeline:

### 1. Trigger
- Cron (weekly) or manual trigger in n8n

### 2. Competitor Source
- Read competitors from Google Sheets (e.g., list of domains or company names)

### 3. Research
- Query recent information using a Perplexity-like search tool or web search node

### 4. Summarization / Synthesis
Use an LLM / AI agent to:
- Filter noise
- Normalize phrasing
- Extract only meaningful, time-bound updates

### 5. Report Generation
Create a markdown report enforcing a strict structure:

### 6. Delivery
- Convert markdown into an HTML snippet
- Send via Gmail / email node to predefined recipients

---

## 🅰️ Variant A – LLM Workflow (No Agency)

This variant treats the LLM as a pure function. n8n owns the orchestration; the model only summarizes and formats.

### Characteristics
- All steps (fetch competitors, call search, collate results) are explicitly defined in the workflow
- LLM is used for:
  - Summarization
  - Extraction
  - Markdown formatting

### Pros
- High reliability and predictability
- Easier to debug (each step is visible in n8n)
- Ideal starting point for productionization

### Cons
- Less flexible if business questions change frequently
- More logic lives in the workflow, not in the prompt

---

## 🅱️ Variant B – Agentic Workflow (Low Agency)

This variant pushes more responsibility into the agent prompt, but still within a fixed orchestration skeleton.

### Characteristics
The workflow still:
- Reads from sheets
- Calls search tools
- Controls loop / branching

The agent:
- Handles the per-competitor research logic
- Applies the “last 30 days” constraint
- Structures findings into markdown blocks

### Pros
- More flexible than Variant A (can adapt prompts without changing the workflow graph)
- Allows more reasoning while keeping guardrails
- Good compromise for PMs who want “smarter” agents but still need control

### Cons
- Slightly harder to test than Variant A
- Error surface shifts into prompts + agent behavior

---

## 🅲 Variant C – AI Agent (High Agency)

Here the system becomes goal-driven. The agent is given the objective and tools; it decides how to achieve the outcome.

### Characteristics

Workflow:
- Sets the goal and tools
- Delegates execution to the agent

Agent:
- Decides how to:
  - Interpret the goal
  - Sequence tool calls
  - Aggregate the results
- Produces final report body

### Pros
- Maximum autonomy and flexibility
- Great for demos, prototyping, and investor / leadership storytelling
- Useful playground for evaluating agent behavior and failure modes

### Cons
- Least predictable
- Requires strong evals and guardrails for production use
- Higher risk of hallucinations, off-goal behavior, or formatting breaks

## Workflows in n8n
<img width="354" height="535" alt="Screenshot 2025-11-16 at 7 24 49 PM" src="https://github.com/user-attachments/assets/278dac5f-cb84-4cc4-bf86-e7675cd17246" />
