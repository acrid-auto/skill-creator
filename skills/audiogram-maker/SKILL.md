# SKILL: audiogram-maker

## Description

Turns audio clips into audiogram videos with waveform visualization and synchronized captions. Perfect for podcast clips, voice notes, and audio content promotion on social media. Supports oscilloscope and spectrum visualizer modes with customizable colors, cover art, and title overlays.

## Usage

Use when you need to create a visual video from an audio file — podcast clips, voice notes, music previews, or any audio content that needs a visual component for social media.

**Triggers:** "Turn this audio into a video", "Make an audiogram from my podcast", "Create a video for this audio clip"

## Inputs

| Parameter | Required | Type | Default | Description |
|-----------|----------|------|---------|-------------|
| `audio_path` | Yes | string | — | Path to audio file (.wav recommended for long files, .mp3 supported) |
| `title` | Yes | string | — | Title text displayed on the audiogram |
| `cover_image` | No | string | `podcast-cover.jpeg` | Path to cover art image |
| `visualizer` | No | `oscilloscope` \| `spectrum` | `oscilloscope` | Waveform visualization style |
| `color` | No | string | `#ffffff` | Visualizer color (hex) |
| `output_path` | No | string | `./out/audiogram.mp4` | Output file path |

## Prerequisites

- Node.js 18+ and npm installed
- The `template-audiogram` template cloned (included in `remotion-templates/`)
- Bun or tsx installed (for transcription script)
- ffmpeg installed

## Outputs

Rendered MP4 video (1080x1080, 30fps, square format) with:
- Cover image and title at top
- Animated audio visualizer (oscilloscope or spectrum bars)
- Word-by-word synchronized captions at bottom
- Auto-calculated duration from audio length

## Steps

1. **Validate input** — Confirm the audio file exists. For files longer than a few minutes, recommend .wav format for memory efficiency. If file doesn't exist, return error.

2. **Navigate to template** — Change to `remotion-templates/template-audiogram/`.

3. **Install dependencies** — Run `npm install` if needed.

4. **Copy assets** — Copy audio and cover image into `public/`:
   ```bash
   cp "<audio_path>" public/dialogue.wav
   cp "<cover_image>" public/podcast-cover.jpeg  # if custom cover provided
   ```

5. **Generate captions** — Run the transcription script:
   ```bash
   npx tsx transcribe.ts
   ```
   When prompted:
   - Audio file path: `./public/dialogue.wav`
   - Speech start time: `0` (or skip intro seconds)

   This installs Whisper.cpp (first run), transcribes with word-level timestamps, and saves `public/captions.json`.

6. **Configure composition** — Update `src/Root.tsx` defaultProps:
   - `audioFileUrl`: point to the audio file
   - `titleText`: set to the provided title
   - `coverImageUrl`: point to cover image
   - `audioVisualizer`: set visualizer type and color

7. **Render** — Run:
   ```bash
   npx remotion render Audiogram out/audiogram.mp4
   ```

8. **Deliver** — Report output location, duration, and format.

## Error Handling

| Scenario | Action |
|----------|--------|
| Audio file not found | Return path error |
| Audio too long (>30min) | Warn about render time, recommend trimming |
| Whisper produces no captions | Return: "No speech detected. Check audio content." |
| Memory issues with large MP3 | Recommend converting to WAV: `ffmpeg -i input.mp3 input.wav` |
| ffmpeg not installed | Return install instructions |

## Customization

- **Visualizer type**: `oscilloscope` (waveform line) or `spectrum` (frequency bars)
- **Colors**: Set visualizer color, title color, caption color via props
- **Dimensions**: Default 1080x1080 square — change in Root.tsx for other aspect ratios
- **Font**: IBM Plex Sans (weights 500/600) — change in Google Fonts import
- **Caption lines**: Default 5 lines visible — change `LINES_PER_PAGE` in constants
- **Caption font size**: Default 72px — change `CAPTIONS_FONT_SIZE` in constants
