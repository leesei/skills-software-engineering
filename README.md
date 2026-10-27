# skills-software-engineering

Skills that sit around the [mattpocock/skills](https://github.com/mattpocock/skills) engineering flow:

- `doc-layout` says where documents live
- `design-with-docs` fills the design documents before implementation starts

## The skills

- **`doc-layout`** _(model-invoked)_ — the default home for every project document (README, GLOSSARY, spec, ADR, design doc, tickets, agent scratch), who each one faces, which skill writes it, and the rules that keep one fact in one place. Other skills reach it when they need to know where an artifact belongs; `/setup-matt-pocock-skills` writes the per-repo instance it reads.
- **`design-with-docs`** _(user-invoked)_ — the design pass between `grill-with-docs` and `to-spec`: grills the module shapes, decides interfaces and seams with the `codebase-design` vocabulary, and writes `docs/DESIGN.md` and the ADRs. The spec that follows copies the decided interfaces, so tickets slice shapes instead of inventing them.

## The flow

```
grill-with-docs  →  design-with-docs  →  to-spec  →  to-tickets  →  implement
   idea + terms        shapes + docs      the spec     the slices     the code
```

## Install

**skills.sh:**

```sh
npx skills add leesei/skills-software-engineering
```

**Manual:** copy `skills/<name>/` into your agent's skills directory (`~/.agents/skills/` for the layout `mattpocock/skills` uses).

## Requires

- `doc-layout` works alongside `setup-matt-pocock-skills`; run it once per repo so the project's layout instance exists
- `design-with-docs` calls three skills from `mattpocock/skills`: `grilling`, `codebase-design`, and `domain-modeling`

## Motivation

This section and the associated skills were created to solve a recurring problem: **architecture decisions and document layouts drift across projects when each session re-derives the rules from scratch.**

**The problem:** Earlier attempts generalized an ADR to make it applicable to other projects, but putting the rule in the wrong layer caused two issues:

1. **Prompts are ephemeral** — a prompt added to a session cannot constrain skills that never invoke the originating command (e.g., `code-review` searches for a spec, `improve-codebase-architecture` reads `GLOSSARY.md` and `docs/adr/`).
2. **No single home** — without a canonical reference, every skill or flow that needs the layout must re-state it, leading to drift and duplication.

**The three-layer solution**:

1. **`doc-layout`** — the model-invoked reference skill (`~/.agents/skills/doc-layout/SKILL.md`). The single home of the rule: which document faces whom, which artifact each skill writes, the skeleton-then-fill rule, `.scratch/` as the agent scratch root. Every writing or exploring skill consults it.
2. **`/setup-matt-pocock-skills`** — the installer. Its Section C owns "Domain docs" and writes the per-repo instance (`docs/agents/doc-layout.md`) plus the `## Agent skills` block in `AGENTS.md`. One run per repo makes the layout applicable without re-derivation.
3. **The project ADR** — an adoption record. The project-specific ADR shrinks to: "we adopt the doc-layout convention; project-specific additions are these" and cites the skill/doc. ADRs are per-project history; a rule that has to be re-derived in every repo will drift — and yours already has.

**Why this matters:** Having the layout known _before_ `/to-spec` writes (step 1) means the target paths are already decided. The design pass (`design-with-docs` → `grilling` → `codebase-design`) decides interfaces and seams _before_ implementation, so tickets slice shapes instead of inventing them. The "skeleton first, filled after implementation" rule ensures documents have a structure from day one.

**The cost of the wrong approach:** A prompt copied into each session has no single home and no leverage. The ecosystem already uses the "vocabulary underneath" pattern (`codebase-design`, `domain-modeling`) where the skill reads the in-repo document, not the other way around.

## Why these live here

`mattpocock/skills` is curated and closes new-skill contributions (`SCOPE.md`: "New skills: proposals and contributions are closed"). These are local additions, kept in the same shape so they install the same way.
