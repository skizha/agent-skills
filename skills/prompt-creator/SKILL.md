---
name: prompt-creator
description: >
  Expert prompt engineering for AI models (especially Claude). Use when the user wants to:
  create a new prompt from scratch, improve or debug an existing prompt, design a system
  prompt for a chatbot or assistant, write few-shot examples, structure chain-of-thought
  reasoning, generate structured/JSON output, build RAG or agentic prompts, or evaluate
  why a prompt is underperforming and how to fix it. Triggers on requests like
  "write a prompt for...", "create a system prompt", "improve this prompt",
  "help me prompt engineer", "why isn't my prompt working", "make a prompt that outputs JSON".
---

# Prompt Creator

## Workflow

Follow this process for every prompt engineering request:

**1. Clarify the goal (if not already clear)**
- What is the task the prompt must accomplish?
- Who/what will execute it (Claude, GPT, a specific model)?
- How will the output be used (human reads it / parsed by code / passed to another model)?
- What does "good" look like? Any examples of ideal output?

**2. Select the right techniques**
Determine which techniques apply based on the request type:

| Request type | Primary techniques |
|---|---|
| Chatbot / assistant | Role/persona, system prompt split, constraint specification |
| Data extraction | Structured output (JSON), XML tags, constraint specification, give model an "out" |
| Classification | Few-shot examples (`<examples>` tags), structured output, conclusions-last ordering |
| Reasoning / analysis | Chain-of-thought (CoT), extended thinking, output priming |
| Multi-phase complex tasks | Prompt chaining, self-correction chain |
| Document generation | Decomposition, template pattern, output priming |
| Code gen / review | Constraint specification, structured output, CoT self-verification |
| RAG / document QA | Docs-above-query ordering, citations as anti-hallucination, give model an "out" |
| Agent / tool use | Agentic pattern, constraint specification |
| Writing a prompt from scratch | Meta-prompting — describe the task, let the model draft |
| Improving a bad prompt | Diagnose first (see Diagnostics below), meta-prompting for alternatives |

Read [references/techniques.md](references/techniques.md) for full technique details and examples (including Prompt Chaining).
Read [references/claude-specifics.md](references/claude-specifics.md) when targeting Claude specifically (XML tags, **critical long-context ordering**, extended thinking, system vs human split, prompt injection defense).
Read [references/prompt-types.md](references/prompt-types.md) for ready-to-adapt templates for common prompt categories (including RAG, Prompt Chaining, Agentic).

**3. Draft the prompt**
- Use the relevant template from prompt-types.md as a starting point
- Apply techniques from techniques.md
- Apply Claude-specific best practices from claude-specifics.md when relevant
- Follow the quality checklist below before delivering

**4. Deliver and offer iteration**
Present the finished prompt in a code block. Briefly explain key design choices (1-3 sentences). Offer to:
- Add few-shot examples
- Adjust tone, length, or format constraints
- Create test cases to validate the prompt

---

## Quality Checklist

Before delivering any prompt, verify:

- [ ] Task is unambiguous — a stranger could execute it correctly
- [ ] Output format is explicitly specified (or output primed with a partial phrase)
- [ ] Both "do" and "do NOT" constraints are present where relevant
- [ ] Model has an "out" for cases it can't answer (e.g., "respond with 'not found' if...")
- [ ] Variables/placeholders use a consistent convention: `[PLACEHOLDER]` or `{{placeholder}}`
- [ ] Long prompts use XML tags or clear section headers to separate concerns
- [ ] No conflicting instructions; key constraints repeated at the end if critical
- [ ] Role/persona is assigned if tone or expertise level matters
- [ ] Few-shot examples included if output style is non-obvious
- [ ] Reasoning requested (CoT) if the task involves multi-step logic
- [ ] Citations requested if factual accuracy and grounding are critical

---

## Diagnostics: Improving a Bad Prompt

When a user brings an underperforming prompt, diagnose before prescribing:

**Hallucination / making things up**
- Add a fallback: "If the answer isn't in the context, respond with 'Not found.'"
- Add citation requirement: "After each claim, cite the source. Do not make claims you can't cite." (forcing citations forces double errors to fabricate)
- For RAG: add explicit grounding instructions and prompt injection defense (see claude-specifics.md)

**Wrong format / structure**
- Be more explicit about output format; provide a template or example output

**Inconsistent results**
- Add few-shot examples; tighten constraints; reduce degrees of freedom

**Too long / too short**
- Add explicit length constraint: "Respond in exactly 3 bullet points" / "Keep response under 100 words"

**Wrong tone**
- Assign a specific persona; add explicit tone descriptors; provide a style example

**Ignoring part of the instructions**
- Use XML tags to separate sections; repeat critical instructions at the end (recency bias — the model gives more weight to content near the end)
- Simplify: break into multiple smaller prompts if complexity is the issue

**Reasoning errors**
- Add chain-of-thought: "Think step by step before answering"
- Use extended thinking mode if available

---

## Output Format

Present the final prompt in a fenced code block:

```
[FINAL PROMPT HERE]
```

Then add a short **Design notes** section (2-4 bullets) explaining the key choices made.
