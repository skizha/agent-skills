# agent-skills

A plugin marketplace for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that adds specialized workflows and domain knowledge.

## Available skills

| Skill | What it does |
|-------|-------------|
| [humanize](skills/humanize) | Detects and rewrites 24 categories of AI-generated text patterns. Based on Wikipedia's "Signs of AI writing" guide. |
| [prompt-creator](skills/prompt-creator) | Expert prompt engineering for AI models (especially Claude). Creates prompts from scratch, improves underperforming prompts, designs system prompts, structures chain-of-thought and few-shot examples, and builds RAG and agentic prompts. Based on Anthropic's official prompt engineering docs. |
| [sql-server-query-tuner](skills/sql-server-query-tuner) | SQL Server query performance tuning — analyzes execution plans, identifies anti-patterns, recommends indexes, rewrites slow queries, and runs DMV diagnostics against live instances. |
| [linkedin-articles](skills/linkedin-articles) | Write and optimize LinkedIn articles — 5 structure templates, 8 hook formulas, headline patterns, SEO keyword placement, and hashtag strategy. |
| [excalidraw-diagram](skills/excalidraw-diagram) | Generate beautiful `.excalidraw` diagram files for architectures, workflows, and concepts — with visual argument design, evidence artifacts, and a Playwright-based render pipeline. |

## Installation

### From the marketplace

```
/plugin marketplace add skizha/agent-skills
/plugin install skizha-skills@skizha-skills
```

All skills are available in your next Claude Code session under the `skizha-skills` namespace.

### Local testing

Load the repo directly as a plugin directory:

```bash
claude --plugin-dir /path/to/agent-skills
```

## Usage

Skills trigger automatically based on your prompt. You can also invoke them directly:

| Skill | Direct invocation |
|-------|------------------|
| humanize | `/skizha-skills:humanize` |
| prompt-creator | `/skizha-skills:prompt-creator` |
| sql-server-query-tuner | `/skizha-skills:sql-server-query-tuner` |
| linkedin-articles | `/skizha-skills:linkedin-articles` |
| excalidraw-diagram | `/skizha-skills:excalidraw-diagram` |

**Humanize** — activates when you say things like:
- "humanize this text"
- "make this sound less robotic"
- "remove AI writing patterns from this file"

**Prompt Creator** — activates when you say things like:
- "write a prompt for a customer service chatbot"
- "create a system prompt that outputs JSON"
- "improve this prompt — it keeps hallucinating"
- "help me prompt engineer a RAG pipeline"
- "why isn't my prompt working?"
- "make a few-shot prompt for sentiment classification"

Covers all major techniques: role/persona, few-shot, chain-of-thought, prompt chaining, structured output, RAG, agentic prompts, and meta-prompting. Includes Claude-specific best practices (XML tags, extended thinking, long context ordering) and a diagnostics guide for fixing underperforming prompts. Based on official docs from Anthropic, Microsoft Azure OpenAI, and OpenAI.

**SQL Server Query Tuner** — activates when you say things like:
- "tune this query"
- "why is this SQL slow"
- "check my database for missing indexes"
- "analyze this execution plan"
- "optimize this SQL Server query"

Supports both pasting queries/execution plans for analysis and running DMV diagnostics against a live SQL Server instance.

**LinkedIn Articles** — activates when you say things like:
- "write a LinkedIn article about [topic]"
- "optimize my LinkedIn article"
- "write a hook for my article"
- "add hashtags to this article"
- "improve my article structure"

Includes 5 article structure templates (Problem-Solution, Listicle, Story-Driven, How-To, Contrarian), 8 hook formulas with examples, headline patterns, SEO keyword placement guidance, and a hashtag strategy (broad/mid/niche mix). Delivers ready-to-paste output with hashtags.

**Excalidraw Diagram** — activates when you say things like:
- "create a diagram for my authentication flow"
- "draw an architecture diagram for this system"
- "visualize this workflow as an Excalidraw diagram"
- "make an Excalidraw diagram showing the data pipeline"

Generates `.excalidraw` JSON files following visual argument design principles — fan-outs for one-to-many, timelines for sequences, convergence for aggregation. Includes a Playwright-based render pipeline to preview diagrams as PNG and iteratively fix layout issues. Customizable color palette and a library of element templates. Requires Python + `uv` for the optional render pipeline (one-time setup).

## Marketplace structure

```
agent-skills/
├── .claude-plugin/
│   ├── plugin.json         # plugin manifest
│   └── marketplace.json    # marketplace manifest
└── skills/
    └── skill-name/
        ├── SKILL.md           # required -- instructions and metadata
        ├── references/        # optional -- detailed docs loaded on demand
        ├── scripts/           # optional -- executable code
        └── assets/            # optional -- templates, images, etc.
```

The `marketplace.json` defines a single plugin (`skizha-skills`) that bundles all skills. Each skill lives under `skills/` with at least a `SKILL.md` containing YAML frontmatter (`description` field) and markdown instructions. Skills are auto-discovered from the `skills/` directory.

## Contributing

To add a new skill:

1. Create a folder under `skills/` with a `SKILL.md` file
2. Submit a PR — the skill is auto-discovered from `skills/` and no changes to `marketplace.json` are needed

## License

MIT
