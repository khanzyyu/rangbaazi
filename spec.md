# RangBaazi

## Current State
Full-stack betting/casino platform with React frontend. Admin Panel has tabs: Overview, Events, Bets, Transactions, Users, Payments, Website. All 11 casino games exist (Aviator, Slots, Fishing, Mines, Roulette, Plinko, Color Prediction/Win Go, Teen Patti, Andar Bahar, Baccarat, Dragon Tiger). Admin panel is accessible only to user `Khanzyy@`.

## Requested Changes (Diff)

### Add
- New "Games" tab in Admin Panel with manual controls for all casino games:
  - Enable/Disable toggle per game (shows/hides it from users)
  - Set house edge % per game
  - For Win Go (Color Prediction): manually set next round result (Red/Green/Violet/Random)
  - For Aviator: manually set crash point (or Auto)
  - For Mines: no manual result needed (RNG-based)
  - For Slot Machine: toggle enabled/disabled
  - A "Game Control" status card per game showing: game name, enabled status, house edge, any manual override
- `gameSettings` state in BettingContext holding per-game config
- `updateGameSettings` function in context
- Games pages should respect `enabled` flag — if admin disables a game, users see "Game currently unavailable" message

### Modify
- AdminPage.tsx: add "Games" tab with Gamepad icon
- BettingContext.tsx: add `GameSettings` type, `gameSettings` state, `updateGameSettings` function
- Casino/game pages: read `gameSettings` to check if game is enabled before rendering
- Color Prediction game: accept optional `forcedResult` from gameSettings and use it for next round
- Aviator game: accept optional `forcedCrashPoint` from gameSettings

### Remove
- Nothing removed

## Implementation Plan
1. Add `GameSettings` type and state to BettingContext with defaults for all 11 games
2. Add `updateGameSettings` to context and expose it
3. Add Games tab to AdminPage with per-game cards: toggle, house edge input, manual override fields for Aviator and Win Go
4. In CasinoPage/game pages, check `gameSettings[gameId].enabled` before rendering — show "unavailable" if disabled
5. Pass `forcedResult` to ColorPredictionGame and `forcedCrashPoint` to AviatorGame from gameSettings
