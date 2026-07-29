# Garden Grower — Math Game for Kids

## Overview
A single-file HTML game (`index.html`) that teaches multiplication and division to a young child (6-8 years old). The game has a magical garden theme: answering math questions correctly grows plants in a garden plot.

## Architecture
- **Single file**: `index.html` — all HTML, CSS, and JavaScript inline. No external dependencies, no build step, no server needed.
- **Offline-capable**: Works from any URL (GitHub Pages). Can also work from `file://`.
- **localStorage**: Saves garden state, level, streak, total plants grown.
- **No frameworks**: Vanilla HTML/CSS/JS only. No React, no Tailwind, no CDN links.
- **No image files**: All visuals are CSS + inline SVG.

## NEW DESIGN VISION — Dreamy & Magical

The UI should feel like a whimsical storybook garden at golden hour. Think soft, warm, glowing, with depth and life. NOT flat. NOT corporate. NOT generic web design. This should delight a 6-year-old.

### Visual Direction
- **Sky/Background**: Animated gradient sky that shifts slowly through warm sunset/twilight colors (soft pinks, lavenders, warm golds, gentle blues). Maybe subtle clouds drifting. A soft glowing sun or moon in the corner.
- **Floating particles**: Gentle floating sparkles, petals, or fireflies drifting across the screen (CSS animations, ~15-20 particles, very subtle, not distracting).
- **Glassmorphism**: Panels (question area, top bar) use frosted glass effect — semi-transparent white with backdrop-blur, soft shadows, rounded corners.
- **Depth**: The garden should feel 3D — like looking down into a real garden bed at an angle. Use CSS perspective and transforms to tilt the garden plane, with soil rows receding into the background. Plants in back rows are slightly smaller. Soil has texture via gradients (darker in back, lighter in front).
- **Glowing accents**: The active slot pulses with a warm golden glow. Correct answers create a burst of sparkles around the plant. 
- **Typography**: Use a rounded, playful font. Since no CDN, use system fonts: `font-family: "Comic Sans MS", "Chalkboard SE", "Comic Neue", -apple-system, system-ui, sans-serif`. Big, bold, friendly.

### Garden with Depth (3D Perspective)
- Wrap the garden in a container with `perspective: 800px` and `transform: rotateX(35deg)` to tilt it like a real garden bed viewed from above-front.
- Soil background: layered gradients to create depth — darker brown at the back, warmer lighter soil at front. Add subtle CSS noise/texture.
- Slots: rounded rectangles with inset shadows to look like dug soil patches. Active slot glows.
- Plants in back rows scale down slightly (0.85x for row 2, 0.75x for row 3) to enhance depth.
- Garden border: a subtle wooden fence or stone border at the back edge (CSS gradient or simple SVG).

### SVG Plants (NOT emoji)
Replace ALL emoji plants with hand-crafted inline SVG illustrations. Create 5 plant types, each with 4 growth stages:

**Plant types:**
1. **Sunflower** 🌻 → tall green stem, large yellow petals, brown center
2. **Tulip** 🌷 → smooth stem, cup-shaped flower in red/pink/purple
3. **Strawberry** 🍓 → low bushy plant, red berries with seeds, green leaves
4. **Carrot** 🥕 → green leafy tops, orange carrot top poking from soil
5. **Tomato** 🍅 → bushy plant, red round fruit, green leaves

**Growth stages (SVG for each):**
- **Stage 0 — Soil**: Small dark mound in the dirt, maybe a tiny crack where seed was planted
- **Stage 1 — Seedling**: Tiny green sprout, 2 small leaves emerging from soil
- **Stage 2 — Growing**: Taller stem with several leaves, bud forming (for flowers) or small green fruit (for fruit plants)
- **Stage 3 — Mature**: Full grown plant with vibrant flower/fruit, full leaves, colorful

