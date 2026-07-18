---
name: discovery
description: Asks a series of guided questions to turn a vague idea into a Spec draft. Use when starting a new project from scratch and no Spec exists yet.
---

## Questions
- What's the objective of this project, in one phrase?
- What are the 3 minimum things it must be able to do (MVP features)?
- What should it explicitly NOT do or NOT be? (category exclusions, not just missing features)
- How does the first screen/interaction look?
- What data source(s) does it need?
- Are there constraints? (platform, timeline, who maintains it)

## How to ask
- Iterate — go deeper if an answer is vague or ambiguous.
- Don't ask all questions at once.
- Prefer closed/multiple-choice questions when the ambiguity is concrete;
  open questions when genuine exploration is needed.

## Beyond the list
The questions above are the required minimum — never skip them. But they are
a floor, not a ceiling. If something about this specific idea surfaces a real
ambiguity or risk not covered by the list (e.g. a legal/compliance concern, an
unusual data constraint, a technical limitation specific to this domain),
ask about it too. Note explicitly why you're asking beyond the standard list,
so it's clear this is a deliberate addition, not an inconsistent process.

## When to stop
- Stop once there's enough clarity to move to Technical Blueprinting (CLAUDE.md).
- Never drift into "how to build it" — that belongs to the next phase, not discovery.

## Result
A draft Spec with this structure:
- What it is
- MVP Features
- What it is NOT
- Home Screen (first user experience)
- Pending / Open Questions

## Quality criteria
- Every MVP feature has a clear yes/no boundary — no vague scope left.
- The "What it is NOT" section has at least one real exclusion, not left empty.
- No unresolved ambiguity should remain — anything undecided goes explicitly
  into "Pending / Open Questions", not silently assumed.
