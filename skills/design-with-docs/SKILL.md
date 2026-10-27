---
name: design-with-docs
description: "Design pass between grilling and the spec: grill the module shapes, decide interfaces and seams, and write docs/DESIGN.md and ADRs."
disable-model-invocation: true
---

# Design With Docs

The gap between a grilled idea and a spec is the system's shape: which modules exist, what a caller must know about each, and where behaviour can be swapped. Decide that here, on paper, and the tickets that follow can slice decided interfaces instead of each implementer inventing one. The deliverable is the design, written into the project's documents; implementation comes later.

Run this after `grill-with-docs`, which settles the idea and the vocabulary, and before `to-spec`, whose Implementation Decisions section copies what this pass decides.

## Process

### 1. Read what the project already decided

Read `GLOSSARY.md`, the ADRs that touch this area, and `docs/DESIGN.md` if it exists. Use the glossary's terms from here on.

**Done when:** the vocabulary and the prior decisions that constrain this design are known, and any decision this design would reverse is named rather than silently overridden.

### 2. Load the design vocabulary

Call the Skill tool with `codebase-design` for the vocabulary and principles: **module**, **interface**, **depth**, **seam**, **adapter**, **leverage**, **locality**, and the deletion test.

**Done when:** the terms are in hand and will be used exactly — not "component", "service", or "boundary".

### 3. Grill the module shapes

Call the Skill tool with `grilling`, aimed at the design rather than the feature. For each module the effort introduces or changes, walk the same tree:

- What must a caller know to use it correctly — invariants, error modes, ordering, required configuration, performance — beyond the type signature?
- What sits behind the seam, and what does the caller stop having to know because of it?
- What actually varies across the seam? One adapter is a hypothetical seam; two is a real one.
- Does it survive the deletion test: would deleting it concentrate the complexity, or just move it?

**Done when:** every module in scope has a named interface and a named seam, and its depth claim survives the deletion test. A module whose only justification is "it might vary" is dropped.

### 4. Explore the load-bearing interface

For the one or two modules the effort rests on, call `codebase-design` and run its design-it-twice pattern: parallel sub-agents, radically different interfaces, then compare on depth, locality and seam placement.

**Done when:** either two designed interfaces have been compared with a stated choice, or the interface is genuinely uncontested and that is stated in one line.

### 5. Write the design down

- **`docs/SPEC.md`** — create if missing. This is a project-wise, non-feature specific specification that captures the project's purpose, goals, constraints, core principles, architecture overview, and high-level requirements that apply across the whole project. Keep it stable and not tied to a single feature or effort.
- **`docs/DESIGN.md`** — create the skeleton if it is missing, then fill the sections this effort touches: Context, Module map (Mermaid), Modules and interfaces, Seams and adapters, Data flow (Mermaid), Limitations, References. One section per module: its interface, what is behind the seam, and the trade-off that placed the seam there.
- **`docs/adr/NNNN-<slug>.md`** — one ADR per decision that is hard to reverse and not obvious from the code: a seam that must not move, an interface shape a later change would break, a boundary the effort deliberately keeps. Call the Skill tool with `domain-modeling` for the ADR format. A decision that is cheap to reverse stays in `DESIGN.md` prose.
- **`GLOSSARY.md`** — every domain concept this design names, with its meaning and what it avoids, added through `domain-modeling` as it crystallises.

**Done when:** `docs/SPEC.md` exists with project-wise, non-feature specific content; `DESIGN.md` carries the module map and every module in scope has its interface written down; every hard-to-reverse decision has an ADR; every concept the design uses is in the glossary.

### 6. Hand the design to the spec

The spec's Implementation Decisions section copies the decided interfaces — modules, interfaces, seams, contracts — without file paths or code snippets, and names the ADRs it depends on. `to-spec` is synthesis, so everything it needs must already be written here.

**Done when:** nothing in the design is left implicit for an implementer to invent, and `/to-spec` can be run without design questions coming back.

## Boundaries

- **Shapes now, code later.** This pass ends with documents; implementation belongs to `implement`.
- **ADRs are settled ground.** When the design needs to move one, surface it and ask before writing the new ADR.
- **The effort's scope is the design's scope.** Modules outside it stay as they are, even when they look shallow; `improve-codebase-architecture` is the skill for surveying those.

## When there is no code yet

The survey skills need a codebase to scan, so on a greenfield project this pass is the design step. Project the module map from the spec's user stories, design the interfaces the first slice will need, and mark the rest `TBD` in the skeleton. Once the first code exists, run `improve-codebase-architecture` to check the map against reality and fold what it finds back in.
