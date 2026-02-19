# agent-skills

A plugin marketplace for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that adds specialized workflows and domain knowledge.

## Available plugins

| Plugin | Skills | What it does |
|--------|--------|-------------|
| humanize-skills | [humanize](skills/humanize) | Detects and rewrites 24 categories of AI-generated text patterns. Based on Wikipedia's "Signs of AI writing" guide. |
| prompt-creator | [prompt-creator](skills/prompt-creator) | Expert prompt engineering for AI models (especially Claude). Creates prompts from scratch, improves underperforming prompts, designs system prompts, structures chain-of-thought and few-shot examples, and builds RAG and agentic prompts. Based on Anthropic's official prompt engineering docs. |
| sql-server-query-tuner | [sql-server-query-tuner](skills/sql-server-query-tuner) | SQL Server query performance tuning — analyzes execution plans, identifies anti-patterns, recommends indexes, rewrites slow queries, and runs DMV diagnostics against live instances. |

## Installation

### From the marketplace

```
/plugin marketplace add skizha/agent-skills
/plugin install humanize-skills@skizha-skills
```

The skills will be available in your next Claude Code session. Invoke them with `/humanize` or by asking Claude to humanize text, or trigger the SQL tuner by asking Claude to tune a query.

### Local testing

Load the repo directly as a plugin directory:

```bash
claude --plugin-dir /path/to/agent-skills
```

## Usage

Skills trigger automatically based on your prompt. For example:

**Humanize** — activates when you say things like:
- "humanize this text"
- "make this sound less robotic"
- "remove AI writing patterns from this file"

You can also invoke it directly with `/humanize`.

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

## Marketplace structure

```
agent-skills/
├── .claude-plugin/
│   └── marketplace.json    # marketplace manifest
└── skills/
    └── skill-name/
        ├── SKILL.md           # required -- instructions and metadata
        ├── references/        # optional -- detailed docs loaded on demand
        ├── scripts/           # optional -- executable code
        └── assets/            # optional -- templates, images, etc.
```

The `marketplace.json` defines plugins, each of which bundles one or more skills. Each skill lives under `skills/` with at least a `SKILL.md` containing YAML frontmatter (`description` field) and markdown instructions.

## Contributing

To add a new skill:

1. Create a folder under `skills/` with a `SKILL.md` file
2. Add your plugin entry to `.claude-plugin/marketplace.json` with a `name`, `description`, and `skills` array pointing to your skill folders
3. Submit a PR

## License

MIT
