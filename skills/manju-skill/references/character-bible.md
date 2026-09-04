# Character Bible

Use this reference for `character` mode. Design characters who remain visually stable across generated shots while still making independent choices and changing through the story.

## Required character model

For every major character, define:

- **Identity:** name, explicit adult age where romance may occur, role, occupation or social position.
- **External goal:** what they are actively trying to achieve.
- **Internal pressure:** fear, belief, debt, loyalty, shame, or contradiction that complicates the goal.
- **Agency:** decisions only this character can make and consequences caused by those decisions.
- **Competence and limit:** what they do well, where they fail, and why the story still needs others.
- **Relationship stance:** what they want from each key character, what they refuse, and what can change that position.
- **Arc turns:** starting state, midpoint choice, crisis choice, and ending state.

Avoid describing a character only through attractiveness, devotion to the protagonist, or usefulness as a victim.

## Visual identity anchors

Use a compact identity block that can be repeated in image or video prompts:

1. Apparent age and build.
2. Face shape and two stable facial traits.
3. Hair shape, color, and texture.
4. Base costume silhouette and material.
5. Signature color plus one recurring prop or accessory.
6. Default posture or gesture.

Separate stable identity from scene state:

- **Stable:** face, hair, body proportions, signature accessory, palette.
- **Stage-specific:** costume evolution, injury, status symbols, grooming.
- **Shot-specific:** expression, pose, weather, dirt, lighting, camera angle.

Do not change stable anchors merely to make a later version look “stronger.” Show growth through silhouette, condition, posture, blocking, and environment.

## Costume and state progression

Create only the stages the outline needs. For each stage specify:

| Field | Purpose |
|---|---|
| Episode range | Prevent premature visual changes |
| Silhouette | Make the stage readable at thumbnail size |
| Materials and palette | Support prompt consistency |
| Story meaning | Explain why the design changed |
| Locked elements | Preserve identity across the change |

Avoid real luxury brands unless the user specifically needs them. Describe visible materials and design cues instead.

## Relationship design

Each important relationship needs reciprocal goals. Track:

- Current alignment: allied, opposed, dependent, uncertain, or estranged.
- Power balance and each person's real ability to refuse.
- Source of attraction, trust, rivalry, resentment, or obligation.
- Boundary neither side will cross.
- Next choice that changes the relationship.

For romance, state that all participants are adults and distinguish desire from consent. Survival dependence, employment, command authority, captivity, medical vulnerability, or intoxication cannot substitute for freely given agreement.

## Prompt block

When visual keywords are useful, produce:

```text
Identity anchors: [stable physical and costume traits]
Stage state: [episode-range design and condition]
Shot state: [action, expression, environment, lighting]
Avoid drift: [traits that must not change]
```

Add an English version only when requested or when the target generator benefits from English prompts.

## Character self-check

- Every major character has a goal unrelated to pleasing the protagonist.
- At least one plot event changes because of each major character's choice.
- Visual anchors are short enough to repeat consistently.
- Costume changes have story causes and episode ranges.
- Abilities have visible cues, limits, and consequences.
- Relationships evolve through choices rather than sudden submission.
- All romantic participants are explicitly adult and able to consent.
