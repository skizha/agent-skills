# Prompting Techniques

## Table of Contents
1. [Clarity and Directness](#clarity-and-directness)
2. [Role / Persona](#role--persona)
3. [Few-Shot Examples](#few-shot-examples)
4. [Chain-of-Thought (CoT)](#chain-of-thought-cot)
5. [Prompt Chaining](#prompt-chaining)
6. [Structured Output](#structured-output)
7. [Output Priming / Cues](#output-priming--cues)
8. [Constraint Specification](#constraint-specification)
9. [Self-Consistency / Verification](#self-consistency--verification)
10. [Retrieval Augmentation Pattern](#retrieval-augmentation-pattern)
11. [Meta-Prompting](#meta-prompting)
12. [Iterative Refinement Pattern](#iterative-refinement-pattern)
13. [Production Best Practices](#production-best-practices)

Source: Anthropic prompt engineering docs (https://platform.claude.com/docs/en/docs/build-with-claude/prompt-engineering/overview)

---

## Clarity and Directness

**The golden rule:** Show your prompt to a colleague with minimal context and ask them to follow the instructions. If they're confused, Claude will be too.

Think of Claude as a brilliant but brand-new employee with amnesia — it has no context on your norms, styles, or preferred ways of working. The more precisely you explain what you want, the better.

**Provide contextual information:**
- What the results will be used for
- Who the target audience is
- Where this task sits in a larger workflow
- What "successful completion" looks like

**Vague vs. specific — a clear example:**

| Vague | Specific |
|---|---|
| "Remove PII from this feedback." | "Replace customer names with CUSTOMER_[ID], emails with EMAIL_[ID]@example.com, phone numbers with PHONE_[ID]. Copy verbatim if no PII found. Output only the processed messages, separated by ---." |

**Tips:**
- Provide instructions as sequential numbered steps
- Be specific about output format, length, and audience
- Include both what to do and what NOT to do

---

## Role / Persona

Assign a clear role to shape tone, expertise level, and behavior. The right role can turn Claude from a general assistant into a virtual domain expert.

```
You are a seasoned data scientist at a Fortune 500 company.
Analyze this dataset for anomalies: <dataset>{{DATASET}}</dataset>
```

**When to use:** Whenever a specific domain voice, expertise level, or communication style is needed.

**Tips:**
- Specificity matters dramatically: "data scientist specializing in customer insight analysis for Fortune 500 companies" will outperform "data scientist"
- Experiment with different roles on the same data — a `data scientist` and a `marketing strategist` will surface different insights
- Optionally add behavioral traits: "You are direct, skip preamble, and cite sources."
- For deployed assistants, put the role in the system prompt (not the user turn)

---

## Few-Shot Examples

Provide input/output examples before the actual task to demonstrate the desired format and style. This is your single most powerful shortcut for consistent, high-quality outputs.

**Structure:** Wrap examples in `<example>` tags (multiple examples in `<examples>` tags):

```
Our CS team is overwhelmed with unstructured feedback. Categorize issues using these categories:
UI/UX, Performance, Feature Request, Integration, Pricing, Other.
Rate sentiment (Positive/Neutral/Negative) and priority (High/Medium/Low).

<examples>
<example>
Input: The new dashboard is a mess! It takes forever to load, and I can't find the export button. Fix this ASAP!
Category: UI/UX, Performance
Sentiment: Negative
Priority: High
</example>
<example>
Input: Love the Salesforce integration! But it'd be great if you could add Hubspot too.
Category: Integration, Feature Request
Sentiment: Positive
Priority: Medium
</example>
</examples>

Now analyze this feedback: {{FEEDBACK}}
```

**When to use:**
- Output format is non-obvious or nuanced
- Consistency across many inputs is critical
- Model keeps drifting from desired style

**Tips:**
- 3-5 diverse, relevant examples is the sweet spot
- Cover edge cases and potential challenges in your examples
- Vary examples enough that Claude doesn't pick up on unintended patterns
- Ask Claude to evaluate your examples for relevance, diversity, or clarity
- Ask Claude to generate more examples based on your initial set

---

## Chain-of-Thought (CoT)

Instruct the model to reason step-by-step before answering. Use for tasks that a human would need to think through: complex math, multi-step analysis, writing complex documents, or decisions with many factors.

**Critical:** Always have Claude output its thinking. Without outputting its thought process, no thinking occurs!

**Three levels, ordered least to most powerful:**

**Level 1 — Basic (zero-shot):**
```
Think step by step before giving your final answer.

Problem: A train travels at 60 mph for 2.5 hours, then 80 mph for 1.5 hours. Total distance?
```

**Level 2 — Guided (specify what to think about):**
```
Draft a personalized donor email.

Think before writing. First, consider what messaging appeals to this donor based on their history.
Then consider which aspects of the Care for Kids program match their interests.
Finally, write the email using your analysis.

<program>{{PROGRAM_DETAILS}}</program>
<donor>{{DONOR_DETAILS}}</donor>
```

**Level 3 — Structured (XML-separated reasoning from answer):**
```
You're a financial advisor. A client wants to invest $10,000 in either:
A) A stock returning ~12% annually but volatile
B) A bond guaranteeing 6% annually
They need the money in 5 years for a house down payment.

Think through this in <thinking> tags, then give your final recommendation in <answer> tags.
```

The structured pattern makes it easy to strip out reasoning in post-processing and gives Claude clearer guidance on what goes where.

**When to use:** Math, logic, multi-step analysis, classification with nuance, debugging, high-stakes decisions.

---

## Prompt Chaining

Break complex tasks into smaller, sequential prompts where each output feeds into the next. Especially valuable when a task has multiple distinct phases that each require careful thought.

**Why chain instead of one big prompt?**
- Each subtask gets Claude's full attention
- Easier to isolate and fix a failing step
- Intermediate outputs can be reviewed/edited before proceeding
- Independent subtasks can run in parallel

**Common chain patterns:**

| Pattern | Chain |
|---|---|
| Content creation | Research → Outline → Draft → Edit → Format |
| Data processing | Extract → Transform → Analyze → Summarize |
| Decision-making | Gather info → List options → Analyze each → Recommend |
| Quality assurance | Generate → Review/Critique → Refine → Final review |

**Self-correction chain (generate → critique → improve):**

Prompt 1: `Summarize this research paper in <summary> tags. Focus on methodology, findings, and clinical implications.`

Prompt 2: `Review this summary for accuracy, clarity, and completeness. Grade it A-F on each dimension with one sentence of justification.\n<summary>{{SUMMARY}}</summary>\n<paper>{{PAPER}}</paper>`

Prompt 3: `Improve the summary based on the feedback. Output only the revised version.\n<summary>{{SUMMARY}}</summary>\n<feedback>{{FEEDBACK}}</feedback>`

**Tips:**
- Use XML tags for clean handoffs between prompts: output in `<result>` tags, pass them as input to the next
- Each prompt should have one clear objective
- For independent subtasks (e.g., analyzing multiple documents), run chains in parallel
- Debug by isolating the failing step into its own prompt

---

## Structured Output

Request specific output formats to make responses machine-parseable or consistently formatted.

**JSON:**
```
Extract the following fields from the email below and return them as JSON.
Fields: sender_name, sender_email, subject, urgency (low/medium/high), action_required (boolean)

Return only the JSON object, no explanation.

Email: [EMAIL]
```

**Markdown table:**
```
Compare the three cloud providers below on: pricing, uptime SLA, and support tiers.
Format as a markdown table with providers as rows and criteria as columns.
```

**Custom schema:**
```
List the top 5 risks. For each risk use this format:
**Risk N: [Title]**
- Likelihood: Low / Medium / High
- Impact: Low / Medium / High
- Mitigation: [one sentence]
```

**Token efficiency tip:** For passing dense structured data, markdown tables are more token-efficient than JSON or repeated field labels. Avoid excessive whitespace — consecutive spaces are each their own token.

**Tip:** Ask Claude to wrap outputs in XML tags (e.g., `<json>`, `<analysis>`) to make post-processing easier when responses contain both reasoning and structured data.

**When to use:** API integration, downstream processing, reports, consistent document generation.

---

## Output Priming / Cues

End the prompt with a partial phrase to "jumpstart" the model toward a specific format. The model continues from where you left off.

```
Summarize the following meeting notes.

Key takeaways:
-
```

```
Based on the data below, the most likely root cause is:
```

```
Here's a bulleted list of action items:
•
```

**Why it works:** The model completes what looks like the most natural continuation of the text. Priming the output is often more reliable than describing the format in instructions alone.

**Tips:**
- Combine with explicit format instructions for maximum reliability
- Use numbered or bulleted prefixes to force list output
- Use `##` or section headings to prime structured document output
- Priming with `"The answer is:"` after a reasoning block extracts a clean final answer

**When to use:** Whenever you need precise output format control without overloading the instructions.

---

## Constraint Specification

Explicitly state what the model should and should not do. Pair positive constraints ("do X") with negative constraints ("do NOT do Y").

```
Summarize the article below.
- Length: 3-5 sentences
- Audience: non-technical executive
- Do NOT include technical jargon
- Do NOT speculate beyond what the article states
- Use active voice
```

**Give the model an "out":** Always provide a fallback for when the model can't answer. This is one of the most effective ways to prevent hallucination.

```
Answer the question based on the document below.
If the answer is not in the document, respond with "Not found."

[DOCUMENT]

Question: [QUESTION]
```

Without an explicit fallback, the model will attempt to answer anyway — often incorrectly.

**Recency bias:** Information at the end of a prompt tends to have more influence on the output. If a key constraint is being ignored, try repeating it at the very end of the prompt.

**When to use:** Any time defaults produce the wrong style, length, scope, or tone.

---

## Self-Consistency / Verification

Ask the model to check its own work or verify against constraints before finalizing.

**Self-check:**
```
Answer the question, then verify your answer against the constraints below.
If your answer violates any constraint, revise it before outputting.

Question: [QUESTION]
Constraints: [CONSTRAINTS]
```

**Critique-and-revise:**
```
Draft a response to the complaint below. Then critique your draft for tone and completeness.
Finally, write a revised version incorporating your critique.

Complaint: [COMPLAINT]
```

**Test-and-fix (code):**
```
Write a function to calculate factorial.
Before finalizing, verify your solution with test cases for n=0, n=1, n=5, n=10.
Fix any issues you find.
```

**When to use:** High-stakes outputs, compliance-sensitive content, code, formal documents.

---

## Retrieval Augmentation Pattern

Structure prompts that include external context (documents, database results, search snippets).

```
Answer the question using ONLY the context provided below.
If the answer cannot be found in the context, say "I don't know."

<context>
[RETRIEVED CONTENT]
</context>

Question: [QUESTION]
```

**Grounding variants:**
- "Quote the exact sentence from the context that supports your answer."
- "Find quotes relevant to the question first, place them in `<quotes>` tags, then answer based only on those quotes."
- "If multiple sources conflict, note the conflict and explain which you trust more and why."

**Citations as anti-hallucination:** Requiring citations is a powerful hallucination deterrent. To fabricate a response, the model must now make *two* errors — a false claim AND a bad citation. Inline citations (placed next to each claim) are more effective than citations at the end, since the model anticipates the citation sooner and stays grounded.

```
Answer the question below. After each claim, cite the source document and section in [brackets].
If a claim cannot be supported by the provided documents, do not include it.

[DOCUMENTS]

Question: [QUESTION]
```

**When to use:** RAG pipelines, document QA, knowledge-grounded chatbots, research summaries.

---

## Meta-Prompting

Use the model itself to generate or improve your prompts. Describe the task you need a prompt for, and let the model draft one — then iterate.

**Generate a prompt from a task description:**
```
You are an expert prompt engineer. Write a high-quality prompt for the following task.

Task: [DESCRIPTION OF WHAT THE PROMPT NEEDS TO DO]

Requirements for the prompt:
- Target model: Claude / GPT-4 / [MODEL]
- Output format: [JSON / prose / list / etc.]
- Audience for the output: [WHO READS THE RESULT]
- Key constraints: [ANY KNOWN REQUIREMENTS]

Return only the prompt, ready to use.
```

**Improve an existing prompt:**
```
You are an expert prompt engineer. Improve the prompt below.

Current prompt:
---
[CURRENT PROMPT]
---

Problems observed:
- [PROBLEM 1]
- [PROBLEM 2]

Produce an improved version that fixes these issues. Explain the key changes you made.
```

**Evaluate a prompt before using it:**
```
Review the prompt below and identify any weaknesses: ambiguity, missing constraints, likely failure modes, or format issues.

Prompt:
---
[PROMPT]
---

Rate it on clarity, specificity, and completeness (1-5 each). Then suggest improvements.
```

**When to use:** When starting from scratch, when a prompt is underperforming and you want a second opinion, or when iterating quickly across many prompt versions.

---

## Iterative Refinement Pattern

Build prompts that produce an artifact, then improve it in subsequent turns.

**Single-turn:**
```
Write a first draft of [ARTIFACT], then immediately improve it by [CRITERIA].
Output only the improved version.
```

**Multi-turn scaffold:**
```
Turn 1: Draft
Turn 2: "Improve clarity and conciseness. Keep it under 200 words."
Turn 3: "Adjust tone to be more formal."
```

**When to use:** Creative writing, marketing copy, technical documentation that needs polish.

---

## Production Best Practices

Techniques for deploying prompts reliably at scale.

**Pin to a specific model version:** In production, always specify an exact model snapshot (e.g., `claude-sonnet-4-6`, `gpt-4.1-2025-04-14`) rather than a floating alias. Model updates change behavior silently — pinning prevents surprise regressions.

**Build evals before shipping:** Before deploying a prompt, create a small test set of inputs and expected outputs. Measure pass rate. This lets you safely iterate on prompts and catch regressions when upgrading models.

**Temperature settings:**
| Use case | Temperature |
|---|---|
| Factual Q&A, data extraction, classification | 0 or very low (0.0–0.2) |
| Summarization, analysis | 0.3–0.5 |
| Creative writing, brainstorming | 0.7–1.0 |

Adjust temperature OR top_p — not both at once, as they interact in hard-to-predict ways.

**Conclusions last:** Structure prompts so that reasoning or evidence comes first and the final classification, decision, or answer appears last. This improves accuracy as the model "earns" its conclusion through the reasoning.

```
Analyze the customer review below and determine the sentiment.

Review: [REVIEW]

First list the positive signals, then list the negative signals.
Sentiment:
```
