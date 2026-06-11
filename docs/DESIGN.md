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

## Letters, switches, and robustness (v9)

- Letter hunt: «Знайди літеру А!» with three lettered balloons (10 curated
  letters: А Б В Д І К М О С Т, each voiced by its name — бе, ве, де…).
  Right answers and gentle corrections also voice the letters.
- Title-screen switches «Цифри» and «Літери» (persisted in localStorage)
  enable digit hunts + the number-track gate, and letter hunts. Colours
  are always on. Hunt types rotate through the enabled pool.
- Hunts are now truly the only task on screen: collectibles are cleared
  when a hunt or gate starts and nothing spawns until it ends; obstacles
  pause too; ground balloon slots were raised so no balloon ever bumps
  into the vehicle by itself — answering always requires a jump.
- Bug fixes from a full review: a generation token prevents a stale voice
  clip from playing over a newer one after stop(); clip fetches time out
  after 5 s and network failures are retried instead of being cached as
  permanently missing; the decoded-audio cache is capped at 90 clips
  (memory on older iPhones); the finishing gate has a watchdog that
  restarts it if it ever gets lost; the idle encouragement line no longer
  fires during hunts; number-track distractors can't equal a digit already
  visible on the cards; and the countdown world idles forward slowly so
  the screen never looks frozen.

## Calmer screen and the number track (v8)

- Less clutter: collectibles spawn one at a time (no more arcs, zigzags,
  or parades), at most three on screen, with longer gaps between them.
  Every item is worth exactly one star (gifts no longer burst into bonus
  stars), the flyby drone drops nothing, and obstacles never spawn while
  a hunt or gate is on screen.
- The math gate is gone. The new number-track gate shows three big cards
  front and centre with one blank: [2][3][?] continues the count («Порахуй:
  два, три… Яка цифра йде далі?») and [2][?][4] is a lost number («Ой!
  Цифра загубилась!»). The visible gap conveys "next"/"missing" without
  the words «перед»/«після», which Romchyk doesn't know yet. Rows live in
  1–5. Correct answers fill the card green and the voice reads the whole
  completed row; wrong answers get the usual friendly correction; an
  ignored question reveals itself after ~26 s and the party starts anyway.

## Neural voice (v7)

- All 419 possible utterances are pre-generated as mp3 clips with the
  uk-UA-PolinaNeural voice (Microsoft Edge TTS) by
  `tools/generate_voice.py` — dramatically more human than the browser
  synthesizer. Emotion prosody (rate/pitch per category) is baked into
  each clip.
- Clips live in `audio/v_<djb2>.mp3`; the game hashes the phrase text
  (stress marks stripped) with the same djb2 at runtime, fetches lazily,
  decodes through WebAudio, and caches. Music ducks while the voice talks.
- Core clips (numbers, countdown) preload right after the PLAY tap.
- The browser's speechSynthesis (Lesya) remains as an offline/missing-clip
  fallback, still with stress marks and emotion presets.
- Compound phrases were split into separate utterances (count + praise,
  ten-stars + story payoff) so every line maps to one clip.

## Stories, family, and the math gate (v6)

- Correct word stress: phrases carry combining-acute marks (Ро́мчик,
  По́їзд) so the TTS accents the right syllable; the on-screen bubble
  strips the marks. A '{n}' slot in phrases is filled with a random
  affectionate address (Ро́мчику, Рома́сику, Ро́мцю, со́нечко, чемпіо́не,
  дру́же) for livelier speech.
- Mission stories: each stage opens with a randomly picked one-line intro
  (4 per vehicle, 40 total) told during the extended countdown — deliver
  pies for grandma Galya, find Tyson's ball, return a lost star to the sky.
  The celebration closes the same story with its payoff line.
- New family: grandma Galya (big house, small dog Tyson) and grandma Anya
  (apartment, glasses) appear in stories, in celebration line-ups (one
  visits per celebration; Galya brings Tyson), and Tyson joins the bone
  companion rotation alongside Sheila and Rey.
- Math gate: after the tenth star, a tiny addition question (sums ≤ 5)
  on the familiar digit balloons unlocks the celebration. Wrong answers
  get a friendly correction and retry; an ignored question auto-passes
  after ~28 s so the party is never blocked.
- Baby Darynka is six months old: she sits on a blanket, waves her arms,
  and wiggles instead of jumping.
- Stage intros are ~3 s longer (story time), making each level last
  noticeably longer together with the math gate.

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
