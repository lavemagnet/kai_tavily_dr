# Deep Research (Tavily)

Research skill for **[Kai 9000](https://kai9000.com)** — the open-source AI assistant. Multi-step deep research via [Tavily](https://tavily.com). Performs structured investigation — recon → deep-dive → synthesis — and writes a cited `.md` report.

## What it does

Given a research question, the skill:

1. **Decomposes** the topic into 3–5 sub-questions, generates search queries in both Russian and English.
2. **Recons** with `tavily_search` — maps the topic landscape, collects candidate sources.
3. **Deep-dives** into key sources with `tavily_extract` (full text for facts and citations). Optionally crawls important sites with `tavily_crawl`.
4. **Synthesizes** findings, notes contradictions between sources, estimates confidence honestly.
5. **Writes** a structured Markdown report to disk.

## Depth levels

| Level | Trigger | Actions | Min sources |
|-------|---------|---------|-------------|
| **L0** Fact-check | Single fact | 1 search → answer | 3 |
| **L1** Quick overview | 1 domain | 1–2 searches + 1 extract | 3 |
| **L2** Standard *(default)* | 2–3 domains, analysis needed | Multi-query search (ru+en) + extracts + synthesis | 5 |
| **L3** Deep | 4+ domains, important decision | Full collection + `tavily_research` + contradiction analysis | 10 |
| **L4** Ultra-deep | Maximum depth | L3 + multi-perspective critique (skeptic/opponent/practitioner) | 10, ≥3 independent per key claim |

## Report format

```markdown
# <Title>

**Date:** YYYY-MM-DD
**Engine:** tavily ({search+extract | research})
**Confidence:** high | medium | low
**Depth:** L0–L4

---

## Key Findings
## Context
## Analysis
### Sub-topic 1
### Sub-topic N
## Sources
## Methodology Notes
## Recommendations (optional)
```

Reports are saved to `/root/deep_research/YYYY-MM-DD HHMM - <slug>.md`.

## Requirements

- **[Kai 9000](https://kai9000.com)** (any platform)
- **Tavily MCP server** connected to Kai, exposing: `tavily_search`, `tavily_extract`, `tavily_crawl`/`tavily_map`, `tavily_research` (optional, for L3/L4)
- Optional: Context7 MCP server for code/library/API questions

## Installation

Add this skill to Kai 9000's skills directory. The exact path depends on your platform; refer to [Kai's skill documentation](https://kai9000.com/docs/).

```bash
git clone https://github.com/lavemagnet/kai_tavily_dr.git
```

Then import `SKILL.md` into Kai via Settings → Skills.

## Usage

Trigger with prompts like:

- «Исследуй X»
- «Глубоко разбери Y»
- «Сравни X и Y»
- «Что известно про Z? State of the art»
- «Сделай обзор W»

**Not** for simple single-fact questions — use a quick web search for those.

## Limitations

- Queries go to Tavily cloud (not private) — don't include sensitive data.
- Tavily consumes API credits per call; the skill doses requests by depth level.
- Single-turn execution; no follow-up questions — acts on reasonable assumptions and documents them.

## License

MIT
