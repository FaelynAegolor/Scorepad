<div align="center">
  <img src="icon-512.png" width="96" height="96" alt="Scorepad logo, an amber tally-mark badge">

  # Scorepad<span>.uk</span>

  **A free scorekeeper for board and card games. It does the sums, remembers the rules, and can even sync live across everyone's phones at the table. No passing the pad required.**

  [![Live site](https://img.shields.io/badge/live-scorepad.uk-f4b942?style=for-the-badge&labelColor=1a1226)](https://scorepad.uk)
  [![License](https://img.shields.io/badge/license-GPL--3.0-f4b942?style=for-the-badge&labelColor=1a1226)](LICENSE)
  [![Build step](https://img.shields.io/badge/build%20step-none-f4b942?style=for-the-badge&labelColor=1a1226)](index.html)
  [![Installable](https://img.shields.io/badge/installable-PWA-f4b942?style=for-the-badge&labelColor=1a1226)](#deal-it-onto-your-home-screen)
</div>

---

No app store, no sign-up, no ads, no dice required. Just open it, name your players, and start keeping score. Scorepad handles the maths while you handle the trash talk.

## What's in the box

- **Three ways to keep score**
  - **Running score:** tap `−`/`+` beside a player's number to nudge it live, or tap the number itself for a full keypad. A little pill pops up to show your last few taps, so a fat-fingered +50 doesn't go unnoticed.
  - **Round-by-round:** enter each player's score for the hand on their own tile (in any order; the tile darkens once it's in, with a green tick to prove it), then hit **Add round** to lock it in. Tap a round's label to rename it, if "Round 3" doesn't quite capture what just happened. Round history, running totals, and a gold underline on the leader are all tracked in one shared table, and you can switch freely between running and round-by-round mid-game without losing a single point.
  - **Category table** (Wingspan, Wyrmspan, Yahtzee): end-game scoring games don't have "rounds" so much as a fixed list of categories to total up. Every category is on the table from the start, dice already metaphorically rolled. Tap any row to make it the one your taps and keypad entries target, or step through them one at a time with the ▲▼. Either way, no hunting for tiny buttons on a phone screen.
- **Game presets:** Flip 7, Sushi Go!, Star Realms, and Yu-Gi-Oh! each swap in a custom card/counter picker and sensible starting defaults (health totals, countdown targets, the works), so the app already knows the house rules better than half the table. There's also a generic **Life total** mode for anything else that just needs a shared starting number ticking down.
- **Saved setups:** got a regular group? Save your players (and the game they usually play) as a named setup, and next time it's one tap instead of typing everyone's name in again. It lives in its own personal library that survives even a full Reset everything, the same way your theme choice already does.
- **Ranking, your way:** most games are highest-score-wins by default, but for the sneaky ones where the *lowest* score takes it (Hearts, Golf, and friends), a one-tap "Lowest score wins" toggle in Scoring options flips the standings, the winner banner, and the leader glow to match. No mental subtraction required.
- **Play together, live:** share a 6-character code, or let someone scan a QR code straight to the join screen (no typing, no squinting at a tiny code across the table), and everyone sees every tap, rename, and round land on their own phone in real time, at the table or across the internet. No accounts, just a code; leave any time and your game quietly reverts to local-only.
- **Player colours:** everyone gets their own hue from an 8-colour palette, and the current leader's tile gets a properly smug 3D glow.
- **Six themes:** Faelyn, Martha, Blaugen, Tarquin, Sleevey Barry, and Boo-bees, each with its own fonts, palette, and background treatment rather than just a colour swap. A first-ever visit matches your device's own light or dark setting automatically; pick one yourself under Colour Theme and that choice sticks for good, OS setting or not.
- **A proper finish:** a winner screen with confetti, ranked standings, a "Share results" button, and a "Save as image" button that renders a branded PNG for the group chat.
- **Works offline, saves locally:** your game lives in your browser's storage by default. No account, no server, no one peeking at your suspiciously high Sushi Go! score, unless you turn on Play together for that one game, which only ever shares that one game's code, never anything else on your device.

## Deal it onto your home screen

Scorepad is installable. Add it to your phone's home screen and it opens full-screen, no browser chrome, works offline, icon and all. On Android/Chrome/Edge you'll get a native install prompt; on iPhone (Safari doesn't allow that) it'll walk you through the manual **Share → Add to Home Screen** steps instead.

## Under the hood

The whole app is a single `index.html` file: no build step, no bundler, no `node_modules` to summon. HTML, CSS, and JavaScript all live together, with only Google Fonts (and, for sync, the Firebase SDK) loaded externally. A handful of small files (`manifest.json`, `sw.js`, and a few icons) sit alongside it purely to make the "install as an app" trick work.

Open `index.html` in a browser and you're playing. Deploying is just as simple: this repo ships straight to [GitHub Pages](https://pages.github.com/), pointed at [scorepad.uk](https://scorepad.uk) via the `CNAME` file.

```
Scorepad/
├── index.html               the whole app
├── manifest.json            PWA metadata
├── sw.js                    service worker (offline + install support)
├── icon-*.png               home screen icons
├── apple-touch-icon.png     iOS home screen icon
└── CNAME                    custom domain for GitHub Pages
```

**Scoring model.** Every game is stored as one flat, chronological ledger (`state.rounds`): score entries and round-boundary markers interleaved in a single array, rather than separate "rounds" and "totals" structures kept in sync by hand. Round-by-round display, running totals, and category tables are all just different ways of reading the same ledger, which is what lets you switch scoring modes mid-game, undo cleanly, and edit any past entry without the rest of the game's math drifting out of sync.

**Live sync.** "Play together" is backed by a Firebase Realtime Database. Players and round entries sync as individually-diffed children rather than one big object overwrite, so two people editing different things at the same moment (everyone tapping their own score at once, say) don't stomp on each other; only a genuine edit to the *same* field ever has to pick a winner. Each in-flight write is tracked per field, so an older echo can never regress a newer local edit while it's still settling. This matters because Firebase's local cache can echo a write straight back to the same tab that just made it, sometimes mid-way through a bigger change (seeding a fresh Wingspan scoresheet's nine categories, say) and before the rest of it has gone out. A small reentrancy guard makes sure a same-tab echo like that is never mistaken for the new source of truth until the whole change has actually finished sending. Everything you enter offline still lands in your browser's local storage first; syncing is a mirror on top, not a replacement for it.

**Concurrency-safe scoring.** Category-table taps append a small delta entry to the ledger rather than reading a category's current total and overwriting it with a new absolute value. The latter shape doesn't survive two devices nudging the *same* category around the same moment: each device can only clear entries it's already seen locally, so two overlapping taps would each insert their own "correct" total instead of one superseding the other, and the total silently doubles. A delta-only append has no "current total" to race on: however many entries land in a segment, from however many devices, in whatever order, they just sum to the right answer. That's the same reason plain running-mode taps were never vulnerable to this shape of bug in the first place.

**QR codes, no CDN.** The QR generator behind "Play together" is vendored straight into `index.html` rather than pulled from a CDN at request time. The app is meant to work fully offline via its service worker, and a third-party script tag would quietly break that promise on exactly the trip where there's no signal to fall back on.

**No React, no framework.** Rendering is plain DOM manipulation (`createElement`, template strings, event listeners) rather than anything fancier. That's deliberate, to keep the "open one file, read the whole app" property that makes a project like this easy to poke at and extend without a build pipeline getting in the way.

## Got a game to suggest?

Scorepad's preset list is short on purpose: every game gets built properly or not at all, no half-built houses (of cards) here. If there's a game you'd love to see with its own keypad and rules baked in, [send a suggestion](mailto:hello@scorepad.uk?subject=Game%20suggestion). There's a shortcut for it right in the app's menu too.

## The rulebook (license)

Released under the [GNU GPLv3](LICENSE), plus one additional term: fork it, remix it, run your own house rules, deploy it wherever you like. It's free to use. Just keep two things intact:

1. **Keep it open.** Any modified or hosted version must stay GPLv3 too. No closing the source off.
2. **Keep the credit.** Visible attribution to Scorepad.uk and its original author must stay wherever the software or a fork of it is used or deployed. It can't be passed off as someone else's original work.

See the [LICENSE](LICENSE) file for the full legal text.

## Say thanks

If Scorepad's saved you from an argument about whether that was really 47 points in Sushi Go!, consider [buying a coffee on Ko-fi](https://ko-fi.com/faelynaegolor). Entirely optional, much like reading the rulebook before round one.

<div align="center">
  <sub>Built for game night. No dice were harmed in the making of this scoreboard.</sub>
</div>
