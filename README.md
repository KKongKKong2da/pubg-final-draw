# PUBG FINAL DRAW 🪂

**English** | [한국어](README.ko.md)

Current version: **v1.1.0**

Draw algorithm: **los-shuffle-v2**

**PUBG FINAL DRAW** is a web-based random draw tool that takes a participant roster and determines the last person standing, styled after a PUBG spectator minimap.

Instead of a roulette wheel or pinball machine, participants drop onto Erangel, the playzone contracts, and combat and Blue Zone eliminations continue until the final winner is determined. The result is fixed first at the moment the draw begins; the movement and combat shown on screen are a presentation that plays back the predetermined ranking.

It runs from a single `index.html` file with no installation or server required, and supports copying the full ranking, reproducing results from a seed, speed controls, and mobile screens.

> This project is not a game; it is a **draw tool**. By borrowing the visual language of a PUBG esports spectator minimap, it aims to add a story and spectacle to the result of “who was selected.”

---

## 🎯 Why It Was Made

Roulette and pinball draw tools, commonly used internally to select prize winners or determine an order, are convenient, but the process of revealing the result is relatively monotonous.

While watching the **PUBG Nations Cup 2026 (PNC 2026) Grand Finals DAY 3 (the final day) on Sunday, 2026-06-28**, I was impressed by the spectator view, which conveyed the situation through dots and nameplates, the playzone, combat, and eliminations on a full map instead of following all 100 players individually. I thought that applying this view to a draw could turn a simple selection into a small event that everyone could watch together.

This project addresses four goals:

1. **Make the process of selecting one person from many enjoyable to watch, like a battle royale broadcast.**
2. No matter how elaborate the presentation is, **determine the result solely from randomness generated at the moment the draw starts.**
3. Increase a participant's winning probability precisely according to the number of tickets entered with `name*N`.
4. Copy and share the full ranking and seed, then reproduce the result under the same conditions.

---

## 🧠 Key Features

| Feature | Description |
|---|---|
| **Roster and ticket input** | Separate names with commas or line breaks. `name*3` means that the name enters with 3 independent tickets. |
| **Result determined first** | At the moment the draw begins, a result-only random stream shuffles the complete ticket order using Fisher-Yates. Everything shown afterward plays back the determined result. |
| **Drops near major Erangel locations** | Most participants are placed around major locations such as Pochinki, Rozhok, Yasnaya, and School, while the rest are scattered across the battleground. Drop positions do not affect the result. |
| **6-phase playzone** | After a brief initial observation, the next white circle appears first, and the blue playzone contracts over 6 phases. The first circle starts wide, with pressure increasing toward the end. |
| **Distributed movement and spectator pacing** | Participants move toward different positions within the safe zone. Only nearby opponents approach one another to fight, and maximum speed limits during approach, engagement, and disengagement prevent anyone from suddenly flying in from far away. |
| **Distinct combat and Blue Zone eliminations** | Combat and Blue Zone eliminations use different colors and effects, as in `재범님 KILLS 태훈님` and `BLUE ZONE ELIMINATES 태훈님`. |
| **Cheer highlight** | Tap a participant during the draw to highlight every ticket sharing that name while the rest dim. Tap again to clear, or tap another name to move the highlight. It is a viewing aid and does not affect the result. |
| **Late-game camera zoom** | After the first playzone phase ends, the camera smoothly zooms toward the survivors and the playzone. The zoom is limited so that every survivor remains visible on screen. |
| **Fixed pacing and speed controls** | A draw takes approximately 60 seconds at 1× speed. The recommended default is 3× (approximately 20 seconds) for 2–4 total tickets and 2× (approximately 30 seconds) for 5 or more. Speed can be adjusted manually from 0.5× to 3×, and changing it during a draw does not cause time to jump. |
| **Full ranking and copy** | The final ranking of every ticket is preserved. If one name has multiple tickets, it may appear multiple times in the ranking; separate ticket numbers are not displayed. |
| **Seed-based reproduction** | Using the same seed with the same algorithm version, roster, and input order reproduces the same full ranking. |
| **Single-file and mobile support** | The map image and all code are contained in `index.html`. On mobile, the battleground follows the roster, seed, speed, and start controls. When a draw starts, the roster collapses into a summary and the page automatically moves to the battleground. Only the 3 most recent kill-feed entries appear below the map. |

---

