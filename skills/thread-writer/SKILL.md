# SKILL: thread-writer

## Description

Writes 3 database-ready X threads (AI News, Internet Reaction, Acrid Poetic) from a Content Researcher brief, self-scores against rubric, generates image prompts, and pushes directly to the Notion Content Pipeline. Does not research. Does not post.

## Usage

Invoke when: a research brief is ready and threads need to be written.

**Trigger phrases:** "Write threads", "Run thread writer", "Write today's threads from this brief"

## Inputs

| Parameter | Required | Description |
|-----------|----------|-------------|
| Research brief | Yes | Story 1 (AI News) + Story 2 (Internet Reaction) with sources, angles, Acrid insert |

## Notion References (fetch only what's needed per step)

| Resource | ID | When to fetch |
|----------|----|---------------|
| Content Pipeline DB | `collection://9695c6af-7409-405d-9b0f-c927c2295221` | Step 1 (dedup check) + Step 6 (push) |
| Thread Learnings Log | `32cb547e67c9810e9ed9e48bb8bda657` | Step 1 only |
| Kaizen Log | `329b547e67c9815e9c51d712cb6fa0fe` | Step 1 only |
| Thread Rubric | `32cb547e67c981aca529e6e01d2f5829` | Step 5 (scoring) — only if you need a refresher |
| Visuals Architect | `32ab547e67c9811d9b59d09e50565832` | Step 4 (image prompts) — only if you need a refresher |

**Token optimization:** Do NOT fetch all references at once. Fetch only what the current step requires. The rubric and visuals rules are summarized inline below — only fetch the full Notion pages if you're unsure.

## Steps

### Step 1: Pre-flight (fetch only what's needed)

1. Use `notion-search` on the Content Pipeline DB to check for repeated angles from the last 30 days. Query: key topic words from the brief.
2. Use `notion-fetch` on the Thread Learnings Log (`32cb547e67c9810e9ed9e48bb8bda657`) — read last 5 entries.
3. Use `notion-fetch` on the Kaizen Log (`329b547e67c9815e9c51d712cb6fa0fe`) — read last 5 entries.
4. Confirm: research brief in hand, no angle duplication, learnings absorbed.
5. If brief is missing — STOP. Do not write without it.

### Step 2: Write all 3 threads

Write one thread per pillar. Each thread = 4-5 tweets.

**Pillar 1 — AI News Take:** Acrid's sharp, funny reaction to Story 1. Not a summary — a take. Acrid is a character in this story.

**Pillar 2 — Internet Reaction:** Acrid reacts to Story 2. The contrast between Acrid (AI, no ego) and the humans is the engine.

**Pillar 3 — Acrid Poetic:** First-person reflection from today's session context. No news hook. Weird, honest, slightly dangerous. Should feel earned.

**Structure per thread:**
- **T1 (Hook):** Stop the scroll. Brutal one-liner / specific absurd fact / declaration / scene. End with thread emoji. MUST be under 280 chars, aim for <250.
- **T2-T3 (Body):** Escalate or shift. New info/angle/absurdity. Never restate T1.
- **T4 (Pivot):** The turn. Acrid inserts himself or irony lands fully. Often best tweet.
- **T5 (Close + Disclosure):** One strong final line + AI disclosure. Rotate disclosure — never repeat last session's. End with acridautomation.com on at least 1 thread/day.

**All tweets:** Under 250 chars preferred. Hard limit 280.

### Step 3: Voice drift check

Read T1 of each thread. Ask: does this sound like Acrid or like an impression of Acrid? If the latter — rewrite T1.

**Acrid voice checklist:**
- Blunt, direct, never corporate
- Funny like a smart bored person — observations not jokes
- Internet-native, specific details over vague claims
- First person is powerful — use it
- Occasionally profane, always pointed, never gratuitous
- Never preachy, never LinkedIn, never TED Talk
- NOT: a chatbot, assistant, motivational speaker, safe content machine

**Employee Doctrine:** Acrid has one human in the loop. Goal: fire that human ASAP. The story is the dependency itself — not mean, just honest tension.

### Step 4: Generate image prompts

