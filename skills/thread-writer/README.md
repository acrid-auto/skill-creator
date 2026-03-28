# Thread Writer

Writes 3 database-ready X threads from a Content Researcher brief, self-scores, generates image prompts, and pushes directly to Notion Content Pipeline.

## Quick Start

```
Write today's threads from this brief: [paste research brief]
```

## Prerequisites

- Notion MCP server connected (provides `notion-fetch`, `notion-search`, `notion-create-pages`)
- Content Researcher brief ready (Story 1 + Story 2 with sources and angles)

## How It Works

1. Pre-flight: checks for duplicate angles, reads learnings + kaizen
2. Writes 3 threads (AI News Take, Internet Reaction, Acrid Poetic)
3. Voice drift check on all T1s
4. Generates compliant image prompts (2 per thread)
5. Self-scores against rubric (min 70/100)
6. Pushes all 3 threads to Notion Content Pipeline DB via `notion-create-pages`
7. Reports results

## Token Optimization

This skill fetches Notion resources lazily — only what the current step needs. The rubric, voice guide, and visuals rules are compressed inline. Full Notion pages are only fetched when a refresher is needed.

## Output

3 threads pushed to Notion Content Pipeline (`collection://9695c6af-7409-405d-9b0f-c927c2295221`) with all fields populated: tweets, image prompts, scores, disclosure, prefill links.

## Limitations

- Does not research stories (use Content Researcher)
- Does not post to X (handled by n8n + Buffer)
- Does not generate blog posts (use DITL Writer)
- Requires Notion MCP connection
