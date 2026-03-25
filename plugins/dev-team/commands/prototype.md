---
description: Build a clickable interactive prototype to validate design direction before full implementation.
argument-hint: "<feature to prototype>"
---

Use the `frontend` agent to build a **clickable interactive prototype** for the following task.

The goal is a working demo — not production code. Focus on visual fidelity, navigation flow, transitions, and interactive states. Skip tests, error handling, and production polish.

## Before coding, commit to an aesthetic direction:

- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Pick a deliberate aesthetic — brutally minimal, luxury/refined, playful, retro-futuristic, editorial, organic, industrial, soft/pastel, bold maximalist, etc. Be specific.
- **Constraints**: Technical or brand limitations (platform, framework, existing design system).
- **Differentiation**: What should make this interface memorable?

## Aesthetic rules:

- Choose distinctive, characterful fonts — never default to Inter, Roboto, Arial, or system fonts.
- Commit to a cohesive color palette. Dominant colors with sharp accents over timid, evenly-distributed palettes.
- Add atmosphere: gradient meshes, noise textures, geometric patterns, layered transparencies, or grain overlays where they serve the aesthetic.
- Use animation for delight: staggered reveals on load, hover states that surprise, scroll-triggered effects. Prefer CSS-only; always include `prefers-reduced-motion` fallbacks.
- Never use emoji as UI icons — they look cheap and inconsistent. Use inline SVG icons styled to match the aesthetic. If an icon isn't essential, use typography or spatial design instead.

## Output:

- If no project framework exists → standalone HTML/CSS/JS file(s) in `prototypes/<feature>/`.
- If inside an existing project → use the project's framework (React, Vue, etc.) and place in `prototypes/<feature>/`.
- Working navigation between views, hover/active states, placeholder content. No backend, no real data.

Check `specs/ux-research.md` and `specs/ui-design.md` for existing research and design specs to inform the prototype.

Task: $ARGUMENTS
