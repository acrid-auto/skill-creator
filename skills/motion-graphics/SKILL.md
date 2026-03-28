# SKILL: motion-graphics

## Description

Generates motion graphics videos from natural language prompts. Describe what you want — kinetic typography, data visualizations, animated charts, logo reveals, UI mockups, chat bubbles, product showcases — and it generates the React/Remotion code, compiles it, and renders to MP4. Powered by AI code generation with auto-correction for compilation errors.

## Usage

Use when you need animated motion graphics, data visualizations, kinetic text, or any programmatic animation described in plain language. No video editing skills needed.

**Triggers:** "Create a motion graphic of...", "Animate a chart showing...", "Make a text animation that...", "Generate a logo reveal for..."

## Inputs

| Parameter | Required | Type | Default | Description |
|-----------|----------|------|---------|-------------|
| `prompt` | Yes | string | — | Natural language description of the motion graphic |
| `duration_frames` | No | number | `150` | Video length in frames (at 30fps: 150 = 5 seconds) |
| `fps` | No | number | `30` | Frames per second |
| `width` | No | number | `1920` | Video width in pixels |
| `height` | No | number | `1080` | Video height in pixels |
| `output_path` | No | string | `./out/motion.mp4` | Output file path |

## Prerequisites

- Node.js 18+ and npm installed
- The `template-prompt-to-motion-graphics-saas` template cloned (included in `remotion-templates/`)
- Environment variable: `OPENAI_API_KEY=sk-...`

## Outputs

Rendered MP4 video with programmatic motion graphics matching the prompt description.

## Supported Content Types

| Type | Examples |
|------|---------|
| **Kinetic typography** | Typewriter effects, word carousels, animated text reveals |
| **Data visualizations** | Bar charts, pie charts, line graphs, progress bars, counters |
| **UI mockups** | Chat bubbles, app interfaces, notification animations |
| **Logo animations** | Brand reveals, animated logos, intro sequences |
| **Abstract motion** | Geometric patterns, particle effects, bouncing shapes |
| **Social media** | Story templates, Reels covers, promotional animations |
| **3D graphics** | Rotating objects, 3D scenes via Three.js |
| **Transitions** | Scene transitions, fades, slides, wipes |

## Steps

1. **Validate prerequisites** — Check `OPENAI_API_KEY` is set.

2. **Navigate to template** — Change to `remotion-templates/template-prompt-to-motion-graphics-saas/`.

3. **Install dependencies** — Run `npm install` if needed.

4. **Validate the prompt** — The prompt must describe a visual/animated scene. If it's a question or non-visual request, return: "Please describe a visual animation. Example: 'An animated bar chart showing quarterly revenue growth with spring animations.'"

5. **Detect required skills** — Analyze the prompt to determine which Remotion capabilities are needed:
   - `typography` — text animations, fonts, layout
   - `charts` — data visualization, graphs
   - `messaging` — chat UI, bubble animations
   - `transitions` — scene changes, fades
   - `sequencing` — multi-part compositions
   - `spring-physics` — natural motion, bouncing
   - `social-media` — platform-specific formats
   - `3d` — Three.js 3D graphics

6. **Generate the component code** — Send the prompt to GPT with the system prompt and relevant skill context. The AI generates a self-contained React/Remotion component that:
   - Starts with constants in UPPER_SNAKE_CASE
   - Uses `useCurrentFrame()` and `useVideoConfig()` for animation
   - Uses `spring()` for natural motion and `interpolate()` for value mapping
   - Returns JSX wrapped in `<AbsoluteFill>`

7. **Sanitize the output** — Strip markdown fences, validate JSX presence, extract clean component code.

8. **Compile and test** — Transpile the generated code with Babel. If compilation fails:
   - Feed the error back to the AI with the failing code
   - Request a fix (up to 3 auto-correction attempts)
   - If still failing after 3 attempts, report the error

9. **Write the component** — Save the generated component to the Remotion project source.

10. **Render** — Run:
    ```bash
    npx remotion render DynamicComp out/motion.mp4 \
      --props='{"code": "<generated_code>"}' \
      --frames=<duration_frames> \
      --fps=<fps>
    ```

11. **Deliver** — Report output location, duration, resolution.

## Error Handling

| Scenario | Action |
|----------|--------|
| Non-visual prompt | Return validation error with example prompts |
| OpenAI API error | Report error, suggest checking API key and quota |
| Code generation produces invalid JSX | Auto-correct up to 3 times, then report |
| Runtime error in animation | Feed error + code back to AI for fix |
| Render fails | Report Remotion error from stderr |

## Example Prompts

**Kinetic text:**
```
A typewriter animation that types out "THE FUTURE IS NOW" letter by letter,
then highlights each word in sequence with a golden glow
```

**Data viz:**
```
An animated bar chart showing gold prices from 2020-2024,
bars grow from bottom with spring animation, values labeled on top
```

**Chat UI:**
```
An iMessage conversation where two people discuss AI,
messages appear one by one with spring bounce animations
```

**Logo reveal:**
```
A logo that assembles from scattered geometric shapes,
pieces fly in from edges and lock into place with satisfying snap
```