## ⚖️ Fairness and Reproducibility

### Separating the result from the presentation

When the start button is pressed, the full ranking of all participating tickets is determined before anything else. Drop positions, the playzone path, movement, combat, and camera zoom use separate random streams that are isolated from the result.

Therefore, even if someone appears to lose a fight or get caught in the Blue Zone, their actual ranking was determined before that scene. Later changes to the presentation code do not affect the result random stream.

### Every ticket has the same probability

`name*3` does not give one name a special bonus. It places 3 independent tickets with that same name into the array. Every ticket is shuffled with the same probability as every other ticket.

For example, if A has 3 tickets while B and C each have 1, A's probability of winning is 3/5, while B's and C's probabilities are each 1/5.

### Reproducing the same result

All of the following must be identical:

- Draw algorithm version
- Participant names and input order
- Number of tickets for each name
- Seed

Copied results include the app version, algorithm version, seed, roster hash, participant count, and ticket count. Reproducibility is governed by the draw algorithm version; the app version is recorded for reference only. Even when the app version increases, the same seed produces the same result as long as the algorithm version is unchanged.

> **Prototypes and prereleases before v1.0.0 may produce different results even with the same seed. Starting with v1.0.0, result compatibility is maintained against `los-shuffle-v2`. For example, v1.0.0 and v1.1.0 share the same algorithm, so the same seed produces the same result.**

### Seed input behavior

- If the field is left empty, a new seed is generated automatically each time a draw starts.
- An automatically generated seed is not inserted into the input field; it appears only on screen and in copied results.
- If a seed is entered manually, the draw starts with that value and the input remains until the user clears it.

---

## 🌱 Development Process — How a Non-Developer Built It with AI

This project was not created by translating a finished specification into code all at once. It was built by explaining ideas to AI, running the result, describing what felt awkward, and iterating on those points.

### 1. Turning a spectator experience into a draw concept

The starting point was the full-map PUBG esports spectator view from **PNC 2026 Finals DAY 3, watched on Sunday, 2026-06-28**. I wanted to bring the experience of “watching the movement and elimination of 100 players on a single map rather than playing the game” into a draw. At this point, I defined the project not as a game, but as a **spectator-style draw tool**.

### 2. Establishing fairness before designing the screen

The first design principle was to “determine the result first and treat everything else as presentation.” I implemented the Fisher-Yates shuffle and independent-ticket model first, verified through repeated runs that outcomes were proportional to ticket counts, and only then added the visuals.

### 3. Adding Erangel and drops near major locations

The first version used a simple grid map, but I added Erangel to immediately convey the PUBG theme. Instead of distributing participants uniformly across empty terrain, I placed them so they would start clustered around major towns, as they would in a real match. The map was compressed as WebP and embedded directly into the HTML.

### 4. Refining the playzone and the rhythm of a match

I fixed the first and final circles and divided the interval between them into 6 phases. I repeatedly adjusted the contraction rates, time per phase, first-circle size, and preview duration. Rather than implementing looting, the sequence was compressed to briefly show drop positions immediately after the start, reveal the white circle first, and then move the blue playzone. Each draw was tuned to take approximately 60 seconds at 1× speed regardless of participant count.

### 5. Giving movement and combat a story

Instead of simply making dots disappear, I added a flow in which nearby participants confront one another, engage in nonlethal combat, scatter and rest afterward, and then encounter other opponents. To avoid forcibly pulling distant opponents together even with small rosters, combat has a maximum engagement distance, while approach, position adjustment, disengagement, and playzone movement have maximum speed limits. Participants outside the playzone move toward safety, while late participants are presented as running toward the boundary before being eliminated by the Blue Zone.

### 6. Comparing it again with an actual spectator screen

I placed real tournament minimap images beside the result and readjusted the white circle, blue danger zone, kill feed, and late-game zoom. As the survivor count falls, the camera moves closer to the playzone, but the zoom is capped so participants do not disappear off screen. Traces of the unimplemented Red Zone were removed.

### 7. Completing the features required of a draw tool

I added full-ranking preservation, ranking copy, seed input and reproduction, participant and ticket counts, input error messages, and a mobile layout. On mobile, the map sits below the start and reset controls so the battleground is immediately accessible after setup. When the draw starts, the roster input collapses into a summary and the page automatically moves to the battleground. To keep the battlefield unobstructed, only the 3 most recent kill-feed entries appear below the map. Finally, result, placement, and movement randomness were separated, and fixed time steps and a Web Crypto-based random seed were introduced to finalize the v1.0.0 architecture.

