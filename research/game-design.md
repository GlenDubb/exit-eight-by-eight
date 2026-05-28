# Exit Eight by Eight — Game Design Document

## Core Gameplay Loop

- **The Premise:** A psychological horror game in a looping subway corridor inspired by Exit 8.
- **The Twist:** Instead of visual anomalies, math problems are scrawled on the walls.
- **The Rules:** If the math problem is correct, you keep walking forward. If it is wrong, you must turn around and run back to the start of the corridor.

## Level Structure

- Each level contains **4–6 math problems** displayed throughout the corridor.
- Every problem includes its answer on the wall — the player must judge whether each answer is **correct or incorrect**.
- **All answers must be correct** to proceed. If even **one** problem is solved incorrectly, the player needs to back.

## Math Problem Design

- Problems are targeted at a **Year 9** difficulty level.
- Each problem should be solvable in **10–15 seconds** with mental math or quick working.
- Problem categories (TBD — see [Math Problem Types](#math-problem-types)).

## The Scoring System

- **Maximum Score:** 100 points per run.
- **The Flawless Rule:** Any run completed with zero mistakes automatically scores a perfect 100, regardless of how slow the player moves.
- **The "Time Tax":** Making a mistake adds a real-time physical delay (backtracking) plus a static time penalty to the clock. This lowers the final speed score but encourages players to finish "bad games."

## Automated Token Economy & Anti-Exploit

To prevent players from constantly quitting and restarting when they make a mistake, the game uses an automated token system linked to a **Lifetime Average Score**:

- **Speedrun Tokens (The Shield):** These tokens act as a safety net for competitive runs.
- **The Shield Trigger (Bad Run):** If a run score falls 10 points below the player's lifetime average, a token is automatically consumed. The terrible score is discarded, protecting their average.
- **The Fuel Trigger (Good Run):** Scoring above the lifetime average — or hitting a perfect 100 — automatically awards a token.
- **The DNF Penalty:** Force-quitting a match gives the player a single-run score of `Current Average - 5`. This prevents strategic rage-quitting without permanently destroying their career stats.

## Progression & Leaderboards

- **Tiered Leaderboards:** Players are placed in Tiers based on Lifetime Average — Bronze (0–49), Silver (50–69), Gold (70–84), Diamond (85–100). No run-count lock; new players start in Bronze immediately.
- **Tier Mobility:** Players can move up and down Tiers as their average changes. Best times are permanent records stamped with the Tier held when set — dropping a Tier does not erase history.
- **Leaderboard Entry:** Each entry records best Flawless Run time, total completions, Lifetime Average, and Tier at time of record.
- **The Meta-Strategy:** Slow, careful play is good for farming Tokens safely but gives zero leaderboard prestige. To rank high, players must play fast and risk burning Tokens.

## Math Problem Types

> **Status:** In progress. Categories identified, problem bank TBD.
>
> Problems are aligned to **NSW NESA NAPLAN Year 9 numeracy** (non-calculator). Each problem must be verifiable mentally. Most problems are fast (sub-2 sec); some require up to 10–15 seconds.
>
> **TODO:** Deep-dive into ACARA/NAP NAPLAN Year 9 non-calculator past papers to design the problem bank. Key resources:
> - https://www.nap.edu.au/_resources/Example_Test_Numeracy_Y9_non_calc.pdf
> - ACARA past papers (2008–2016) available via acaraweb.blob.core.windows.net
>
> ### Categories
>
> | Category | NAPLAN Strand | Speed Tier | Example |
> |---|---|---|---|
> | Basic arithmetic | Number & Algebra | Fast | `7 × 8 = 56` |
> | Powers & roots | Number & Algebra | Fast | `9² = 81` |
> | Negative numbers | Number & Algebra | Medium | `-4 × -3 = 12` |
> | Fractions & decimals | Number & Algebra | Medium | `3/4 of 60 = 48` |
> | Percentages | Number & Algebra | Medium | `15% of 80 = 12` |
> | Ratio & proportion | Number & Algebra | Medium | `3:5 = 12:20` |
> | Simple algebra | Number & Algebra | Slow | `3x + 7 = 22, x = ?` |
> | Geometry (area, angles) | Measurement & Geometry | Slow | `Area of triangle, b=8, h=9 = ?` |
> | Probability & basic stats | Statistics & Probability | Medium | `Mean of 4, 6, 8 = 5` |
>
> **Note:** Statistics & Probability is limited to problems that work as wall text — probability, mean, median, mode from short lists. No charts or tables.

## Environmental Distractions

> **Status:** To be designed.
>
> Ideas for atmospheric elements that disrupt the player's focus while solving problems.
