# Turn Clock · EDH

A priority-aware chess clock for Commander / EDH, designed for use on a shared phone or tablet at the game table.

---

## The Problem

Standard chess clocks only track whose *turn* it is. In Commander, the more meaningful question is who currently has *priority* — and a player who counters every spell, flashes in multiple permanents each turn, or just tanks every decision off-turn never gets clocked by a simple turn timer. Turn Clock tracks the player who actually has priority at any given moment, making it a much more accurate measure of who is consuming game time.

## How It Works

Turn Clock implements an **action-triggered priority model**:

- By default, the clock runs on the **active player** (the player whose turn it is)
- Any player can tap their card to claim priority — the clock immediately transfers to them
- The **← END TURN** button advances to the next player in clockwise turn order
- A center **PAUSE** button is reachable from any seat
- The **← PASS PRIORITY** button (visible in the center when a non-active player holds priority) returns the clock to the active player

This captures the meaningful time sinks — long contemplation before a counterspell, tanking a response to a combo — without requiring button presses for every silent priority pass.

## Features

- **4-player clockwise layout** — cards are positioned to mirror a physical table, with the top two cards rendered upside-down for the players sitting across from you
- **Tap anywhere on a card** to claim priority — no small button to aim for
- **Configurable time budget** — 5 to 60 minutes per player in 5-minute steps (default: 20 min)
- **Selectable starting player** — choose manually or randomize with ⚄ RANDOM
- **Low-time alerts** — at ≤10% remaining, a two-tone audio chime plays (once per player per game) and the progress bar glows red
- **Storm warning** — if a player holds priority continuously for ≥20% of the configured time budget, a ⚡ STORM badge appears on their card and in the status banner
- **Activity log** — collapsible timestamped record of every priority transfer, turn advancement, elimination, and alert
- **Player elimination** — two-tap confirmation removes a player from the rotation permanently
- **Dark / Light mode** — toggle in the header of both screens
- **2, 3, and 4 player support**

## Usage

Turn Clock is a single self-contained HTML file with no dependencies to install.

1. Download `turn-clock.html`
2. Open it in any modern browser — Chrome, Safari, Firefox, Edge
3. On mobile: tap **Share → Add to Home Screen** to pin it as a full-screen app icon

The first time you open it, an internet connection is needed to load React and the fonts from CDN (~400 KB total). After that, the browser caches everything and the app works fully offline.

### Recommended Setup

Place a tablet in the center of the table. All four players can reach the PAUSE button and see every card. Each player taps their own card to claim priority.

## Design Notes

### Why not track every priority pass?

In a 4-player game, priority passes silently dozens of times per turn with no action taken. Clicking a button for every pass would create more friction than it resolves. Turn Clock tracks *meaningful* priority — moments where a player is actively deciding whether to act — not the mechanical protocol of priority passing around an empty stack.

### Time thresholds are percentage-based

Rather than hardcoding "warn at 5 minutes, critical at 2 minutes," all thresholds scale with the configured time budget:

| Threshold | Effect |
|-----------|--------|
| ≤ 20% remaining | Timer display shifts to amber |
| ≤ 10% remaining | Timer displays in red, progress bar glows, audio chime plays |
| ≥ 20% held continuously | ⚡ STORM badge appears on the priority holder |

This means the alerts are meaningful whether you've set a 5-minute blitz budget or a 45-minute relaxed game.

### Clockwise layout

Players are seated in clockwise turn order around the table. In the 4-player layout:

```
┌─────────────┬─────────────┐
│  Player 1   │  Player 2   │  ← upside-down (facing far side of table)
├─────────────┴─────────────┤
│          PAUSE            │
├─────────────┬─────────────┤
│  Player 4   │  Player 3   │
└─────────────┴─────────────┘
```

Turn order proceeds clockwise: P1 → P2 → P3 → P4 → P1.

## Contributing

Pull requests are welcome. Some directions worth exploring:

- **Sound customization** — volume control, alternate alert sounds, or a mute toggle
- **Persistent settings** — remember player names and time budget between sessions via localStorage
- **Custom player colors** — let players pick their own accent color rather than using the fixed MTG-inspired palette
- **Per-player storm thresholds** — some players naturally play more reactively; a per-player configurable threshold could reduce false positives
- **Spectator mode** — a read-only view at a different URL for streaming or recording purposes

## Tech Stack

- **React 18** (loaded from CDN via UMD bundle)
- **Babel Standalone** (compiles JSX in-browser)
- **Web Audio API** (low-time chime, no audio files)
- **Google Fonts** — Cinzel, Share Tech Mono
- No build step, no npm, no framework CLI — just one HTML file

## License

MIT — see [LICENSE](LICENSE)
