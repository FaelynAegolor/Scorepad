<div align="center">
  <img src="icon-512.png" width="96" height="96" alt="Scorepad logo — an amber tally-mark badge">

  # Scorepad<span>.uk</span>

  **A free scorekeeper for board and card games — it does the sums, remembers the rules, and can even sync live across everyone's phones at the table.**

  [![Live site](https://img.shields.io/badge/live-scorepad.uk-f4b942?style=for-the-badge&labelColor=1a1226)](https://scorepad.uk)
  [![License](https://img.shields.io/badge/license-GPL--3.0-f4b942?style=for-the-badge&labelColor=1a1226)](LICENSE)
  [![Build step](https://img.shields.io/badge/build%20step-none-f4b942?style=for-the-badge&labelColor=1a1226)](index.html)
  [![Installable](https://img.shields.io/badge/installable-PWA-f4b942?style=for-the-badge&labelColor=1a1226)](#deal-it-onto-your-home-screen)
</div>

---

No app store, no sign-up, no ads, no dice required. Just open it, name your players, and start keeping score — Scorepad handles the maths while you handle the trash talk.

## What's in the box

- **Three ways to keep score**
  - **Running score** — tap `−`/`+` beside a player's number to nudge it live, or tap the number itself for a full keypad. A little pill pops up to show your last few taps, so a fat-fingered +50 doesn't go unnoticed.
  - **Round-by-round** — enter each player's score for the hand on their own tile (in any order — the tile darkens once it's in, with a green tick to prove it), then hit **Add round** to lock it in. Round history, running totals, and a gold underline on the leader are all tracked in one shared table — and you can switch freely between running and round-by-round mid-game without losing a single point.
  - **Category table** (Wingspan, Wyrmspan) — end-game scoring games don't have "rounds" so much as a fixed list of categories to total up. Every category is on the table from the start; a small ▲▼ per row moves which one your taps and keypad entries currently target, so you can fill them in whatever order suits how you're tallying the board.
- **Game presets** — Flip 7, Sushi Go!, Star Realms, and Yu-Gi-Oh! each swap in a custom card/counter picker and sensible starting defaults (health totals, countdown targets, the works), so the app already knows the house rules better than half the table. There's also a generic **Life total** mode for anything else that just needs a shared starting number ticking down.
- **Play together, live** — share a 6-character code and everyone at the table (or not at the table — it works over the internet too) sees every tap, rename, and round land on their own phone in real time. No accounts, just a code; leave any time and your game quietly reverts to local-only.
- **Player colours** — everyone gets their own hue from an 8-colour palette, and the current leader's tile gets a properly smug 3D glow.
- **Six themes** — Faelyn, Martha, Blaugen, Tarquin, Sleevey Barry, and Boo-bees, each with its own fonts, palette, and background treatment rather than just a colour swap. Pick your table's mood.
- **A proper finish** — a winner screen with confetti, ranked standings, a "Share results" button, and a "Save as image" button that renders a branded PNG for the group chat.
- **Works offline, saves locally** — your game lives in your browser's storage by default. No account, no server, no one peeking at your suspiciously high Sushi Go! score — unless you turn on Play together for that one game, which only ever shares that one game's code, never anything else on your device.

## Deal it onto your home screen

Scorepad is installable — add it to your phone's home screen and it opens full-screen, no browser chrome, works offline, icon and all. On Android/Chrome/Edge you'll get a native install prompt; on iPhone (Safari doesn't allow that) it'll walk you through the manual **Share → Add to Home Screen** steps instead.

## Under the hood

The whole app is a single `index.html` file — no build step, no bundler, no `node_modules` to summon. HTML, CSS, and JavaScript all live together, with only Google Fonts (and, for sync, the Firebase SDK) loaded externally. A handful of small files (`manifest.json`, `sw.js`, and a few icons) sit alongside it purely to make the "install as an app" trick work.

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

**Scoring model.** Every game is stored as one flat, chronological ledger (`state.rounds`) — score entries and round-boundary markers interleaved in a single array, rather than separate "rounds" and "totals" structures kept in sync by hand. Round-by-round display, running totals, and category tables are all just different ways of reading the same ledger, which is what lets you switch scoring modes mid-game, undo cleanly, and edit any past entry without the rest of the game's math drifting out of sync.

**Live sync.** "Play together" is backed by a Firebase Realtime Database. Players and round entries sync as individually-diffed children rather than one big object overwrite, so two people editing different things at the same moment (everyone tapping their own score at once, say) don't stomp on each other — only a genuine edit to the *same* field ever has to pick a winner. Everything you enter offline still lands in your browser's local storage first; syncing is a mirror on top, not a replacement for it.

**No React, no framework.** Rendering is plain DOM manipulation (`createElement`, template strings, event listeners) — deliberately, to keep the "open one file, read the whole app" property that makes a project like this easy to poke at and extend without a build pipeline getting in the way.

## Got a game to suggest?

Scorepad's preset list is short on purpose — every game gets built properly or not at all. If there's a game you'd love to see with its own keypad and rules baked in, [send a suggestion](mailto:hello@scorepad.uk?subject=Game%20suggestion) — there's a shortcut for it right in the app's menu too.

## The rulebook (license)

Released under the [GNU GPLv3](LICENSE), plus one additional term: fork it, remix it, run your own house rules, deploy it wherever you like — it's free to use. Just keep two things intact:

1. **Keep it open.** Any modified or hosted version must stay GPLv3 too — no closing the source off.
2. **Keep the credit.** Visible attribution to Scorepad.uk and its original author must stay wherever the software or a fork of it is used or deployed — it can't be passed off as someone else's original work.

See the [LICENSE](LICENSE) file for the full legal text.

## Say thanks

If Scorepad's saved you from an argument about whether that was really 47 points in Sushi Go!, consider [buying a coffee on Ko-fi](https://ko-fi.com/faelynaegolor). Entirely optional — much like reading the rulebook before round one.

<div align="center">
  <sub>Built for game night. No dice were harmed in the making of this scoreboard.</sub>
</div>
