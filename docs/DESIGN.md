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
2. Tap = jump (ground vehicles) or fly up (flying vehicles). Generous magnet
   pulls nearby collectibles in, so timing barely matters.
3. Collect 10 items → celebration: confetti, fanfare, the family cheers
   (Tato, Mama, little sister Darynka, huskies Sheila and Rey), voice praise.
4. Ten stages rotate forever (v3): red race car (day meadow) → motorcycle
   (sunset hills) → drone (rainbow sky, flowers) → fire truck (town,
   flashing beacon, siren) → tractor (farm: fence, sunflowers, hay) →
   helicopter (sea: waves, sailboats) → train with numbered wagons
   (mountains, rails, steam) → police car (night city: lit windows,
   streetlights, crescent moon) → plane (above cloud tops, hot-air
   balloons) → rocket (night space: sleeping moon, planets, meteors,
   fireflies).

## Number learning (v3)

Romchyk recognizes digits well, so numbers are woven into the core loop:

- Every collect is counted out loud in Ukrainian («Один!», «Два!»…) with a
  big digit popping at the collection point; every third count appends a
  praise phrase. The HUD progress stars are numbered 1–10.
- Train wagons carry big visible numbers.
- Number hunt mini-game, once per stage (~12 s in): the voice asks «Знайди
  цифру N!» (banner shows the numeral), three numbered balloons drift by
  (no magnet — the child must steer into one). Correct → jingle, praise
  with the digit name, +1 star. Wrong → soft pop, the voice names the wrong
  digit and repeats the question; the balloon respawns. The hunt repeats its
  prompt every 9 s and quietly ends after ~30 s if ignored. No penalties.

## Voice and learning upgrades (v5)

- Emotion presets for speech: praise and turbo are excited (higher pitch,
  faster), celebrations and greetings are warm, hunt instructions are calm
  and slow, animal sounds and bumps are playful. Implemented as per-utterance
  pitch/rate presets.
- Phrase pools roughly doubled (20 praises, 8 celebration lines, variants
  for puddles, gifts, bumps, greetings, per-dog lines); a no-repeat picker
  guarantees the same phrase never plays twice in a row. Voice selection
  prefers enhanced/premium uk-UA voices when installed.
- Star-counting ceremony: every celebration opens with «Порахуймо зірочки!»
  and counts all ten stars aloud one by one over a big numbered star row
  (queued utterances), ending with «Десять зірочок!» plus warm praise.
- Colour hunt: the second hunt of each stage asks for a colour instead of a
  digit («Знайди червону кульку!», grammatically correct accusative forms);
  plain coloured balloons, wrong picks name the actual colour.
- Tappable sheep graze on the farm stage — they hop and bleat («Бе-е!
  Овечка!»).
- Gentle idle nudge: if nothing is collected for ~14 s, a warm encouragement
  line plays («Лови зірочку, Ромчику!»).

## Pace and entertainment (v4)

- Ready-set-go countdown opens every stage: big numerals 1-2-3 with voice
  («Раз! Два! Три! Поїхали!») while the vehicle can hop in place.
- Lightning bolt (⚡) collectible triggers a 4-second turbo: 1.55× speed,
  wind streaks, rainbow trail, «Турбо!» voice line.
- The world speeds up ~1.5% per collected star, so each stage accelerates
  toward its celebration.
- Two number hunts per stage (at ~12 s and ~34 s); item spawns are slightly
  sparser, so a stage lasts noticeably longer.
- Funny no-fail obstacles per vehicle: traffic cones that fly apart
  (roads), a hay bale that bursts into straw (tractor), a cow that leaps
  over the train with a startled «Му-у!» (no slowdown — pure comedy),
  grumpy thunderclouds that buzz and nudge flyers down, and crumbling
  asteroids in space. Hits cost at most a brief slowdown.
- Celebrations are longer (6.8 s) with fireworks bursts, and Romchyk
  himself bounces in the middle of the family line-up.
- The chiptune now alternates A and B melodies every four bars.

## Tap interactivity (v3)

Cause-and-effect toys on top of the one-button core (the tap still jumps):

- Tap the vehicle → vehicle-specific honk (beep, vroom, siren, putt-putt,
  choo-choo, whistle, rumble) + squash bounce, sometimes a spoken word
  («Бі-бі!», «Чух-чух!»).
- Tap the running husky → bark + floating hearts, sometimes her name.
- Tap the sun/moon → giggle arpeggio + sun rays spin fast for 2 s.

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
