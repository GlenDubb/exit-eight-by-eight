# Exit Eight by Eight — Product Requirements Document

## Problem Statement

Year 9 students need engaging, repeatable practice with mental numeracy skills aligned to the NSW NESA NAPLAN curriculum. Traditional worksheet-based practice is dull and offers no intrinsic motivation to replay. Students who struggle with math avoid practice entirely, while high-performing students have no competitive outlet that rewards both speed and accuracy.

Teachers need a tool that students will voluntarily use outside class time, that provides educational feedback, and that requires zero setup — just share a link.

## Solution

Exit Eight by Eight is a browser-based, first-person 3D psychological horror game set in a looping subway maze. Inspired by the indie game Exit 8, players walk through repeating Corridors and judge whether math problems on the walls are correct or incorrect. The horror atmosphere and competitive meta-game (Tokens, Tiers, Leaderboards) drive replayability, while a post-Run Review provides the educational feedback loop.

Players can start immediately as a Guest (zero friction), or sign in with a Google account (including NSW education Google Workspace) to unlock the persistent meta-game: Lifetime Average, Tokens, Tiers, and global Leaderboards.

The game is cheap to host, scales automatically via Firebase, and loads instantly in a browser — no install required.

## User Stories

1. As a **Guest**, I want to play a full Run without signing in, so that I can try the game with zero friction.
2. As a **Guest**, I want to see a Run Review at the end of my Run showing which problems I got wrong and the correct answers, so that I learn from my mistakes.
3. As a **Guest**, I want to be prompted to sign in after completing a Run, so that I know my progress could be saved.
4. As an **Authenticated Player**, I want to sign in with my Google account (including NSW education accounts), so that my progress is saved.
5. As an **Authenticated Player**, I want my Lifetime Average to be calculated and stored server-side across all my counted Runs, so that I have a long-term skill metric.
6. As an **Authenticated Player**, I want to earn a Token automatically when I complete all 8 Levels with a Run Score at or above the Token Threshold, so that I build a safety net for competitive play.
7. As an **Authenticated Player**, I want a Token to be automatically consumed when my Run Score falls below the Mulligan Threshold, so that bad Runs are discarded without me having to decide.
8. As an **Authenticated Player**, I want a Token consumed if I force-quit (DNF) a Run, and a score of 0 recorded if I have no Tokens, so that rage-quitting has a cost.
9. As an **Authenticated Player**, I want to be placed in a Tier (Bronze, Silver, Gold, Diamond) based on my Lifetime Average, so that I compete against players of similar skill.
10. As an **Authenticated Player**, I want to see my Tier change in real time as my Lifetime Average moves, so that I understand the consequences of good and bad Runs.
11. As an **Authenticated Player**, I want my best Flawless Run time to be permanently recorded on the Leaderboard for the Tier I held when I set it, so that my achievements are never erased even if I drop Tiers.
12. As an **Authenticated Player**, I want to view the Leaderboard for each Tier, showing best Flawless Run time, total completions, Lifetime Average, and Tier at time of record, so that I can compare myself to other players.
13. As a **player**, I want to walk through Passive Corridors between Main Corridors, so that the pacing feels atmospheric and builds tension.
14. As a **player**, I want each Main Corridor to contain 6 math problems on the walls that I read while walking, so that I am constantly doing math.
15. As a **player**, I want to turn back at any point in a Main Corridor when I spot an Anomaly (incorrect answer), so that the decision feels urgent and real-time.
16. As a **player**, I want walking through to the end of a Main Corridor to signal "all answers are correct," so that the forward/back mechanic is the only input.
17. As a **player**, I want a wrong decision (walking forward when an Anomaly exists, or turning back when all answers are correct) to Reset me to Level 0, so that the stakes are high and every decision matters.
18. As a **player**, I want to receive no feedback during the Run about which problem I got wrong, so that the horror tension and paranoia are preserved.
19. As a **player**, I want to complete 8 Levels to finish a Run, so that the game has a clear structure and endpoint.
20. As a **player**, I want each Main Corridor to have either 0 or 1 Anomaly (roughly 50% chance), so that the anomaly check is a clean binary decision.
21. As a **player**, I want every problem to have some level of Obstruction (varying from simple placement to flickering lights), so that reading problems is a physical challenge, not just a math challenge.
22. As a **player**, I want Obstructions to be randomised per Corridor but balanced across Corridors and Runs, so that overall difficulty is fair.
23. As a **player**, I want to be able to move freely within a Main Corridor (walk past problems, come back), so that I can manage my time strategically.
24. As a **player**, I want math problems to span 9 NAPLAN-aligned categories at varying speed tiers (Fast, Medium, Slow), so that the game tests a range of numeracy skills.
25. As a **player**, I want most problems to be verifiable in under 2 seconds with occasional problems taking up to 10–15 seconds, so that the pacing is fast with moments of tension.
26. As a **speedrunner**, I want a ~20% random chance per Run of encountering an Unanswered Problem whose solution is 64, triggering the 64-Door shortcut, so that there is a rare, legendary optimisation opportunity.
27. As a **speedrunner**, I want the 64-Door to save approximately 15 seconds when used correctly, so that spotting it is competitively meaningful.
28. As a **speedrunner**, I want the Unanswered Problem to look identical to other problems, so that recognising it requires attention and I cannot easily filter it out.
29. As a **player**, I want my Run Score to combine speed and number of Resets, with speed heavily weighted, so that fast play is rewarded even with mistakes.
30. As a **player**, I want a Flawless Run to score a minimum of 72 (guaranteeing a Token), with speed pushing the score higher toward 100, so that careful play is always viable for Token farming.
31. As a **player**, I want the Run Score floor to be 0, so that catastrophic Runs have maximum impact when Tokens are unavailable.
32. As a **player**, I want a target of ~3:30–4:00 for a perfect Run, so that the game is a quick, repeatable experience.
33. As a **player**, I want to see the Run Review after every completed Run (including Guest Runs), showing all problems, my decisions, and correct answers, so that I always learn.
34. As a **teacher**, I want to share a single URL with my class so that students can start playing immediately without any setup or account creation.

