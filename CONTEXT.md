# Exit Eight by Eight

A psychological horror math game set in a looping subway maze. Players navigate corridors, judge whether math problems on the walls are correct, and race the clock.

## Language

**Run**:
A single complete attempt at the full game, from the first corridor to the last. One Run produces a Run Score.
_Avoid_: Game, session, match

**Run Score**:
A score out of 100 produced at the end of a completed Run. Combines speed and number of resets, with speed heavily weighted. A Flawless Run scores a minimum of 72, with speed pushing it higher toward 100. Runs with resets can still score well if the player is fast enough (e.g., 3 resets at near-perfect speed can still reach 72). Floor is 0. Exact formula is provisional and subject to playtesting.
_Avoid_: Points, grade, result

**Flawless Run**:
A Run completed with zero resets — every Main Corridor decision was correct on the first attempt. Scores a minimum of 72 (guaranteeing a Token), with speed pushing the score higher. A slow Flawless Run is the grind path for Tokens; a fast Flawless Run is how you climb the leaderboard.
_Avoid_: Perfect run, clean run

**Corridor**:
A short subway passageway the player walks through. Corridors are visually near-identical to create psychological tension through repetition.
_Avoid_: Room, hallway, level

**Passive Corridor**:
A Corridor with no math problems and no decision point. The player walks through it as connective tissue between Main Corridors.
_Avoid_: Empty corridor, filler

**Main Corridor**:
A Corridor containing 6 math problems on the walls (all answered; rarely, one corridor per Run may also contain an Unanswered Problem). Target time is ~25 seconds. The player reads problems while walking and can turn back at any point upon spotting an Anomaly. Walking through to the end signals "all correct."
_Avoid_: Active corridor, puzzle corridor

**Anomaly**:
A math problem on the wall whose displayed answer is incorrect. A Main Corridor has either 0 or 1 Anomaly — never more. Roughly 50% of Main Corridors contain an Anomaly. If an Anomaly is present, the player must turn back. The player receives no feedback during the Run about which problem was wrong.
_Avoid_: Error, bug, mistake

**Unanswered Problem**:
A math equation displayed on the wall without an answer. Has a 20% random chance of appearing in a Run — when it does, it appears in exactly one Main Corridor. A medium-difficulty problem (~3 sec to solve). It is never an Anomaly (no answer to be wrong). The sole trigger for the 64-Door: if the solution is 64, the door is a shortcut saving ~15 seconds. Looks identical to other problems, so players instinctively try to solve it even when they intend to ignore it. A legendary event for speedrunners; most casual players won't encounter it often enough to learn to exploit it.
_Avoid_: Blank problem, missing answer

**Obstruction**:
An environmental effect applied to every problem at varying intensity. Ranges from simple placement differences (problem at an odd angle, requires panning) to timed reveals (flickering light, screen that cycles on/off). Randomised per Corridor but balanced so that overall difficulty is fair and consistent across Corridors and Runs. Obstructions add time pressure without increasing math difficulty.
_Avoid_: Distraction, hazard, trap

**The 64-Door**:
A special door in the Main Corridor that provides a faster route to the next Level — the player doesn't have to walk as far forward or backward. Only triggered by an Unanswered Problem whose solution is 64. Appears at most once per Run with a ~20% chance. Named after 8 × 8 = 64. A rare, high-value shortcut akin to a Trackmania trick jump — most runs have no 64-Door opportunity at all.
_Avoid_: Secret door, bonus door, skip door

**Reset**:
Returning the player to Level 0 after a wrong decision — either walking forward when an Anomaly was present, or turning back when all answers were correct. The player is not told what they got wrong during the Run.
_Avoid_: Fail, death, game over

**Level**:
One successful pass through a Main Corridor. The player advances one Level by making the correct forward-or-back decision, or by using a valid 64-Door. A Run has 8 Levels.
_Avoid_: Stage, round, wave

**Run Review**:
A post-Run summary shown at the end of a completed Run. Shows the player which problems they got wrong and the correct answers. This is the educational feedback loop — learning happens after the Run, not during it.
_Avoid_: Results screen, report card, debrief

**Lifetime Average**:
A player's rolling average score across all counted Runs, stored on the server. The primary long-term stat that determines Token earnings.
_Avoid_: Career score, global average, rating

**Token**:
A mulligan earned by completing all 8 Levels with a score at or above the Token Threshold. Automatically consumed when a completed Run's score falls below the Mulligan Threshold — the bad Run is discarded and does not count against the Lifetime Average. Also consumed on a DNF. One Token earned per qualifying Run. The player never manually chooses to spend a Token; the system handles it.
_Avoid_: Shield, credit, coin

**Mulligan Threshold**:
The Run Score below which a Token is automatically consumed to discard the Run. May be a global fixed value or vary by Tier. Exact values are provisional and subject to playtesting.
_Avoid_: Auto-spend threshold, burn threshold

**DNF**:
A force-quit mid-Run. If the player has Tokens, one is consumed and no score is recorded. If the player has zero Tokens, the quit records a score of 0 against their Lifetime Average.
_Avoid_: Abandon, forfeit, disconnect

**Token Threshold**:
The minimum Run score needed to earn a Token. Calculated as the lower of (Lifetime Average minus 10) or 70. The floor of 70 prevents elite players from being penalised for having a high average. These values are provisional and subject to playtesting.
_Avoid_: Token floor, earn threshold

**Tier**:
A leaderboard bracket determined by the player's Lifetime Average. Players can move up and down between Tiers as their average changes. Tiers are: Bronze (0–49), Silver (50–69), Gold (70–84), Diamond (85–100). Exact thresholds are provisional.
_Avoid_: Rank, division, league

**Leaderboard**:
A per-Tier ranking of players. Each entry records the player's best Flawless Run time, total completions, Lifetime Average, and the Tier they held when the time was set. Players can drop Tiers, but their best time entry remains on the Tier where it was achieved — it is a permanent historical record. There is no run-count lock; new players start in Bronze immediately.
_Avoid_: Scoreboard, rankings, ladder

**Guest**:
An unauthenticated player. Can play unlimited Runs and see the Run Review (educational feedback). Nothing persists between sessions — no Lifetime Average, no Tokens, no Leaderboard entry. Zero friction to start playing.
_Avoid_: Anonymous user, visitor, demo player

**Authenticated Player**:
A player signed in with a Google account (including NSW education Google Workspace accounts). Gains access to the meta-game: Lifetime Average, Tokens, Tiers, and Leaderboard. All Run data is persisted server-side.
_Avoid_: Registered user, member, logged-in user

## Example dialogue

> **Dev**: "The player did a Run but kept failing on the third Main Corridor."
> **Designer**: "Was there an Anomaly they missed, or did they take the 64-Door on a bad answer?"
> **Dev**: "They spotted the Anomaly and turned back correctly, but then on the next pass they walked forward when the Unanswered Problem actually solved to 64 — they missed the shortcut."
> **Designer**: "That's fine, missing the 64-Door isn't a failure. They just lost time. Their Run still continues."
> **Dev**: "They finished the Run but the score was terrible — way below their Lifetime Average."
> **Designer**: "Do they have a Token? They can spend one to mulligan that Run so it doesn't tank their average."
