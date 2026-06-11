# Romchyk: The Big Race (Ромчик: Великі Перегони)

A one-button browser game for a 3.5-year-old who loves cars, motorcycles,
drones, and his two huskies. No install, no build, no dependencies — a single
`index.html` that runs in any modern browser on iPhone, iPad, or Mac.

**Play it here: https://colobocman.github.io/romchyk-game/**

## How to play

- Tap anywhere on the screen (or press Space / Arrow Up on a keyboard).
- Ground vehicles (red race car, motorcycle, fire truck, tractor): tap to
  jump (a second tap in the air gives a little double-jump). Collect toys,
  stars, and balls.
- Flying vehicles (drone, night rocket): tap to fly up, release to float down.
- Collecting a bone calls one of the huskies — Sheila (grey) or Rey (white) —
  to run alongside.
- Gifts burst into bonus stars. Three quick collects in a row earn a rainbow
  trail and extra praise.
- Every 10 stars: confetti celebration with the whole family (Tato, Mama,
  Darynka, and both dogs), then the next vehicle. Six stages rotate forever:
  car (meadow) → motorcycle (sunset) → drone (rainbow sky) → fire truck
  (town) → tractor (farm) → rocket (night space with moon and planets).
- There is no way to lose. Puddles just splash and slow you down for a moment.

## Features

- Ukrainian voiceover via the browser's speech synthesis (uses the "Lesya"
  uk-UA voice on Apple devices, with on-screen text bubbles as fallback).
- Procedural chiptune music and sound effects via WebAudio (no audio files).
- All graphics drawn in code on a canvas: parallax hills, smiling sun,
  rainbow, confetti, dust and sparkle particles.
- Responsive: works in landscape and portrait, mouse, touch, and keyboard.
- Mute button in the top-right corner.

## Development

Everything lives in `index.html`. Serve it with any static server:

```sh
python3 -m http.server 8000
```

See [docs/DESIGN.md](docs/DESIGN.md) for the game design notes.