Each SVG should be ~80×80 viewBox, use soft rounded shapes, warm colors, and have a gentle CSS animation on appearance (scale up with a bounce, or "grow" from the soil).

### Question Panel (Glassmorphism)
- Frosted glass card: `background: rgba(255,255,255,0.65); backdrop-filter: blur(12px); border-radius: 24px; box-shadow: soft warm glow`.
- Question text: large, centered, warm dark color. Subtle glow behind it.
- Answer buttons: 4 large rounded buttons with soft gradient backgrounds (not flat colors). Each button a different pastel gradient. Press effect: scale down slightly + glow. Correct = green glow burst. Wrong = gentle red shake + dim.
- Buttons should look like magical tiles or gems — rounded, glossy, with subtle inner highlight.

### Top Bar
- Frosted glass pill shape
- Stats (level, streak, grown) as small glass badges with icons (use small inline SVG icons, not emoji)
- Mute button as a small circular glass button with SVG speaker icon

### Level-Up Banner
- Full screen overlay with a magical "burst" animation — expanding ring of sparkles, the level number appears large with a golden glow, soft chime sound.
- More dramatic than current — feels like a reward.

### Animations Summary
- Sky gradient shifts slowly (30s loop)
- Floating particles drift (various speeds, 10-20s loops)
- Plant growth: bounce/scale animation when a stage advances
- Correct answer: sparkle burst at plant location + green glow
- Wrong answer: gentle shake + soft red dim (NOT harsh)
- Level up: full sparkle burst + golden banner
- Buttons: soft press feedback, hover glow on desktop
- Active slot: gentle pulsing golden glow ring

## Game Logic (unchanged from before)

### Garden Mechanics
- 4 columns × 3 rows = 12 slots. Each slot has growth stages 0-3.
- One slot is "active" (highlighted with glow). Each correct answer advances the active plant one stage.
- After 3 correct answers (stages 0→1→2→3), plant is fully grown, next empty slot becomes active.
- When all 12 slots grown → level up (harder problems, new empty garden).
- Wrong answer: plant wilts briefly (droop animation), stays at current stage, new question. No game over.

### Question Generation
- **Level 1**: ×2, ×5, ×10
- **Level 2**: ×3, ×4, ÷2, ÷5
- **Level 3**: ×6, ×7, ×8, ×9, mixed ÷
- **Level 4+**: All tables 1-12, division with larger numbers
- 70% multiplication, 30% division
- 4 answer choices: 1 correct + 3 near-misses (±1, ±2, ±10, digit swaps)

### Persistence (localStorage key "gardenGrower")
```json
{
  "level": 1, "streak": 0, "totalGrown": 0,
  "garden": [{"stage":0,"plant":null}, ...], "activeSlot": 0, "muted": false
}
```
- Auto-save after every answer. Cut Garden clears garden, keeps progress.

### Sound (Web Audio API)
- Correct: happy two-note ding
- Wrong: gentle low boop (NOT harsh buzzer)
- Level up: ascending arpeggio
- Mute toggle saved in localStorage

## Technical Constraints
- MUST be single `index.html` file
- NO external dependencies (no CDN, no npm, no image files)
- NO frameworks — vanilla JS only
- All visuals: CSS + inline SVG only
- MUST work on Mobile Safari (iPad/iPhone) and desktop
- Touch-first: all buttons min 60px, large tap targets
- Responsive: iPad landscape primary, stacks vertically on phone
- Max 1200 lines (the SVGs and CSS will add length — that's fine)
- Keep JS logic clean — the visual overhaul is CSS/SVG, game logic stays the same

## Verification
1. Open index.html in browser — should load with animated sky, floating particles, 3D garden
2. Answer questions — plants should grow with SVG illustrations
3. Wrong answers — gentle feedback, no harsh errors
4. localStorage saves/restores
5. Cut Garden works
6. Level-up banner appears with magical animation
7. Responsive on mobile viewport
8. Check on https://tkhumush.github.io/garden-grower/ after push