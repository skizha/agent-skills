# claude-skills

A collection of skills (plugins) for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that add specialized workflows and domain knowledge.

## Available skills

| Skill | What it does |
|-------|-------------|
| [humanize](skills/humanize) | Detects and rewrites 24 categories of AI-generated text patterns. Based on Wikipedia's "Signs of AI writing" guide. |

## Installation

Copy any skill folder into your Claude Code skills directory:

```bash
# clone the repo
git clone https://github.com/skizha/claude-skills.git

# copy the skill you want
cp -r claude-skills/skills/humanize ~/.claude/skills/
```

Or grab just one skill without cloning the whole repo:

```bash
# using gh
gh repo clone skizha/claude-skills -- --depth 1 --filter=blob:none
cp -r claude-skills/skills/humanize ~/.claude/skills/
```

The skill will be available in your next Claude Code session.

## Usage

Skills trigger automatically based on your prompt. For example, the humanize skill activates when you say things like:

- "humanize this text"
- "make this sound less robotic"
- "remove AI writing patterns from this file"

You can also invoke it directly with `/humanize`.

## Skill structure

Each skill is a self-contained folder:

```
skills/
└── skill-name/
    ├── SKILL.md           # required -- instructions and metadata
    ├── references/        # optional -- detailed docs loaded on demand
    ├── scripts/           # optional -- executable code
    └── assets/            # optional -- templates, images, etc.
```

## Contributing

To add a new skill, create a folder under `skills/` with at least a `SKILL.md` file containing YAML frontmatter (`name` and `description` fields) and markdown instructions.

## License

MIT
