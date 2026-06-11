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
2. Tap = jump (ground vehicles) or fly up (drone, rocket). Generous magnet
   pulls nearby collectibles in, so timing barely matters.
3. Collect 10 items → celebration: confetti, fanfare, the family cheers
   (Tato, Mama, little sister Darynka, huskies Sheila and Rey), voice praise.
4. Six stages rotate forever: red race car (day meadow) → motorcycle
   (sunset hills) → drone (rainbow sky, flowers) → fire truck (town with
   houses, flashing beacon, siren jingle) → tractor (farm: fence,
   sunflowers, hay, exhaust puffs) → rocket (night space: sleeping moon,
   planets, twinkling stars, meteors, fireflies).

Special collectibles and rewards (v2):
- A bone summons a husky (alternating Sheila/Rey) to run alongside for a few
  seconds with a bark sound.
- A gift box bursts into two bonus stars.
- Three collects within 2.5 s = combo: rainbow trail behind the vehicle, an
  arpeggio jingle, and special praise.
- Voice praise fires on every second collect (pool of 12 phrases, many with
  his name) instead of every third.
- A decorative drone occasionally flies across during ground stages and
  drops a star. Birds flap across day skies.

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