## Implementation Decisions

### Module Architecture

The application is split into seven modules with clear boundaries:

- **Problem Engine** — Generates math problems from 9 NAPLAN-aligned categories. Assigns difficulty tiers (Fast/Medium/Slow). Produces correct and incorrect answers. Selects Obstructions. Assembles a Main Corridor's problem set. Ensures the difficulty budget is balanced across Corridors within a Run. Pure functions, no side effects.

- **Run Engine** — Orchestrates a full Run as a state machine: generates 8 Levels of Corridor sequences (Passive → Passive → Main), decides which Main Corridors have Anomalies (~50%), rolls the 20% chance for an Unanswered Problem / 64-Door, tracks Level progress, handles Resets. Consumes problem sets from the Problem Engine. Pure game logic — no rendering dependency.

- **Scoring Engine** — Computes Run Score from speed and reset count. Enforces the Flawless Run floor (72). Determines Token earn (Token Threshold) and auto-spend (Mulligan Threshold). Handles DNF scoring. Updates Lifetime Average. Computes Tier placement. All tunable constants exposed as configuration for playtesting.

- **3D Renderer (Corridor Scene)** — Three.js / React Three Fiber scene: first-person camera, corridor geometry, wall-mounted problem text, Obstructions (flickering lights, angled placement, timed reveals), the 64-Door. Handles player movement (walk forward, turn back, free movement within corridor). Consumes state from the Run Engine — does not own game logic.

- **Auth & Player Profile** — Firebase Auth with Google OAuth (supports NSW education Google Workspace accounts). Player profile document in Firestore: Lifetime Average, Token count, total completions, current Tier. Guest mode bypasses all persistence.

- **Leaderboard Service** — Reads and writes Firestore Leaderboard entries. Supports per-Tier queries. Records best Flawless Run time stamped with the player's Tier at time of record. Entries are permanent historical records.

- **UI Shell** — React + TailwindCSS. Login/Guest screen, main menu, Run Review screen (post-Run educational feedback), Leaderboard view, player profile and stats display. Wraps the 3D Corridor Scene.

### Tech Stack (ADR-0001)

- **3D Engine**: Three.js via React Three Fiber (R3F) — lightweight, fast-loading, TypeScript-native
- **Frontend**: React + Vite + TypeScript + TailwindCSS
- **Backend**: Firebase (Auth, Firestore, Hosting)
- **Auth**: Firebase Auth with Google OAuth (NSW education Google Workspace compatible)
- **Deployment**: Static site on Firebase Hosting. No server to manage. Scales automatically.

### Key Architectural Decisions

- **The Run Engine and Scoring Engine are pure logic with no rendering or Firebase dependency.** This allows comprehensive testing in isolation and keeps the 3D Renderer as a thin presentation layer.
- **All tunable gameplay constants (Token Threshold floor, Mulligan Threshold, Flawless Run floor score, Anomaly probability, 64-Door probability, scoring formula weights) are exposed as configuration**, not hardcoded. These values are provisional and will be adjusted through playtesting.
- **Guest mode is a client-only mode.** No Firebase reads or writes occur for Guest players. The game runs entirely in-browser. This means Guest Runs have zero backend cost.
- **Leaderboard entries are immutable.** Once a best time is recorded for a Tier, it is never deleted or moved, even if the player drops to a lower Tier. This prevents gaming via tier manipulation.