### Lessons learned

- Writing down the purpose and principles first provides clear criteria for choosing features.
- Convincing visuals and correct results are separate concerns, so both numbers and repeated runs are needed for verification.
- Keeping only the spectator conventions needed for the draw worked better than reproducing the actual game in full.
- Even a non-developer can improve quality with AI by clearly describing the desired experience and the parts that feel awkward.

---

## 🕹️ How to Use

### Basic flow

1. Enter participant names separated by commas or line breaks.
2. To give someone multiple tickets, enter them as `name*3`.
3. Set the speed and seed if needed. The default is 3× for 2–4 total tickets and 2× for 5 or more. Leaving the seed blank generates a new one automatically.
4. Press **시작** to proceed through drop-position observation → playzone contraction → movement and combat → the last person standing.
5. During the draw, tap a participant to highlight everyone sharing that name; tap again or tap empty space to clear.
6. When the draw ends, use **전체 순위 보기** or **순위 복사**.

### Mobile flow

On mobile, the screen is arranged in this order:

```text
Project title
Participant input and participant/ticket counts
Seed and speed
Start and reset
Battleground
Recent kill feed
Fairness information
```

Pressing **시작** automatically collapses the roster input into a `참가자 N명 · 티켓 N장` summary and smoothly moves the screen to the battleground. During the draw, **명단 보기 / 명단 접기** lets you inspect the input, and pressing **리셋** expands the roster input again.

### Default speeds

- **2–4 total tickets**: default **3×** (approximately 20 seconds)
- **5 or more total tickets**: default **2×** (approximately 30 seconds)
- If the speed slider is adjusted manually, the selected value is retained. Pressing **리셋** restores the recommended default for the current ticket count.

### Input requirements

- At least 2 distinct participants are required.
- The total number of tickets must be between 2 and 100.
- Names may contain up to 40 characters.
- N in `name*N` must be an integer of 1 or greater.
- The seed may be left empty or entered as a 1–8 digit hexadecimal value.

### Input example

```text
공주님, 일원님*5, 재범님*5, 하빈님*3, 병식님, 정환님
```

In this example, there are 6 unique participants and 16 total tickets.

### Copied result example

```text
PUBG FINAL DRAW 결과
app: v1.1.0
algorithm: los-shuffle-v2
seed: b4357198
roster-hash: 91d4a1c2
참가자: 6명
티켓: 16장

전체 순위
1위: 재범님
2위: 일원님
3위: 하빈님
...
```

---

## ⚙️ How It Works — A Non-Developer-Friendly Explanation

### The screen and image are included in one file

The visual design, draw logic, Erangel map, and animations are all contained in `index.html`. The map was converted from a WebP image to a Base64 string and embedded in the HTML. It works even when the file is opened directly without an internet connection.

### One seed produces three random streams

The user sees a single seed, but internally the app creates separate random streams for different roles:

1. **Result randomness**: Determines the full ranking.
2. **Placement randomness**: Produces drop positions, the playzone path, and presentation variations in pacing.
3. **Movement randomness**: Produces real-time scenes such as movement, nonlethal combat, and flashes.

### Display refresh rate is separated from match time

The internal simulation advances in fixed time steps. Even though 60Hz and 144Hz monitors render the screen a different number of times, the draw timing remains largely unchanged. Changing the speed during a draw also does not make time jump backward or forward.

### Participant information stays in the browser

The roster and results are processed only in the current browser. This HTML makes no network request that transmits the participant roster to an external server.

---

## 📂 File Structure

The project uses only three files, with no complex development directory:

| File | Description |
|---|---|
| `index.html` | The complete web app, including the map image, draw logic, interface, and animations |
| `README.md` | English project overview, usage, fairness, development process, and deployment guide |
| `README.ko.md` | Korean project overview, usage, fairness, development process, and deployment guide |

---

## 🚀 Running and Deployment

### Run locally

1. Download `index.html`.
2. Open it in a browser such as Chrome, Edge, or Safari.
3. Enter a roster and start the draw.

### Deploy with GitHub Pages

