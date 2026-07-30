<div align="center">
  <img src="icon512.png" width="96" height="96" alt="Scorepad logo — an amber tally-mark badge">

  # Scorepad<span>.uk</span>

  **A free, offline scorekeeper for board and card games — it does the sums so you don't have to.**

  [![Live site](https://img.shields.io/badge/live-scorepad.uk-f4b942?style=for-the-badge&labelColor=1a1226)](https://scorepad.uk)
  [![License](https://img.shields.io/badge/license-GPL--3.0-f4b942?style=for-the-badge&labelColor=1a1226)](LICENSE)
  [![Build step](https://img.shields.io/badge/build%20step-none-f4b942?style=for-the-badge&labelColor=1a1226)](index.html)
  [![Installable](https://img.shields.io/badge/installable-PWA-f4b942?style=for-the-badge&labelColor=1a1226)](#deal-it-onto-your-home-screen)
</div>

---

No app store, no sign-up, no ads, no dice required. Just open it, name your players, and start keeping score — Scorepad handles the maths while you handle the trash talk.

## What's in the box

- **Two ways to keep score**
  - **Running score** — tap `−`/`+` beside a player's number to nudge it live, or tap the number itself for a full keypad. A little pill pops up to show your last few taps, so a fat-fingered +50 doesn't go unnoticed.
  - **Round-by-round** — enter each player's score for the hand on their own tile (in any order — the tile darkens once it's in, with a green tick to prove it), then hit **Add round** to lock it in. Round history, running totals, and a gold underline on the leader are all tracked in one shared table — and you can switch between the two modes mid-game without losing a single point.
- **Game presets** — Flip 7 and Sushi Go! swap in custom card-picker keypads and sensible defaults, so the app already knows the house rules better than half the table.
- **Player colours** — everyone gets their own hue from an 8-colour palette, and the current leader's tile gets a properly smug 3D glow.
- **Four themes** — Felt & Ivory, Pastel Pop, Neon Arcade, and Ink & Paper, each with its own fonts and palette. Pick your table's mood.
- **A proper finish** — a winner screen with confetti, ranked standings, a "Share results" button, and a "Save as image" button that renders a branded PNG for the group chat.
- **Works offline, saves locally** — your game lives in your browser's storage. No account, no server, no one peeking at your suspiciously high Sushi Go! score.

## Deal it onto your home screen

Scorepad is installable — add it to your phone's home screen and it opens full-screen, no browser chrome, works offline, icon and all. On Android/Chrome/Edge you'll get a native install prompt; on iPhone (Safari doesn't allow that) it'll walk you through the manual **Share → Add to Home Screen** steps instead.

## Under the hood

The whole app is a single `index.html` file — no build step, no bundler, no `node_modules` to summon. HTML, CSS, and JavaScript all live together, with only Google Fonts loaded externally. A handful of small files (`manifest.json`, `sw.js`, and a few icons) sit alongside it purely to make the "install as an app" trick work.

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

## Got a game to suggest?

Scorepad's preset list is short on purpose — every game gets built properly or not at all. If there's a game you'd love to see with its own keypad and rules baked in, [send a suggestion](mailto:hello@scorepad.uk?subject=Game%20suggestion) — there's a shortcut for it right in the app's menu too.

## The rulebook (license)

Released under the [GNU GPLv3](LICENSE). Fork it, remix it, run your own house rules — just keep it open.

## Say thanks

If Scorepad's saved you from an argument about whether that was really 47 points in Sushi Go!, consider [buying a coffee on Ko-fi](https://ko-fi.com/faelynaegolor). Entirely optional — much like reading the rulebook before round one.

<div align="center">
  <sub>Built for game night. No dice were harmed in the making of this scoreboard.</sub>
</div>
