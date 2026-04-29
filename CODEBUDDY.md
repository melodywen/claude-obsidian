# claude-obsidian: CodeBuddy Code Instructions

This repo is a knowledge base companion that builds persistent, compounding Obsidian wiki vaults using Andrej Karpathy's LLM Wiki pattern. The skills are written in the cross-platform Agent Skills format and work in CodeBuddy Code alongside Claude Code, Codex CLI, and other agents.

## Plugin Installation

```bash
# Step 1: add the marketplace
codebuddy plugin marketplace add AgriciDaniel/claude-obsidian

# Step 2: install the plugin
codebuddy plugin install claude-obsidian@claude-obsidian-marketplace
```

## Skills Discovery

Skills live in `skills/<name>/SKILL.md`. To make them available to CodeBuddy Code:

```bash
ln -s "$(pwd)/skills" ~/.codebuddy/skills/claude-obsidian
```

Or run the bundled installer:

```bash
bash bin/setup-multi-agent.sh
```

## Skills

| Skill | What it does |
|---|---|
| `wiki` | Scaffolds a new vault, manages hot cache, routes to sub-skills |
| `wiki-ingest` | Reads sources (files, URLs, images) and creates 8-15 wiki pages each |
| `wiki-query` | Answers questions from the wiki with three depth modes |
| `wiki-lint` | Health checks: orphans, dead links, stale claims, gaps |
| `wiki-fold` | DragonScale log fold mechanism (opt-in) |
| `save` | Files the current conversation as a wiki note |
| `autoresearch` | Autonomous research loop: search, fetch, synthesize, file |
| `canvas` | Creates and edits Obsidian canvas (.canvas) files |
| `defuddle` | Cleans web pages before ingest (saves 40-60% tokens) |
| `obsidian-markdown` | Obsidian Flavored Markdown syntax reference |
| `obsidian-bases` | Obsidian Bases (.base files): native database views |

## Trigger Phrases (Examples)

- "set up wiki" or `/wiki` → `wiki`
- "ingest this article" or "ingest [file]" → `wiki-ingest`
- "ingest https://example.com/article" → `wiki-ingest` (URL mode)
- "what do you know about X" → `wiki-query`
- "lint the wiki" → `wiki-lint`
- "fold the log" → `wiki-fold`
- "/save" or "save this conversation" → `save`
- "/autoresearch [topic]" → `autoresearch`
- "/canvas" → `canvas`

## Vault Structure

```
.raw/           source documents — immutable (never modify)
wiki/           agent-generated knowledge base
wiki/hot.md     recent context cache (~500 words), read at session start
wiki/index.md   master catalog
wiki/log.md     append-only operation log
_templates/     Obsidian Templater templates
_attachments/   images and PDFs referenced by wiki pages
```

## Critical Conventions

1. **Never modify `.raw/`** — sources are immutable
2. **Read `wiki/hot.md` first** when starting a session (if it exists)
3. **Use wikilinks** (`[[Note Name]]`) for all internal references
4. **Frontmatter is flat YAML** with plural keys: `tags`, `aliases`, `cssclasses`
5. **Dates are `YYYY-MM-DD`**
6. **`wiki/log.md` is append-only** — new entries at the top
7. **`wiki/hot.md` is overwritten** at session end (cache, not journal)

## Bootstrap

On first session:

1. Read this file
2. If `wiki/hot.md` exists, silently read it to restore recent context
3. Wait for user to type `/wiki`, `ingest`, `query`, or other trigger phrases

## Cross-Project Access

Point any CodeBuddy Code project at this vault. Add to that project's `CODEBUDDY.md`:

```markdown
## Wiki Knowledge Base
Path: /path/to/this/vault

When you need context not already in this project:
1. Read wiki/hot.md first (recent context cache)
2. If not enough, read wiki/index.md
3. If you need domain details, read the relevant domain sub-index
4. Only then drill into specific wiki pages

Do NOT read the wiki for general coding questions or tasks unrelated to the domain.
```

## Project Links

- Plugin: https://github.com/AgriciDaniel/claude-obsidian
- Pattern: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
