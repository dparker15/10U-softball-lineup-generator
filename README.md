# Sandy Plains Softball Lineup Generator

A single-file web app for youth softball coaches to instantly generate a batting lineup and 3-inning fielding rotation for their team. The physical game may run longer, but most games at this level don't reliably go past the 3rd inning, so the generator covers just the first 3 innings — the coach can adjust fielding manually on the fly for anything beyond that.

## Features

- **Enter players one by one**, each with a name, jersey number, and optional ranked position preferences
- **Save/load your roster** to the browser's local storage so you don't have to re-enter it every game
- **Auto-generates** a randomized batting order and fielding grid that satisfies all rotation rules and honors position preferences where possible
- **Persistent Game Info** — date, team names, and field stay visible and editable on both the input and results screens, so a typo doesn't force a regenerate
- **Click-to-swap** any two players in either the batting lineup or the fielding grid
- **Rule validation** with inline error messages — printing is blocked until all violations are resolved
- **PDF export** opens in a new browser tab for printing or saving, with no browser headers or footers — prints two identical copies (2-up) on one letter sheet, each with the batting lineup and fielding grid side by side
- **Last updated date and What's New** in the page footer, showing when the site last changed and what's different
- **Responsive design** — works on desktop, tablet, and mobile
- No installation, no backend, no dependencies to install — just open the HTML file

---

## How to Use

### 1. Enter Game Info
Fill in the game date, home team name, away team name, and the field name (e.g., *Sandy Plains Field 3*). This card stays visible after you generate a lineup, so you can fix any of these fields at any time without needing to regenerate.

### 2. Add Your Roster
Enter each player's name, jersey number, and (optionally) their ranked position preferences using shorthand, highest preference first. Tap or hover the **(i)** next to the Position Prefs heading for a shorthand reminder:

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
- A **fielding rotation** across 3 innings that enforces all rules below, giving priority to each player's ranked position preferences

### 4. Adjust if Needed
Click any two rows in the batting lineup to swap them. Click any two cells in the fielding grid to swap those players. Rule violations are flagged in real time.

### 5. Print or Save
Click **Print / Save PDF** to open a clean PDF in a new browser tab. From there, use the browser's native PDF viewer to print or download.

The PDF is a single 8.5" × 11" sheet with two identical copies, one on the top half and one on the bottom half, separated by a dashed cut line. Each copy shows the matchup, date, and field at the top, with the batting lineup on the left and the fielding grid on the right.

---

## Fielding Rules

The generator enforces the following rules automatically. Manual swaps are validated against the same rules in real time.

| # | Rule |
|---|------|
| 1 | Every position must be filled every inning by a distinct player |
| 2 | Players are placed in their preferred positions where possible, honoring rank order (no player has a bench preference — that's randomized) |
| 3 | A player cannot sit any Bench slot more than once, in aggregate, per game |
| 4 | A player can pitch at most 2 of the 3 innings (not required to be consecutive) |

11-player rosters include one **Bench** row; 12-player rosters include two (**Bench 1** and **Bench 2**). With 10 or fewer players, the number of named positions always equals the roster size, so every player fields a position every inning and there is no bench.

---

## Fielding Positions by Player Count

| Players | Positions |
|---------|-----------|
| 12 | Catcher, Pitcher, 1st Base, 2nd Base, Short Stop, 3rd Base, Left Field, Left Center Field, Right Center Field, Right Field, Bench 1, Bench 2 |
| 11 | Catcher, Pitcher, 1st Base, 2nd Base, Short Stop, 3rd Base, Left Field, Left Center Field, Right Center Field, Right Field, Bench |
| 10 | Catcher, Pitcher, 1st Base, 2nd Base, Short Stop, 3rd Base, Left Field, Left Center Field, Right Center Field, Right Field |
| 9 | Catcher, Pitcher, 1st Base, 2nd Base, Short Stop, 3rd Base, Left Field, Center Field, Right Field |
| 8 | Catcher, Pitcher, 1st Base, 2nd Base, Short Stop, 3rd Base, Left Field, Right Field |
| 7 | Catcher, Pitcher, 1st Base, 2nd Base, Short Stop, 3rd Base, Outfield |

---
