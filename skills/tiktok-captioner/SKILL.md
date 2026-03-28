# SKILL: tiktok-captioner

## Description

Adds TikTok-style word-by-word animated captions to any video. Uses Whisper.cpp for speech-to-text transcription with token-level timestamps, then renders the video with animated caption overlays — highlighted words, spring entrance animations, and bold uppercase styling. Drop in a video, get back a captioned version ready for social media.

## Usage

Use when you have a video file and want to add animated captions/subtitles in the TikTok word-highlight style.

**Triggers:** "Add captions to this video", "Caption this video TikTok style", "Add subtitles to my video"

## Inputs

| Parameter | Required | Type | Default | Description |
|-----------|----------|------|---------|-------------|
| `video_path` | Yes | string | — | Path to the input video file (.mp4, .webm, .mkv, .mov) |
| `highlight_color` | No | string | `#39E508` | Color for the currently spoken word (hex) |
| `caption_switch_ms` | No | number | `1200` | How often captions refresh in ms. Use 200 for one-word-at-a-time, 1500 for phrases |
| `language` | No | string | `en` | Language code for Whisper transcription |
| `output_path` | No | string | `./out/captioned.mp4` | Output file path |

## Prerequisites

- Node.js 18+ and npm installed
- The `template-tiktok` template cloned (included in `remotion-templates/`)
- ~2.5GB disk space for Whisper.cpp model (auto-downloads on first run)
- ffmpeg installed (for audio extraction)

## Outputs

Rendered MP4 video with TikTok-style animated captions:
- Bold uppercase text with black stroke outline
- Active word highlighted in accent color
- Spring entrance animation (scale + slide up)
- Positioned at bottom of frame

## Steps

1. **Validate input** — Confirm the video file exists at `video_path`. Supported formats: .mp4, .webm, .mkv, .mov. If file doesn't exist, return: "Video file not found at '<video_path>'."

2. **Navigate to template** — Change to the `remotion-templates/template-tiktok/` directory.

3. **Install dependencies** — Run `npm install` if `node_modules/` doesn't exist.

4. **Copy video to project** — Copy the input video to `public/` directory:
   ```bash
   cp "<video_path>" public/input-video.mp4
   ```

5. **Generate captions** — Run the Whisper.cpp transcription:
   ```bash
   node sub.mjs public/input-video.mp4
   ```
   This will:
   - Auto-install Whisper.cpp binary (first run only)
   - Download the medium.en model (~1.5GB, first run only)
   - Extract audio as 16kHz WAV
   - Transcribe with word-level timestamps
   - Save captions to `public/subs/input-video.json`

6. **Configure the composition** — Update `src/Root.tsx` to point to the input video:
   - Set `videoSource` to `staticFile("input-video.mp4")`
   - The caption file is auto-detected from the video filename

7. **Apply customizations** — If custom colors or timing requested:
   - Edit `src/CaptionedVideo/Page.tsx`: set `HIGHLIGHT_COLOR` to the requested color
   - Edit `src/CaptionedVideo/index.tsx`: set `SWITCH_CAPTIONS_EVERY_MS` to the requested value

8. **Render the video** — Run:
   ```bash
   npx remotion render CaptionedVideo out/captioned.mp4
   ```

9. **Deliver** — Report:
   ```
   Captioned video rendered: <output_path>
   Words transcribed: <N>
   Highlight color: <color>
   Caption refresh: <ms>ms
   ```

## Error Handling

| Scenario | Action |
|----------|--------|
| Video file not found | Return path error with suggestion to check the path |
| Unsupported video format | Return: "Unsupported format. Use .mp4, .webm, .mkv, or .mov" |
| Whisper download fails | Retry once. If still fails, report network error |
| Transcription produces no words | Return: "No speech detected in video. Check audio track." |
| ffmpeg not installed | Return: "ffmpeg is required. Install with: apt install ffmpeg / brew install ffmpeg" |
| Render fails | Check stderr, report Remotion error |

## Customization

- **Font**: Replace `public/theboldfont.ttf` with any TTF font, update `src/load-font.ts`
- **Highlight color**: Change `HIGHLIGHT_COLOR` in `Page.tsx` (default: lime green #39E508)
- **Caption size**: Adjust `fitText()` maxFontSize in `Page.tsx` (default: 120px)
- **Position**: Change `bottom: 350` in `Page.tsx` container style
- **Animation**: Modify spring params in `SubtitlePage.tsx` (damping, mass, stiffness)
- **Whisper model**: Change model in `whisper-config.mjs` (tiny=fast/low quality, large-v3=slow/best quality)
- **Caption frequency**: `SWITCH_CAPTIONS_EVERY_MS` — 200 for word-by-word, 1200 default, 1500 for phrases
