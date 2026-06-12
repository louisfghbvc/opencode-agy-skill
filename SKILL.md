---
name: agy-delegate
description: >-
  Delegate complex, multi-step, or resource-intensive tasks to Google's
  Antigravity CLI (agy). Use when a task requires deep reasoning, code review
  from a second AI perspective, large-scale refactoring, web research with
  Google Search grounding, image generation, or anything that would benefit
  from a different model's capabilities (Claude Opus 4.6, Sonnet 4.6, Gemini
  3.1 Pro). Runs `agy` via shell subprocess and captures the response as plain
  text. Best for tasks where the primary model's context window or reasoning
  depth is a bottleneck.
license: MIT
metadata:
  platforms: [opencode, claude-code, codex, cursor]
  tools: [agy]
---

# agy Delegate Skill

Delegate work to Google Antigravity CLI (`agy`) when the primary agent's
context window, tool budget, or reasoning depth hits a ceiling.

## Prerequisites

- `agy` CLI installed and authenticated (`agy` standalone at least once)
- `agy` on `PATH`, or set `AGY_BIN` environment variable

## When to Delegate

| Scenario | Delegate? |
|----------|:---------:|
| Deep code review with multi-file context | ✅ |
| Large refactor (10+ files) | ✅ |
| Web research / Google Search grounding | ✅ |
| Image generation | ✅ |
| Need a second opinion from Claude Opus 4.6 | ✅ |
| Long-running analysis (>30s of thinking) | ✅ |
| Simple one-shot question | ❌ do it yourself |
| Quick file edit | ❌ do it yourself |
| Anything requiring OpenCode tool calls | ❌ keep in harness |

## Usage

```bash
# Basic one-shot
agy -p "<task description>" --add-dir <working-dir>

# Background (capture output to file for later retrieval)
agy -p "<long running task>" --add-dir . > /tmp/agy-output.txt &

# With file access
agy -p "Analyze src/trading/strategy.py for performance issues" --add-dir /path/to/project
```

## Prompting Guidelines

When delegating to agy, pack full context into the prompt. agy has no access
to OpenCode's harness (no tool calls), so **include everything the task
needs** in the prompt text itself:

```
You are a code analysis expert. Review the following code and identify
potential issues:

[full code or file paths here]

Focus on:
1. Performance bottlenecks
2. Security concerns
3. Code quality improvements
4. Design pattern suggestions
```

### File Access

`--add-dir <dir>` lets agy read files within that directory tree.
If agy needs to read specific files, tell it to do so in the prompt:

```bash
agy -p "Read src/strategy.py and src/backtest.py, then suggest improvements to the rebalancing logic" --add-dir .
```

## Known Limitations

| Limitation | Detail |
|------------|--------|
| Plain text output | `agy --print` does not expose tool calls; only stdout is returned |
| No shared context | agy has its own session; no access to OpenCode conversation history |
| One-shot per call | Each call spawns a fresh agy process |
| No streaming | Full response is buffered and returned at once |
| No media input | agy CLI does not accept image/file attachments |
| Model selection | agy may not support `--model` depending on version; server picks the default |

## Binary Discovery

```bash
# Default: agy on PATH
# Override via env:
export AGY_BIN=/path/to/agy
```