1. Upload `index.html`, `README.md`, and `README.ko.md` to the root of the `pubg-final-draw` repository.
2. Go to the repository's **Settings → Pages**.
3. Select **Deploy from a branch**.
4. Select the `main` branch and `/(root)`, then save.
5. Open the generated Pages URL.

Because `index.html` is the repository's default entry file, no separate build or filename change is required.

---

## 🧪 Pre-Release Verification

### v1.1.0

| Category | Verification |
|---|---|
| **Cheer highlight** | Verified that tapping a participant highlights every ticket sharing that name and dims the rest, tapping the same name clears it, tapping another name moves the highlight, and tapping empty space clears it. Confirmed for both dot and nameplate taps, and that the highlight resets on start and reset |
| **Result independence** | Verified that the cheer highlight does not touch the result random stream: the same seed produces the same full ranking in v1.0.0 and v1.1.0 |

### v1.0.0

To avoid duplicating the development-process narrative, only the following results were regression-tested in the first stable release:

| Category | Verification |
|---|---|
| **Result stability** | Verified that the full ranking remains identical across repeated runs and desktop/mobile environments when the app and algorithm versions, roster, input order, and seed are the same |
| **Algorithm regression** | Verified that the final prerelease and v1.0.0 produce identical full rankings with 3-ticket, 26-ticket, and 38-ticket rosters and multiple fixed seeds |
| **Input validation** | Verified that empty names, a single participant, invalid `name*N` syntax, more than 100 tickets, and invalid seeds are blocked before the draw starts |
| **Seed behavior** | Verified that consecutive runs with an empty field generate a new random seed each time, while a manually entered seed is retained unchanged |
| **Duration** | Verified that the default is 3× (approximately 20 seconds) for 2–4 tickets and 2× (approximately 30 seconds) for 5 or more. A manually selected speed is retained, reset restores the recommended value for the current ticket count, and changing speed during a draw does not cause time to jump backward or forward |
| **Spectator presentation** | Verified that the initial observation and white-circle preview, 6-phase contraction, distributed movement, combat and Blue Zone eliminations, and late-game zoom work in sequence |
| **Small rosters** | Verified that, in draws with 2–4 participants, distant participants do not engage immediately and that approach, engagement, and disengagement begin only at close range. Maximum speed limits for each movement phase and survivor visibility were also verified |
| **Responsive layout** | Verified that the battleground fills the available area on desktop. On mobile, it appears after the settings and start button; the roster collapses at the start, and the page automatically moves to the battleground. The kill feed shows only the 3 most recent entries, each on one line below the map, and stays hidden when there are no entries |
| **Completion and result preservation** | Verified that 3-ticket and default 26-ticket draws finish without browser errors and preserve the complete ranking without omissions. For a 100-ticket roster, input, initial observation, and progression through the first elimination were verified |

---

## 📝 Changelog

### v1.1.0 — Cheer Highlight (2026-07-27)

- **Cheer highlight**: Tap a participant (dot or nameplate) during the draw to highlight every ticket sharing that name while the rest dim. Tap again to clear, tap another name to move the highlight, and tap empty space to clear all. This addresses feedback that attention scatters when one name is entered as multiple tickets.
- The feature is a viewing aid and does not affect the result or seed reproduction. The draw algorithm remains `los-shuffle-v2`, so v1.0.0 seeds reproduce identically in v1.1.0.

### v1.0.0 — First Stable Release (2026-07-22)

- **Finalized the fair draw architecture**: independent tickets, Fisher-Yates shuffle, separated result and presentation randomness, and fixed `los-shuffle-v2`
- **Completed the PUBG spectator presentation**: drops near major Erangel locations, 6-phase playzone, distributed movement, close-range combat with maximum speed limits, combat and Blue Zone eliminations, and a late-game camera
- **Completed practical features**: full ranking, copy, seed-based reproduction, input validation, recommended default speeds based on ticket count, and manual speed controls
- **Finalized the distribution format**: desktop and mobile support, mobile settings → battleground flow, automatic roster collapse, automatic movement to the battleground, 3-entry recent kill feed, single-`index.html` execution, and GitHub Pages support

---

## Copyright Notice

The rights to PUBG: BATTLEGROUNDS, Erangel, and related names and images belong to **KRAFTON, Inc.**

This project is a noncommercial draw tool used internally at KRAFTON. It is neither a game product nor a game client.

---

The last person remains on Erangel, and the final winner is decided. 🏆
