# Sandy Plains Softball Lineup Generator

A single-file web app for youth softball coaches to instantly generate a batting lineup and 5-inning fielding rotation for their team.

## Features

- **Enter players one by one**, each with a name, jersey number, and optional ranked position preferences
- **Save/load your roster** to the browser's local storage so you don't have to re-enter it every game
- **Auto-generates** a randomized batting order and fielding grid that satisfies all rotation rules and honors position preferences where possible
- **Persistent Game Info** — date, team names, and field stay visible and editable on both the input and results screens, so a typo doesn't force a regenerate
- **Click-to-swap** any two players in either the batting lineup or the fielding grid
- **Rule validation** with inline error messages — printing is blocked until all violations are resolved
- **PDF export** opens in a new browser tab for printing or saving, with no browser headers or footers
- **Responsive design** — works on desktop, tablet, and mobile
- No installation, no backend, no dependencies to install — just open the HTML file

---

## How to Use

### 1. Enter Game Info
Fill in the game date, home team name, away team name, and the field name (e.g., *Sandy Plains Field 3*). This card stays visible after you generate a lineup, so you can fix any of these fields at any time without needing to regenerate.

### 2. Add Your Roster
Enter each player's name, jersey number, and (optionally) their ranked position preferences using shorthand, highest preference first:

```
C, 1B, OF
```

| Shorthand | Position |
|---|---|
| C | Catcher |
| P | Pitcher |
| 1B | 1st Base |
| 2B | 2nd Base |
| 3B | 3rd Base |
| SS | Short Stop |
| OF | Any outfield spot |

Use **+ Add Player** to add rows (7–12 players supported; the page starts with 11 rows since that's the most common roster size).

**Save Roster** stores the current roster (names, jerseys, and preferences) in the browser's local storage, overwriting anything saved previously. **Load Saved Roster** appears automatically whenever a saved roster exists and repopulates the rows from it.

### 3. Generate the Lineup
Click **Generate Lineup** to randomly produce:
- A **batting order** (all players, no duplicates)
- A **fielding rotation** across 5 innings that enforces all rules below, giving priority to each player's ranked position preferences

### 4. Adjust if Needed
Click any two rows in the batting lineup to swap them. Click any two cells in the fielding grid to swap those players. Rule violations are flagged in real time.

### 5. Print or Save
Click **Print / Save PDF** to open a clean PDF in a new browser tab. From there, use the browser's native PDF viewer to print or download.

---

## Fielding Rules

The generator enforces the following rules automatically. Manual swaps are validated against the same rules in real time.

| # | Rule |
|---|------|
| 1 | Every position must be filled every inning by a distinct player |
| 2 | Players are placed in their preferred positions where possible, honoring rank order (no player has a bench preference — that's randomized) |
| 3 | A player cannot sit any Bench slot more than once, in aggregate, per game |
| 4 | A player can pitch at most 2 innings, and if 2, they must be consecutive |

11-player rosters include one **Bench** row; 12-player rosters include two (**Bench 1** and **Bench 2**). With 10 or fewer players, the number of named positions always equals the roster size, so every player fields a position every inning and there is no bench.

---

## Fielding Positions by Player Count

| Players | Positions |
|---------|-----------|
| 12 | Catcher, Pitcher, 1st Base, 2nd Base, 3rd Base, Short Stop, Left Field, Left Center Field, Right Center Field, Right Field, Bench 1, Bench 2 |
| 11 | Catcher, Pitcher, 1st Base, 2nd Base, 3rd Base, Short Stop, Left Field, Left Center Field, Right Center Field, Right Field, Bench |
| 10 | Catcher, Pitcher, 1st Base, 2nd Base, 3rd Base, Short Stop, Left Field, Left Center Field, Right Center Field, Right Field |
| 9 | Catcher, Pitcher, 1st Base, 2nd Base, 3rd Base, Short Stop, Left Field, Center Field, Right Field |
| 8 | Catcher, Pitcher, 1st Base, 2nd Base, 3rd Base, Short Stop, Left Field, Right Field |
| 7 | Catcher, Pitcher, 1st Base, 2nd Base, 3rd Base, Short Stop, Outfield |

---

## Running Locally

No build step or server required. Just open the file in any modern browser:

```
open index.html
```

Or double-click the file in your file explorer.

---

## Technical Notes

- **Single file** — all HTML, CSS, and JavaScript is self-contained in `index.html`, including the header icon (embedded as a base64 PNG)
- **Fonts** loaded from Google Fonts (requires internet connection)
- **PDF generation** uses [jsPDF](https://github.com/parallax/jsPDF) and [jsPDF-AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable) loaded from CDN (requires internet connection)
- **Roster storage** uses the browser's `localStorage` under the key `sandyPlainsSavedRoster` — it's local to that browser and device, not synced anywhere
- The fielding grid is generated per inning: bench slot(s) are scheduled first (5 × number-of-bench-slots distinct players across the game, each benched at most once), then the pitcher is scheduled in contiguous 1–2 inning blocks (preferring players who ranked Pitcher highly), then the remaining slots are filled by greedily matching each remaining player's ranked position preferences to an open slot, with any leftovers assigned at random. A cyclic-shift fallback guarantees a valid grid even if no preference-satisfying assignment is found within the attempt limit.
