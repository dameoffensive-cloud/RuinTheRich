# RUIN THE RICH — streaks & sharing

## What a share looks like

Winning a daily and hitting "copy bragging rights" produces:

```
RUIN THE RICH #37 👯PEANUCKLE
💀💀💀💀 in 61 rounds
order of ruin: 💧→🪙→🍌→🤖
🔥 12
https://dameoffensive.github.io/ruin-the-rich/
```

Line by line:
- **#37** — puzzle number (increments daily; #1 was July 21, 2026). The 👯 tag
  appears only on crew runs, so group chats know which market it was.
- **rounds** — the score. Lower = better. This is the number people argue about.
- **order of ruin** — which billionaire died first (🪙 Chadwick, 🍌 Baron,
  💧 Grimble, 🤖 Ivana). This is the strategy fingerprint — two players with
  the same rounds but different orders played different games.
- **🔥 12** — current streak. Only shown at 2+, so a first win stays humble.
- **URL** — how new players find the game. Set in `index.html` → `GAME_URL`.

## Streak rules

- Win any daily (solo or crew) → that calendar day counts. Multiple wins in
  one day count once.
- Consecutive days grow the streak; missing a day resets it to 1 on the next win.
- Days flip at **UTC midnight** (same moment the puzzle changes, worldwide).
- Best-ever streak is remembered and shown on the win screen when your current
  streak is below it.
- Shown in-game next to the 🏆 prestige badges as 🔥N (at 2+).
- Stored in the browser (`localStorage`, key `rtr-streak`). Clearing site data
  or switching devices resets it — there are no accounts. "New run" does NOT
  clear streaks; it only wipes the current market.

## Link previews (OG tags)

`index.html` contains Open Graph + Twitter card tags so the URL unfurls with a
title, description, and image in Discord, iMessage, Slack, and X.

**One manual step:** the tags point to `og.png` (1200x630). Convert the
included `og-card.svg` to PNG (any svg-to-png site, export at 1200x630) and
upload it to the repo root as `og.png`. Without it, previews still show title
and description, just no image.

If the game URL ever changes, update it in three places: `GAME_URL` in
`index.html`, and the two `og:url` / image meta tags in the same file.

## Tuning ideas (not built yet)

- Weekly mutator days (two Chadwicks on Fridays) for fresh share screenshots.
- Golden top hat doodle cosmetic for Ko-fi supporters.
- Streak-loss protection ("the doodles forgive one missed day") if players
  complain — classic retention lever, currently strict on purpose.
