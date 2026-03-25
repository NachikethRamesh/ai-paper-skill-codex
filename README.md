# AI Paper Skill

Codex skill for turning AI/ML research papers into interactive, non-technical, single-page HTML lessons.

For Claude Code ai-paper-skill, check out: https://github.com/NachikethRamesh/ai-paper-skill

`SKILL.md` is the source of truth. This README is a quick overview for humans.

## What This Skill Produces

A self-contained HTML lesson that:

- explains the paper in plain language using analogy-first teaching
- breaks down equations with Math -> English mappings
- includes annotated code examples for intuition (not production code)
- visualizes pipelines, architecture, comparisons, and results
- embeds quizzes that test understanding, not memorization
- defines technical terms with glossary tooltips

Screenshots of some outputs - 

<img width="2878" height="1469" alt="image" src="https://github.com/user-attachments/assets/db1e8cf9-8fb0-4829-b9f9-4c492ec593d0" />
<img width="2879" height="1376" alt="image" src="https://github.com/user-attachments/assets/d752e489-d31d-4066-83b3-e9d728026ab3" />
<img width="2879" height="1471" alt="image" src="https://github.com/user-attachments/assets/d52d1730-56de-4b98-8dd7-e57a290cf21f" />

## Target Audience

Non-technical readers who want to understand real AI papers without needing deep ML or math background.

## Workflow (4 Phases)

1. Analyze the full paper deeply (problem, method, math, results, limits, context).
2. Design a modular lesson arc (typically 5-10 modules).
3. Build one self-contained HTML file with CSS + JS (module by module).
4. Review in browser and deliver.

## Core Requirements

The lesson must include:

- clear problem framing before technical detail
- at least one unique analogy per module
- pipeline animation(s)
- Math -> English translation blocks with line-by-line alignment
- code examples
- concept quizzes (at least one per module)
- glossary tooltips for technical terms
- architecture diagrams
- comparison cards
- research context timeline
- key contribution cards
- related work comparison matrix
- ablation visualizer when ablation results exist in the paper

## Technical Constraints

- output is a single HTML file (Google Fonts allowed; no other external dependencies)
- use `scroll-snap-type: y proximity` (not `mandatory`)
- use `min-height: 100dvh` with `100vh` fallback
- keep JS wrapped in an IIFE
- use passive scroll listeners and `requestAnimationFrame` throttling
- support keyboard navigation, touch interactions, and ARIA-friendly markup

## Directory Layout

```text
ai-paper-skill/
|-- README.md
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
`-- references/
    |-- design-system.md
    `-- interactive-elements.md
```

## Example Invocation

`Use $ai-paper-skill to convert this AI research PDF into a single-page interactive HTML lesson for a non-technical audience.`
