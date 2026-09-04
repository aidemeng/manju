---
name: manju-skill
description: Use when creating, continuing, or reviewing Chinese visual-first AI comic dramas, including story outlines, character bibles, episode catalogs, and storyboard scripts.
---

# Manju Visual Drama

Create production-ready Chinese vertical comic-drama materials whose story beats can be translated into stable images or short video shots. Preserve the user's premise and audience instead of forcing a fixed genre or gender perspective.

## Choose one mode

| Mode | Use when | Required upstream | Read | Output |
|---|---|---|---|---|
| `outline` | Starting or restructuring a story | User brief; existing `project.md` when continuing | [story development](references/story-development.md), [project template](assets/templates/project.md), [outline template](assets/templates/outline.md) | `project.md`, `outline.md` |
| `character` | Creating or revising the cast bible | `project.md` and `outline.md` or an equivalent approved story direction | [character bible](references/character-bible.md), [characters template](assets/templates/characters.md) | `characters.md` |
| `catalog` | Turning the outline into a production index | `project.md`, `outline.md`, `characters.md` | [episode catalog](references/episode-catalog.md), [catalog template](assets/templates/catalog.md) | `catalog.md` |
| `write` | Writing named episodes or a batch | `project.md`, `outline.md`, `characters.md`, `catalog.md` | [storyboard writing](references/storyboard-writing.md), [episode template](assets/templates/episode.md) | `episodes/episode-NNN.md` |
| `review` | Evaluating or revising a batch | All available upstream files plus target episodes | [quality review](references/quality-review.md), [review template](assets/templates/review.md) | `reviews/batch-NN.md` |

Read [the compact example](references/example.md) only when output relationships remain unclear. Do not load references for unrelated modes.

## Defaults

- 80 episodes, 120 seconds each, 5 episodes per batch, 16 batches.
- Five acts of 16 episodes: trigger, expansion, midpoint reversal, breakdown/rebuild, finale/aftermath.
- Chinese output; add English image keywords only when requested or useful for the stated generator.
- Write to `manju-output/<work-name>/` unless the user supplies another directory. If the user asks only for an in-chat draft, do not create files.

Explicit user requirements override these defaults. Do not silently force a project back to 80 episodes when continuing an existing work.

## Workflow

1. Identify the requested mode from the user's goal. If genuinely ambiguous, ask one decisive question.
2. For a new project, establish title, premise, audience, protagonist goal, story mechanism, visual direction, target medium, and content rating. Make reasonable suggestions for missing low-risk fields.
3. Before `character`, `catalog`, `write`, or `review`, inspect the output directory and read existing upstream files. Continue them; do not regenerate them.
4. Report missing required inputs first. Create them only when doing so is within the user's requested scope; otherwise stop that mode without inventing upstream facts.
5. Copy the relevant template from `assets/templates/`, replace every bracketed instruction, and keep facts consistent with upstream files. Normalize every derived value from `project.md`, including episode count, duration, batch size/count, act boundaries, batch ranges, and progress totals; template defaults are not immutable literals.
6. Update existing files incrementally. Explain the impact and obtain confirmation before replacing substantial authored material.
7. After each completed `write` batch, run `review` once. Scores below 80 or any severe safety issue require revision before the next writing batch.

## Non-negotiable constraints

- Every participant in romance or sexual tension must be explicitly adult.
- Never sexualize minors or minor-coded characters, including childlike body framing, school-uniform fetish framing, or “wait until grown” setups.
- Do not frame intimacy exchanged for food, shelter, safety, employment, or power as consent or romance.
- Consent must be freely given. Do not romanticize coercion, inability to refuse, intoxication, captivity, medical vulnerability, or a material power imbalance as agreement.
- Romantic tension is optional. Trust, rivalry, sacrifice, mystery, moral choice, and betrayal are equally valid hooks.
- Give major characters independent goals, consequential choices, and plot function; do not treat them as prizes or humiliation props.
- Prefer visible, generatable action over abstract emotion, but do not mechanically stuff every shot with adjectives.
- Keep violence non-graphic and non-realistic by default; intensify it only within an explicitly approved platform and audience boundary.
- Follow the target platform's stricter content, violence, and advertising rules when supplied.

## Completion check

Confirm that the requested files exist, no template instructions remain, episode numbering and upstream facts agree, storyboard output uses one Markdown table format, and review evidence cites episode and shot numbers.
