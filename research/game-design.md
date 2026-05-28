# Exit Eight by Eight — Game Design Document

## Core Gameplay Loop

- **The Premise:** A psychological horror game in a looping subway corridor inspired by Exit 8.
- **The Twist:** Instead of visual anomalies, math problems are scrawled on the walls.
- **The Rules:** If the math problem is correct, you keep walking forward. If it is wrong, you must turn around and run back to the start of the corridor.

## Level Structure

- Each Level is a sequence of Passive Corridors followed by a Main Corridor.
- Each Main Corridor contains **6 math problems** displayed on the walls.
- Every problem includes its answer — the player must judge whether each answer is correct.
- A Main Corridor has 0 or 1 **Anomaly** (incorrect answer). ~50% of Main Corridors contain one.
- If an Anomaly is present, the player must turn back. Walking forward is a wrong decision.
- If all answers are correct, the player must walk forward. Turning back is a wrong decision.
- A wrong decision triggers a **Reset** to Level 0.
- A Run has **8 Levels**. Completing all 8 ends the Run.

## Math Problem Design

- Problems are targeted at a **Year 9** difficulty level.
- Each problem should be solvable in **10–15 seconds** with mental math or quick working.
- Problem categories (TBD — see [Math Problem Types](#math-problem-types)).

## The Scoring System

- **Maximum Score:** 100 per Run.
- **Run Score:** Combines speed and number of Resets, with speed heavily weighted.
- **Flawless Run:** A Run with zero Resets scores a minimum of 72, with speed pushing it toward 100.
- **Runs with Resets** can still score well if the player is fast (e.g., 3 Resets at near-perfect speed ≈ 72).
- **Floor:** 0. Exact formula is provisional and subject to playtesting.

## Automated Token Economy & Anti-Exploit

To prevent players from constantly quitting and restarting when they make a mistake, the game uses an automated Token system linked to a **Lifetime Average**:

- **Token Earn:** Completing all 8 Levels with a Run Score at or above the Token Threshold (lower of Lifetime Average minus 10, or 70) earns one Token.
- **Token Auto-Spend (Bad Run):** If a Run Score falls below the Mulligan Threshold, a Token is automatically consumed. The bad Run is discarded and does not count.
- **DNF Penalty:** Force-quitting mid-Run consumes a Token if available (no score recorded). If zero Tokens, a score of 0 is recorded against the Lifetime Average.
- Tokens are never manually spent — the system handles everything automatically.

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

## Environmental Obstructions

> **Status:** Defined. See CONTEXT.md for the Obstruction term.

- **Obstructions** are environmental effects applied to every problem at varying intensity.
- Types include: flickering lights, problems at odd angles (requires panning), timed reveals (screen cycles on/off).
- Randomised per Corridor but balanced for fairness across Runs.
- Obstructions add time pressure without increasing math difficulty.
