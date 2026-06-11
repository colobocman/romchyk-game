# Romchyk: The Big Race (Ромчик: Великі Перегони)

A one-button browser game for a 3.5-year-old who loves cars, motorcycles,
drones, and his two huskies. No install, no build, no dependencies — a single
`index.html` that runs in any modern browser on iPhone, iPad, or Mac.

**Play it here: https://colobocman.github.io/romchyk-game/**

## How to play

- Tap anywhere on the screen (or press Space / Arrow Up on a keyboard).
- Car and motorcycle stages: tap to jump (a second tap in the air gives a
  little double-jump). Collect toys, stars, and balls.
- Drone stage: tap to fly up, release to float down.
- Collecting a bone calls one of the huskies — Sheila (grey) or Rey (white) —
  to run alongside.
- Every 10 stars: confetti celebration with the whole family (Tato, Mama,
  Darynka, and both dogs), then the next vehicle.
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
