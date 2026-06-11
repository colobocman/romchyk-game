# Romchyk: The Big Race (Ромчик: Великі Перегони)

A one-button browser game for a 3.5-year-old who loves cars, motorcycles,
drones, and his two huskies. No install, no build, no dependencies — a single
`index.html` that runs in any modern browser on iPhone, iPad, or Mac.

**Play it here: https://colobocman.github.io/romchyk-game/**

## How to play

- Tap anywhere on the screen (or press Space / Arrow Up on a keyboard).
- Ground vehicles: tap to jump (a second tap in the air gives a little
  double-jump). Flying vehicles: tap to fly up, release to float down.
- Ten stages rotate forever: race car (meadow) → motorcycle (sunset) →
  drone (rainbow sky) → fire truck (town) → tractor (farm) → helicopter
  (sea with sailboats) → train with numbered wagons (mountains) → police car
  (night city) → plane (above the clouds, hot-air balloons) → rocket (night
  space with moon and planets).
- Every 10 stars: confetti celebration with the whole family (Tato, Mama,
  Darynka, and both dogs), then the next vehicle.
- There is no way to lose. Puddles just splash and slow you down for a moment.

### Number learning

- The game counts every collected toy out loud in Ukrainian (1 to 10), a big
  digit pops on screen, and the progress stars are numbered 1–10.
- Once per stage, a "find the number" mini-game starts: the voice asks for a
  digit and three numbered balloons float by. The right one earns a star and
  praise; a wrong one gently names itself and asks again.

### Tap toys

- Tap the vehicle: it honks its own sound (beep, vroom, siren, choo-choo…).
- Tap a running husky: bark and floating hearts.
- Tap the sun or the moon: a giggle and fast-spinning sun rays.
- Bones call Sheila or Rey to run alongside. Gifts burst into bonus stars.
  Three quick collects in a row earn a rainbow trail and extra praise.

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
