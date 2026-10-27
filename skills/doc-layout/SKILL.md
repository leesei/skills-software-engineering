---
name: doc-layout
description: Where a project's documents live, who each one faces, and which skill writes it. Use when creating, placing, or reading a project document (README, GLOSSARY, spec, ADR, design doc, deep dive, ticket), when scaffolding a repo's docs, or when a skill needs to know where its output belongs.
---

# Doc Layout

One fact has one home, and each home faces one audience. A document that repeats another's fact goes stale; a fact with no home is re-derived differently on every run.

This is the **default** layout and the rules that keep it coherent. It is not the project's instance: `/setup-matt-pocock-skills` writes that instance to `docs/agents/doc-layout.md`, and its choices win wherever they differ.

## The homes

| Home                      | Audience                                 | Holds                                                                                              | Written by                                  |
| ------------------------- | ---------------------------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| `README.md`               | everyone — end user, operator, developer | what the project is, requirements, install, the commands, an index of the rest                     | anyone, skeleton first                      |
| `GLOSSARY.md`             | everyone                                 | the project's vocabulary: one entry per term, what it means, what it avoids                        | `domain-modeling`                           |
| `docs/SPEC.md`            | operator, developer                      | what was decided to build — the artifact of `/to-spec`                                             | `/to-spec`                                  |
| `docs/adr/NNNN-<slug>.md` | operator, developer                      | a hard-to-reverse decision and why                                                                 | `domain-modeling`                           |
| `docs/DESIGN.md`          | operator, developer                      | how it works: module map, interfaces, seams, data flow, limitations, references                    | `/design-with-docs`, filled as code lands   |
| `docs/<DEEP-DIVE>.md`     | operator, developer                      | one protocol or subsystem walked end to end, cited to primary sources                              | `research` or by hand; `DESIGN.md` links it |
| `docs/agents/*.md`        | agents                                   | the per-repo configuration the skills read: issue tracker, triage labels, domain docs, this layout | `/setup-matt-pocock-skills`                 |
| `.scratch/`               | agents                                   | tickets, in-flight specs, temp files; agent-owned and disposable                                   | `/to-tickets` and any agent                 |

`docs/DESIGN.md` and the deep dives are different depths of one subject: the design document holds the module map, the shapes and the trade-offs; a deep dive holds one mechanism too long for a section, and the design document links it instead of repeating it.

## Before writing a document

Name three things first. The document's audience, its home from the table above, and the documents it must link rather than restate. When all three are named, write it; when two homes both seem right, pick the audience that would be surprised to find it elsewhere.

Read before writing: `GLOSSARY.md`, the ADRs that touch the area, and `docs/DESIGN.md` if the writing describes how the system works.

## Who writes what

- **`/setup-matt-pocock-skills` owns `docs/agents/doc-layout.md`.** It creates it, and it is the only writer. This skill reads it and never rewrites it; where it is absent, the table above applies.
- **Create lazily.** A skill creates the file it needs at the moment it has something to say, and leaves an existing file alone. A missing document is a fact about the work, not a task to finish.
- **Cite, do not restate.** `README.md` indexes the docs; `DESIGN.md` points at the spec, the ADRs and the deep dives; a spec points at the ADRs it depends on. The copy that repeats another home's fact is the copy that goes stale.
- **An ADR cites a home inside the repo, not a skill.** A skill is installed in one agent's environment; a reader without it still needs the decision written down. Record the decision in prose and point at the in-repo document that carries it (`DESIGN.md`, `docs/agents/doc-layout.md`). Install the skill to write the document; cite the document to keep the decision.

## Skeleton first, filled later

`README.md` and `docs/DESIGN.md` may begin as skeletons: the headings a finished version will carry, with `TBD` beneath the ones not yet true. A heading with `TBD` is honest about work that is decided but not done; a missing heading hides a decision the next agent has to rediscover. Fill the skeletons as the implementation lands.

A `DESIGN.md` skeleton carries:

- **Context** — the problem and the shape of the system in a paragraph.
- **Module map** — a Mermaid graph of modules and their dependencies.
- **Modules and interfaces** — one section per module: what a caller must know, what sits behind its seam.
- **Seams and adapters** — where behaviour can be swapped, and what actually varies across each seam.
- **Data flow** — a Mermaid sequence or flow diagram for the paths that cross several modules.
- **Limitations** — what the design knowingly does not handle.
- **References** — the primary sources the design rests on.

## Growth

- **One active effort** lives in `docs/SPEC.md`. A second concurrent effort moves to `docs/specs/<slug>.md`, and `docs/SPEC.md` becomes the index of them.
- **Tickets** live at `.scratch/<feature>/issues/NN-<slug>.md`, numbered in dependency order, blockers first.
- **Temp files** live in `.scratch/tmp/`. Anything that outlives the session graduates into `docs/`.
- **ADRs are numbered once** and never renumbered. A reversal is a new ADR that supersedes the old one and links back to it.

## The flow

1. `grill-with-docs` — settle the idea, the vocabulary, and the decisions worth recording.
2. `design-with-docs` — decide the modules, their interfaces and seams; write `docs/DESIGN.md` and the ADRs.
3. `to-spec` — publish the spec to `docs/SPEC.md`.
4. `to-tickets` — slice the spec into `.scratch/<feature>/issues/`.
5. `implement` per ticket; the README and design skeletons fill in as the code lands.
6. `improve-codebase-architecture` — once code exists, survey it and fold what it finds back into the design.

## When the project differs

`docs/agents/doc-layout.md` wins over this table. Name the difference back to the user, then record a lasting exception there — or in an ADR, if the exception is a trade-off worth remembering — rather than editing this skill.
