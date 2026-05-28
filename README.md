# Exit Eight by Eight

A psychological horror math game set in a looping subway maze. Players navigate corridors, judge whether math problems on the walls are correct, and race the clock.

## Quick Start

```bash
npm install
npm run dev
```

Open http://localhost:5173

## Project Structure

- `src/` — Source code
  - `engines/` — Core game logic (Problem, Run, Scoring engines)
  - `components/` — React components and 3D scenes
  - `services/` — Firebase auth and leaderboard services
- `docs/` — Documentation
  - `prd.md` — Product Requirements Document
  - `adr/` — Architectural Decision Records
- `CONTEXT.md` — Domain glossary and language reference

## Tech Stack

- **Frontend**: React + Vite + TypeScript + TailwindCSS
- **3D**: Three.js via React Three Fiber
- **Backend**: Firebase (Auth, Firestore, Hosting)
- **Testing**: Vitest

## Game Concept

Inspired by Exit 8, players walk through repeating subway corridors containing math problems. Turn back if you spot an incorrect answer (Anomaly), walk forward if all answers are correct. Wrong decisions reset you to Level 0. Complete 8 Levels to finish a Run.

The horror atmosphere and competitive meta-game (Tokens, Tiers, Leaderboards) drive replayability, while post-Run educational feedback supports learning.

## Documentation

- See `docs/prd.md` for the full Product Requirements Document
- See `CONTEXT.md` for domain terminology
- Math problems are aligned to NSW NESA NAPLAN Year 9 numeracy standards
