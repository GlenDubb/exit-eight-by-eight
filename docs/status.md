# Project Status

## Current Phase: Pre-implementation

All design work is complete. 19 GitHub issues created covering the full implementation backlog.

## Issue Tracker

https://github.com/GlenDubb/exit-eight-by-eight/issues

## Completed

- [x] Core game design (research/game-design.md)
- [x] Domain glossary (CONTEXT.md — 22 terms)
- [x] Tech stack ADR (docs/adr/0001)
- [x] Product Requirements Document (docs/prd.md)
- [x] Git repository initialized and pushed
- [x] 19 GitHub issues created with acceptance criteria and dependencies
- [x] NAPLAN deep-dive grilling session — architecture decisions resolved

## In Progress

Nothing — ready for implementation.

## Ready to Start (no blockers)

- **#1** — Project scaffolding
- **#17** — NAPLAN Question Template Research (HITL)

## Key Design Decisions

| Decision | Resolution | Documented in |
|---|---|---|
| Tech stack | Three.js (R3F) + Firebase | docs/adr/0001 |
| Scoring formula | Speed + resets, Flawless floor 72 | CONTEXT.md, PRD |
| Token economy | Auto earn/spend, no manual intervention | CONTEXT.md, PRD |
| Leaderboards | Tiered by Lifetime Average, no run lock | CONTEXT.md, PRD |
| Problem architecture | Question Templates → static Question Bank | CONTEXT.md, issue #17/#18 |
| Wrong answers | Misconception-based per Template | Issue #17 |
| Wall display | Text-only V1, diagrams deferred (#19) | Issue #17, #19 |

## Notes

- Issue #5 is a duplicate — close it manually
- `scripts/create-issues.ps1` and `docs/issues-to-create.md` are temp files from issue creation — safe to delete