### Data Model (Firestore)

- **`players/{uid}`** — Lifetime Average, Token count, total completions, current Tier
- **`runs/{uid}/{runId}`** — Run Score, reset count, elapsed time, flawless flag, timestamp (only for counted Runs; mulliganed Runs are not stored)
- **`leaderboard/{tier}/{uid}`** — Best Flawless Run time, total completions, Lifetime Average at time of record, Tier at time of record

### Scoring Formula

The exact Run Score formula is provisional. The known constraints are:

- Run Score is out of 100
- A Flawless Run scores a minimum of 72, with speed pushing it higher toward 100
- Runs with Resets can still score well if the player is fast (e.g., 3 Resets at near-perfect speed ≈ 72)
- Floor is 0
- Speed is heavily weighted relative to reset penalties
- Target perfect Run time is ~3:30–4:00

The formula will be finalized through playtesting.

### Token Economy

- **Earn**: Complete all 8 Levels with Run Score ≥ Token Threshold. Token Threshold = min(Lifetime Average − 10, 70). One Token per qualifying Run.
- **Auto-spend**: Run Score falls below Mulligan Threshold → Token consumed, Run discarded, does not count against Lifetime Average.
- **DNF**: Force-quit costs a Token. Zero Tokens → score of 0 recorded.
- **No manual spending.** The system handles all Token transactions automatically.

## Testing Decisions

### What makes a good test

Tests should verify **external behavior** — given inputs, assert outputs. They should not depend on internal implementation details, private functions, or specific data structures. Tests should be fast, deterministic, and independent of each other.

### Modules to test

- **Problem Engine** — Test that generated problem sets meet the difficulty budget constraints (correct mix of Fast/Medium/Slow), that answers are mathematically correct or incorrect as intended, that Obstructions are assigned with balanced distribution, and that the Unanswered Problem is generated correctly when triggered.

- **Run Engine** — Test the state machine: Level progression, Reset to Level 0 on wrong decisions, Anomaly placement distribution (~50%), 64-Door trigger on 20% roll, correct handling of the Unanswered Problem corridor, and full Run completion after 8 successful Levels.

- **Scoring Engine** — Test the scoring formula against known scenarios: Flawless Run at various speeds (always ≥ 72), Runs with varying reset counts and speeds, Token Threshold calculation at various Lifetime Averages, Mulligan Threshold auto-spend, DNF with and without Tokens, Tier assignment at boundary values, and Lifetime Average updates.

### Prior art

No existing test infrastructure in the codebase. Tests will be established as part of the initial implementation using Vitest (aligned with the Vite toolchain).

## Out of Scope

- **Real-time multiplayer** — The game is single-player only. Leaderboards are the competitive layer.
- **Mobile-native apps** — Browser-only for V1. The game works on mobile browsers but is not optimized for touch controls.
- **Problem bank design** — The specific math problems are not part of this PRD. A separate deep-dive into ACARA/NAP NAPLAN Year 9 non-calculator past papers is required to populate the Problem Engine's content. A TODO exists in `research/game-design.md`.
- **Audio and music** — Atmospheric audio design is deferred. The horror feel in V1 comes from visuals and Obstructions only.
- **Corridor visual design** — Specific 3D assets, textures, and lighting design for the subway corridors are deferred. V1 uses placeholder geometry.
- **Admin/teacher dashboard** — No class management, student tracking, or teacher-facing features in V1.
- **Localization** — English only.
- **Accessibility** — Deferred for V1 but noted as a priority for future iterations (color contrast for math text, screen reader considerations for educational feedback).

## Further Notes

- The `research/game-design.md` file is partially stale and should be replaced by this PRD as the authoritative spec. The research file can be retained for historical context.
- The `CONTEXT.md` glossary is the authoritative source for domain language. All code, tests, and documentation should use the terms defined there.
- Several gameplay constants are explicitly provisional and flagged for playtesting: scoring formula weights, Token Threshold floor (70), Mulligan Threshold, Tier boundaries (Bronze 0–49, Silver 50–69, Gold 70–84, Diamond 85–100), Anomaly probability (~50%), and 64-Door probability (~20%). These should be extracted as tunable configuration from day one.
- The 64-Door is intentionally rare and legendary. Resist the urge to make it more common during development — its rarity is the design.
