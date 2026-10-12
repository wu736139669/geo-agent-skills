# GEO Agent Skills

Two [Agent Skills](https://agentskills.io) for **Generative Engine Optimization** — making a static site readable and citable by AI agents and answer engines (ChatGPT, Claude, Perplexity, Grok, and coding agents).

| Skill | What it does |
| --- | --- |
| [`markdown-for-agents`](skills/markdown-for-agents/SKILL.md) | Turns a static site into a source AI agents can read cheaply: Markdown versions of every page, `llms.txt` / `llms-full.txt` indexes, `rel="alternate" type="text/markdown"` link relations, JSON-LD, and content negotiation on Cloudflare Workers (`Accept: text/markdown` → Markdown, browsers keep HTML). |
| [`agent-ready-check`](skills/agent-ready-check/SKILL.md) | Audits any site (deployed URL or local build) for AI-agent readiness — robots.txt AI-crawler rules, llms.txt, Markdown negotiation, JSON-LD, sitemap, alternate links — and produces a weighted **9-point score** with per-item fixes. |

They pair: `agent-ready-check` finds the gaps, `markdown-for-agents` fixes them.

## Install

**GitHub CLI (recommended):**

```bash
gh skill install wu736139669/geo-agent-skills
```

**Claude Code / Claude apps:**

```bash
git clone --depth 1 https://github.com/wu736139669/geo-agent-skills /tmp/geo-agent-skills \
  && cp -r /tmp/geo-agent-skills/skills/* ~/.claude/skills/
```

**Codex / agents reading `~/.agents/skills`:**

```bash
git clone --depth 1 https://github.com/wu736139669/geo-agent-skills /tmp/geo-agent-skills \
  && cp -r /tmp/geo-agent-skills/skills/* ~/.agents/skills/
```

## Quick start

Audit first:

```bash
python3 skills/agent-ready-check/scripts/check_site.py https://example.com
```

Then fix what's missing — generate Markdown, publish llms.txt, wire the worker:

```bash
python3 skills/markdown-for-agents/scripts/generate_md.py ./public --base https://example.com
```

## Companion projects

- [markdown-for-agents-worker](https://github.com/wu736139669/markdown-for-agents-worker) — the Cloudflare Worker that serves Markdown via content negotiation (free-plan friendly; alternative to Pro-plan "Markdown for Agents").
- [markdown-for-agents-action](https://github.com/wu736139669/markdown-for-agents-action) — GitHub Action that generates Markdown versions + llms.txt on every deploy.
- **Live reference implementation:** [toolpantry.app](https://toolpantry.app) — 184 browser tools, every page served as `.md`, plus `/tools.json`, an [agent skill](https://github.com/wu736139669/toolpantry-skill), and a public API. Run the checker against it: `python3 skills/agent-ready-check/scripts/check_site.py https://toolpantry.app`

## Why this matters

Research on AI citations shows the strongest predictors are **referring-domain count and machine-readable content** — not classic rankings. Agents fetch with `Accept: text/markdown` and read `llms.txt` indexes; a site that only serves HTML pays a parsing tax for every fetch. These skills make the site cheap to read (so agents read more of it) and easy to verify (so you can measure progress).

## License

MIT
