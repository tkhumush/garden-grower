# Garden Grower — Math Game for Kids

## Overview
A single-file HTML game (`index.html`) that teaches multiplication and division to a young child (6-8 years old). The game has a garden theme: answering math questions correctly grows plants in a garden plot.

## Architecture
- **Single file**: `index.html` — all HTML, CSS, and JavaScript inline. No external dependencies, no build step, no server needed.
- **Offline-capable**: Works from `file://` URL (opened from iPad Files app or AirDrop).
- **localStorage**: Saves garden state, level, streak, total plants grown. Works with `file://` origin.
- **No frameworks**: Vanilla HTML/CSS/JS only. No React, no Tailwind, no CDN links.

## Game Design

### Layout (iPad Landscape — 1024×768 target, but responsive)
- **Left ~60%**: Garden grid (4 columns × 3 rows = 12 slots). Each slot shows growth stages.
- **Right ~40%**: Question panel with big math question + 4 large answer buttons.
- **Top bar**: Title, level indicator, streak counter, plants grown counter.
- **Bottom or sidebar**: "✂️ Cut Garden" button.

### Garden Mechanics
- Each garden slot has growth stages: 🟫 (empty soil) → 🌱 (seed) → 🌿 (sprout) → 🌷/🍅/🌻/🍓 (fully grown, random plant emoji)
- One slot is "active" (highlighted). Each correct answer advances the active plant one stage.
- After ~4 correct answers, the plant is fully grown and a new empty slot becomes active.
- When all 12 slots are grown → level up (harder problems, new empty garden).
- **Wrong answer**: Plant wilts slightly (show 🥀 briefly), stays at current stage, new question generated. No "game over" — always encouraging.

### Question Generation
- **Level 1**: ×2, ×5, ×10 (easy starters)
- **Level 2**: ×3, ×4, ÷2, ÷5
- **Level 3**: ×6, ×7, ×8, ×9, mixed ÷
- **Level 4+**: All multiplication tables 1-12, division with larger numbers
- Generate 4 answer choices: 1 correct + 3 plausible wrong answers (near-misses, e.g. correct±1, correct±2, swapped digits)
- Mix multiplication (70%) and division (30%) within each level

### Persistence (localStorage)
Save to `localStorage` with key `gardenGrower`:
```json
{
  "level": 1,
  "streak": 0,
  "totalGrown": 0,
  "garden": [...],  // array of 12 slots, each with {stage, plant} 
  "activeSlot": 0
}
```
- Auto-save after every answer
- "✂️ Cut Garden" clears garden to all empty soil but keeps level, streak, totalGrown
- On load, restore saved state

### UI/UX Requirements
- **Touch-first**: All buttons minimum 60px height, large tap targets
- **Big, colorful, playful**: Rounded corners, bright colors, fun emoji
- **Instant feedback**: Green flash + ✓ for correct, gentle shake + red for wrong
- **Animations**: CSS transitions for plant growth, button presses, feedback
- **Font**: System font stack, large sizes (questions 3rem, answers 2rem)
- **Sound**: Optional — simple Web Audio API beeps (correct = happy ding, wrong = gentle boop). Mute button in top bar.
- **No login, no text input, no keyboard needed** — purely tap-based
- **Responsive**: Works on iPad (primary), iPhone (secondary), desktop browser (fallback)

## Constraints
- MUST be a single `index.html` file
- NO external dependencies (no CDN, no npm, no images)
- NO frameworks — vanilla JS only
- MUST work offline from `file://` URL
- MUST work on Mobile Safari (iPad/iPhone)
- All emoji used for visuals (no image files needed)
- Target audience: 6-8 year old. Keep it simple, encouraging, fun.
- Max 800 lines total. Keep it clean and well-commented.

## Verification
After building:
1. Open `index.html` in a browser to verify it loads
2. Test that questions appear and answers work
3. Test that plants grow on correct answers
4. Test that localStorage saves and restores
5. Test the "Cut Garden" button
6. Test on mobile viewport (responsive)