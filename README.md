# opencode-agy-skill

An [Agent Skill](https://skills.sh/) that lets your AI coding assistant delegate
complex, multi-step, or resource-intensive tasks to Google's **Antigravity CLI
(`agy`)** — giving you access to Claude Opus 4.6, Sonnet 4.6, Gemini 3.1 Pro,
and other models through the official `agy` binary.

Works with **OpenCode**, **Claude Code**, **Codex CLI**, **Cursor**, **Gemini CLI**,
and any agent that supports the `SKILL.md` format.

## Installation

### Via npx skills (recommended)

```bash
npx skills add louisfghbvc/opencode-agy-skill
```

This clones the skill into your agent's skills directory. Restart your agent
and it will auto-discover the skill.

### Manual (any agent)

```bash
git clone https://github.com/louisfghbvc/opencode-agy-skill.git
cp -r opencode-agy-skill ~/.agents/skills/agy-delegate/
# Or for other agents:
# cp -r opencode-agy-skill ~/.claude/skills/agy-delegate/
# cp -r opencode-agy-skill ~/.codex/skills/agy-delegate/
# cp -r opencode-agy-skill ~/.cursor/skills/agy-delegate/
```

### Prerequisites

- **`agy` CLI** installed and authenticated — run `agy` standalone at least
  once to complete OAuth.
- `agy` must be on your `PATH`, or set `AGY_BIN` environment variable.

## What It Does

When your primary agent encounters a task that would benefit from deeper
reasoning or a different model's perspective, this skill instructs it to
shell out to `agy --print`:

```
Primary agent (e.g. DeepSeek)
  └─ detects complex task
      └─ loads agy-delegate skill
          └─ runs: agy -p "<detailed prompt>" --add-dir .
              └─ agy (Claude Opus / Gemini) processes it
          └─ captures stdout → returns to primary agent
```

### When to Delegate

| Scenario | Delegate? |
|----------|:---------:|
| Deep code review with multi-file context | ✅ |
| Large refactor (10+ files) | ✅ |
| Web research with Google Search grounding | ✅ |
| Image generation | ✅ |
| Second opinion from Claude Opus 4.6 | ✅ |
| Long-running analysis (>30s thinking) | ✅ |
| Simple one-shot question | ❌ |
| Quick file edit | ❌ |
| Anything needing OpenCode tool calls | ❌ |

## How It Works

The skill is a single `SKILL.md` file with YAML frontmatter. Your agent reads
it on-demand when a task matches its description, then follows the instructions
to invoke `agy` via shell subprocess.

No plugins, no MCP servers, no external dependencies — just the `agy` CLI
that you already have installed.

## License

MIT
