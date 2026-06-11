# Game Design — Romchyk: The Big Race

Date: 2026-06-11

## Goal

A browser game for Romchyk (3.5 years old) that his parents can open from a
link on any phone or laptop. He loves cars (and the Cars cartoon),
motorcycles, drones, toys, and the family's two huskies. Controls must work
for a small child: one input, no fail state, frequent rewards.

## Audience constraints → design decisions

| Constraint | Decision |
|---|---|
| 3.5-year-old motor skills | Single input: tap anywhere / Space. Vehicle drives itself. |
| Frustration tolerance | No game over. Puddles only splash and briefly slow down. |
| Attention span | Reward every 1–2 seconds (collectible), big celebration every ~30 s. |
| Reading ability: none | All feedback is voice (uk-UA TTS) + pictures; text is decorative. |
| Distribution: "send mom a link" | Single static `index.html`, hosted on GitHub Pages. |

## Core loop

1. Vehicle auto-drives to the right through a scrolling world.
2. Tap = jump (car, motorcycle) or fly up (drone). Generous magnet pulls
   nearby collectibles in, so timing barely matters.
3. Collect 10 items → celebration: confetti, fanfare, the family cheers
   (Tato, Mama, little sister Darynka, huskies Sheila and Rey), voice praise.
4. Vehicle rotates: red race car (day meadow) → motorcycle (sunset hills) →
   drone (sky with rainbow and flowers) → back to car. Infinite.

Special collectible: a bone summons a husky (alternating Sheila/Rey) to run
alongside for a few seconds with a bark sound. A decorative drone
occasionally flies across during ground stages and drops a star.

## Technology

- **One file**, no dependencies, no build. Canvas 2D for everything except
  the HTML title overlay, mute button, and speech bubble.
- **Music/SFX**: WebAudio, procedurally scheduled 4-bar pentatonic chiptune
  loop; per-stage lead waveform and tempo. SFX synthesized (jump, collect,
  splash, bark, fanfare, drone hum).
- **Voice**: `speechSynthesis` with a uk-UA voice (prefers "Lesya", standard
  on Apple devices). Every spoken phrase also appears as a text bubble, so
  missing voices degrade gracefully. Audio and TTS are unlocked by the PLAY
  button gesture (iOS requirement).
- **Scaling**: logical world height 540 px in landscape; in portrait the
  world width is pinned to ≥560 so gameplay stays large. Ground is anchored
  to the bottom of the screen, sky stretches. DPR-aware rendering.
- **Strings**: all Ukrainian user-facing strings live in one `UA` constant
  (the product is localized for a Ukrainian child; code and comments are
  English).

## Explicitly out of scope

- Score persistence, levels, difficulty — pointless at this age.
- External assets (images, fonts, audio files) — keeps it a single file.
- Copyrighted characters: the car is "inspired by" cartoon race cars (eyes,
  smile, number 3 for his age) but is original art.
