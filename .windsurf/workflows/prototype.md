---
description: Build a throwaway prototype to flesh out a design — either a runnable terminal app for state/business-logic questions, or several radically different UI variations toggleable from one route.
---

# Prototype

A prototype is **throwaway code that answers a question**. The question decides the shape.

## Pick a branch

Identify which question is being answered — from the user's prompt, the surrounding code, or by asking if the user is around:

- **"Does this logic / state model feel right?"** → See Logic Prototype section below. Build a tiny interactive terminal app that pushes the state machine through cases that are hard to reason about on paper.
- **"What should this look like?"** → See UI Prototype section below. Generate several radically different UI variations on a single route, switchable via a URL search param and a floating bottom bar.

The two branches produce very different artifacts — getting this wrong wastes the whole prototype. If the question is genuinely ambiguous and the user isn't reachable, default to whichever branch better matches the surrounding code (a backend module → logic; a page or component → UI) and state the assumption at the top of the prototype.

## Rules that apply to both

1. **Throwaway from day one, and clearly marked as such.** Locate the prototype code close to where it will actually be used. Name it so a casual reader can see it's a prototype, not production.
2. **One command to run.** Whatever the project's existing task runner supports.
3. **No persistence by default.** State lives in memory.
4. **Skip the polish.** No tests, no error handling beyond what makes the prototype _runnable_, no abstractions.
5. **Surface the state.** After every action (logic) or on every variant switch (UI), print or render the full relevant state.
6. **Delete or absorb when done.** When the prototype has answered its question, either delete it or fold the validated decision into the real code.

## When done

The _answer_ is the only thing worth keeping from a prototype. Capture it somewhere durable (commit message, ADR, issue, or a `NOTES.md` next to the prototype) along with the question it was answering.

---

## Logic Prototype

### When this is the right shape

- "I'm not sure if this state machine handles the edge case where X then Y."
- "Does this data model actually let me represent the case where..."
- "I want to feel out what the API should look like before writing it."
- Anything where the user wants to **press buttons and watch state change**.

### Process

1. **State the question** — Write down what state model and what question you're prototyping. One paragraph, in the prototype's README or a comment at the top of the file.

2. **Pick the language** — Use whatever the host project uses.

3. **Isolate the logic in a portable module** — Put the actual logic behind a small, pure interface that could be lifted out and dropped into the real codebase later. The right shape depends on the question:
   - **A pure reducer** — `(state, action) => state`
   - **A state machine** — explicit states and transitions
   - **A small set of pure functions** over a plain data type
   - **A class or module with a clear method surface** when the logic genuinely owns ongoing internal state

4. **Build the smallest TUI that exposes the state** — On every tick, clear the screen and re-render the whole frame. Each frame has:
   - **Current state**, pretty-printed and diff-friendly
   - **Keyboard shortcuts**, listed at the bottom

5. **Make it runnable in one command** — Add a script to the project's existing task runner.

6. **Hand it over** — Give the user the run command. They'll drive it themselves.

7. **Capture the answer** — When the prototype has done its job, record what it taught.

### Anti-patterns

- Don't add tests.
- Don't wire it to the real database.
- Don't generalise.
- Don't blur the logic and the TUI together.
- Don't ship the TUI shell into production.

---

## UI Prototype

### When this is the right shape

- "What should this page look like?"
- "I want to see a few options for this dashboard before committing."
- "Try a different layout for the settings screen."

### Two sub-shapes — strongly prefer sub-shape A

**Sub-shape A — adjustment to an existing page (preferred):** The route already exists. Variants are rendered **on the same route**, gated by a `?variant=` URL search param. The existing data fetching, params, and auth all stay — only the rendering swaps.

**Sub-shape B — a new page (last resort):** Only use this when the thing being prototyped genuinely has no existing page to live inside. Create a **throwaway route** following whatever routing convention the project already uses.

### Process

1. **State the question and pick N** — Default to **3 variants**. More than 5 stops being radically different.

2. **Generate radically different variants** — Variants must be **structurally different** — different layout, different information hierarchy, different primary affordance, not just different colours.

3. **Wire them together** — Create a single switcher component on the route using `?variant=` URL params.

4. **Build the floating switcher** — A small fixed-position bar at the bottom-centre with left/right arrows and variant label. Keyboard: `←` and `→` also cycle. Hidden in production builds.

5. **Hand it over** — Surface the URL (and the `?variant=` keys).

6. **Capture the answer and clean up** — Once a variant has won, fold the winner in and delete the rest.

### Anti-patterns

- Variants that differ only in colour or copy.
- Sharing too much code between variants.
- Wiring variants to real mutations.
- Promoting the prototype directly to production.