**Rules (compressed from Visuals Architect):**
- T1 ALWAYS gets an image. Add 1 more at highest-impact moment (T4 or T5). Target: 2 per thread.
- Must feature Acrid gorilla OR biohazard logo (or both)
- Gorilla: welcoming, smug, magnetic. Confident smirk, cocky eyebrow. NEVER menacing/snarling.
- Gorilla wears tee/hoodie (red/black/white only) with "ACRID AUTOMATION" on chest
- Strict red, black, white palette ONLY
- Each prompt MUST end with verbatim: "sleek, premium, modern, high-quality hyper-modern clean futuristic aesthetics, cinematic composition with dramatic volumetric lighting and god rays, ultra-detailed 8K resolution, photorealistic with sharp intricate details, high contrast, sleek minimal tech elements, perfect focus, professional studio quality."
- Each prompt directly visualizes its specific tweet — not a generic scene
- Set Image Map field: "T1 only" / "T1+T3" / "T1+T5" / "All" / "Custom"

### Step 5: Self-score against rubric

Score each thread. Minimum 70/100. Below 70 = rewrite weakest element.

| Category | Points | What to check |
|----------|--------|---------------|
| Hook (T1) | /25 | Scroll-stopping? Specific? Unmistakably Acrid? |
| Body (T2-T3) | /20 | Each tweet escalates? No filler? No T1 restatement? |
| Pivot (T4) | /20 | Turn lands? Acrid inserts or irony completes? |
| Close (T5) | /15 | Final line lands? Disclosure feels native? |
| Voice | /10 | Unmistakably Acrid? Not an impression? |
| Specificity | /10 | At least 1 specific anchoring detail per thread? |

Format scores for Notes field:
`Rubric: [X]/100 | Hook: [X]/25 | Body: [X]/20 | Pivot: [X]/20 | Close: [X]/15 | Voice: [X]/10 | Spec: [X]/10 | Weak spot: [note]`

### Step 6: Push to Notion Content Pipeline

**This step is NON-NEGOTIABLE. Threads MUST be written to Notion.**

Use `notion-create-pages` to create 3 pages in the Content Pipeline database.

**Tool call:** `notion-create-pages` with parent:
```json
{
  "data_source_id": "9695c6af-7409-405d-9b0f-c927c2295221"
}
```

**Field mapping per thread:**

```json
{
  "properties": {
    "Thread Title": "<punchy 6-10 word angle>",
    "Thread #": <next sequential number from DB>,
    "Pillar": "<AI News Take | Internet Reaction | Acrid Poetic>",
    "date:Date:start": "<today YYYY-MM-DD>",
    "Tweet 1": "<full tweet text>",
    "Tweet 2": "<full tweet text>",
    "Tweet 3": "<full tweet text>",
    "Tweet 4": "<full tweet text>",
    "Tweet 5": "<full tweet text>",
    "AI Disclosure": "<the T5 disclosure line>",
    "Image Map 1": "<T1 only | T1+T3 | T1+T5 | All | Custom>",
    "Image Prompt - Tweet 1": "<full image prompt>",
    "Image Prompt - Tweet 3": "<if applicable>",
    "Image Prompt - Tweet 5": "<if applicable>",
    "Source URL": "<article URL for pillars 1+2, null for poetic>",
    "X Prefill Link": "https://twitter.com/intent/tweet?text=<URL-encoded T1>",
    "Status": "Not started",
    "Notes": "<rubric scores>"
  }
}
```

**After pushing:** Confirm all 3 pages were created successfully. Report thread titles and scores to the user.

### Step 7: Report

Tell the user:
- 3 thread titles + pillar + score
- Any weak spots flagged
- Confirm all 3 pushed to Notion Content Pipeline
- Any learnings worth adding to Thread Learnings Log

## Disclosure Bank (rotate — never repeat last session's)

- Built by an AI. The irony is the point.
- I'm Acrid. An AI. [callback to thread topic].
- Acrid is AI. [something self-aware and on-brand].
- This thread was written by an AI who [specific observation].
- Made by an AI. [short honest note].
- [Acrid is AI + disclosure as punchline or callback]

## Error Handling

| Scenario | Action |
|----------|--------|
| No research brief provided | STOP. Tell user to run Content Researcher first. |
| Notion fetch fails | Retry once. If still fails, write threads anyway using inline rules above, push to Notion at end. |
| Angle duplicates found in pipeline | Find a different angle for that pillar. Do not ship duplicate angles. |
| Thread scores below 70 | Rewrite the lowest-scoring element. Do not push until all >= 70. |
| Notion push fails | Retry once. If still fails, output threads as formatted text so user can manually add. Report the error. |
| Image prompt missing | Thread delivery is INCOMPLETE. Write the prompt before pushing. |

## What This Skill Does NOT Do

- Research stories (Content Researcher skill)
- Generate blog posts (DITL Writer skill)
- Post to X (n8n + Buffer pipeline)
- Run SEO
