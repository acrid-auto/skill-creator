# SKILL: video-creator

## Description

Generates short-form AI videos from a text prompt — complete with AI-generated script, images, voiceover, and synchronized captions. Built on the Remotion prompt-to-video pipeline. Takes a topic and title, runs it through GPT-4 for story generation, DALL-E 3 for visuals, ElevenLabs for narration, then renders a production-ready vertical MP4.

## Usage

Use when you need to create a short-form video (TikTok, Reels, Shorts) from a text description. Handles the entire pipeline from idea to rendered video.

**Triggers:** "Make me a video about...", "Create a short video on...", "Generate a TikTok about..."

## Inputs

| Parameter | Required | Type | Default | Description |
|-----------|----------|------|---------|-------------|
| `title` | Yes | string | — | Video title displayed in the intro slide |
| `topic` | Yes | string | — | Detailed description of what the video should cover |
| `voice_id` | No | string | `21m00Tcm4TlvDq8ikWAM` | ElevenLabs voice ID. Browse voices at elevenlabs.io |
| `output_path` | No | string | `./out/video.mp4` | Where to save the rendered video |

## Prerequisites

- Node.js 18+ and npm installed
- The `template-prompt-to-video` template cloned (included in `remotion-templates/`)
- Environment variables set:
  ```
  OPENAI_API_KEY=sk-...        # For GPT-4 story + DALL-E 3 images
  ELEVENLABS_API_KEY=...       # For text-to-speech narration
  ```

## Outputs

Rendered MP4 video file (1080x1920, 30fps, vertical format) with:
- 1-second branded intro slide
- AI-generated background images with zoom/blur transitions
- Synchronized voiceover narration
- Word-by-word animated captions

## Steps

1. **Validate prerequisites** — Check that `OPENAI_API_KEY` and `ELEVENLABS_API_KEY` environment variables are set. If not, return: "Missing API keys. Set OPENAI_API_KEY and ELEVENLABS_API_KEY in your environment."

2. **Navigate to template** — Change to the `remotion-templates/template-prompt-to-video/` directory. If it doesn't exist, return: "Template not found. Run `git clone https://github.com/remotion-dev/template-prompt-to-video.git` in remotion-templates/."

3. **Install dependencies** — Run `npm install` if `node_modules/` doesn't exist.

4. **Generate the story** — Run the generation CLI:
   ```bash
   npx tsx cli/cli.ts --title "<title>" --topic "<topic>"
   ```
   This triggers the full pipeline:
   - GPT-4.1 generates an 8-10 sentence story from the topic
   - GPT-4.1 creates 5-8 image descriptions mapped to sentences
   - DALL-E 3 generates 1024x1792 images for each description (retries up to 3x on failure)
   - ElevenLabs generates voiceover audio with character-level timestamps
   - Timeline is assembled with alternating zoom animations and blur transitions

5. **Verify generation** — Check that `public/content/<slug>/timeline.json` exists, where `<slug>` is the lowercase hyphenated title. If missing, report the error from the generation step.

6. **Render the video** — Run:
   ```bash
   npx remotion render --composition <slug>
   ```
   The composition ID matches the slug derived from the title.

7. **Deliver** — Report the output file location and a summary:
   ```
   Video rendered: <output_path>
   Duration: <X> seconds
   Resolution: 1080x1920 (vertical)
   Scenes: <N> images with transitions
   ```

## Error Handling

| Scenario | Action |
|----------|--------|
| Missing API keys | Return clear message listing which keys are needed |
| OpenAI API error | Report the error. Common: rate limit (wait and retry), invalid key |
| DALL-E image generation fails | Pipeline retries 3x automatically. If all fail, report which scene failed |
| ElevenLabs API error | Report error. Common: invalid voice ID, quota exceeded |
| Render fails | Check `npx remotion render` stderr output, report error |
| Template not installed | Provide git clone command |

## Customization

- **Voice**: Pass a different `voice_id` — get IDs from ElevenLabs voice library
- **Story style**: Edit `cli/service.ts` → `getGenerateStoryPrompt()` to change story generation prompt
- **Image style**: Edit `cli/service.ts` → `getGenerateImageDescriptionPrompt()` to change image prompt style
- **Caption style**: Edit `src/components/Word.tsx` for font, color, animation
- **Intro slide**: Edit `src/components/AIVideo.tsx` lines 28-55 for title styling
- **Transitions**: Edit `cli/timeline.ts` — change `enterTransition` / `exitTransition` from "blur" to "fade" or "none"
