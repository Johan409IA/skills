---
name: design-reference-implementer
description: Redesigns a whole frontend interface or a full component (form, header, navbar, footer, hero, card grid, sidebar, etc.) from a visual reference such as a screenshot, a low-fidelity prototype, an image, a logo or icons. Plans first with a short design spec the user must approve, then implements it reusing the project's existing components and tokens, and checks the result with Playwright CLI. Make sure to use this skill whenever the user says things like "implementa este diseño de esta imagen", "toma en cuenta la referencia de esta imagen", "inspírate de este diseño", "implement this design from this image", "use this screenshot as a reference", or attaches a design image and wants an interface or component rebuilt after it, even if they don't mention the word "skill". Do NOT use it for minor tweaks (changing a color, nudging a margin, fixing one button), because the planning and approval step is too heavy for small adjustments.
---

# Design Reference Implementer

The reference supplies **design only** (layout, hierarchy, palette, shape language). The project supplies everything else: framework, data, copy, components, behavior.

Redesigns touch many files and a wrong guess about palette or structure is expensive to undo. So the user approves a short spec before any code changes.

## Scope: is this a redesign or a tweak?

Use this skill when the reference changes the **structure or the whole look** of a page or component. Skip it when the change fits in one property or one element (a color, a margin, one button): just make it, no spec.

If it is in between (for example, restyling one existing component without changing its structure), ask the user one question: "full redesign with spec, or direct restyle?". Do not decide silently, because the spec step costs the user a round trip.

## What to take from the reference

Take only:

- **Design**: layout structure, hierarchy, component shapes, density, rhythm.
- **Colors**: the palette and how it is used (surfaces, accents, text, borders).

Never carry over:

- **Text, data or numbers.** The interface keeps its own content. If the screenshot says "Welcome back, Ana", the project does not start saying it.
- **Exact sizes.** Adapt them to the project's scale and viewport behavior.
- **Third-party brands or assets** (logos, illustrations, photos). If the user supplied a logo or asset to use, use theirs. A logo given as the reference itself is a palette and style source, so extract colors from it, but place the user's real asset if they have one.

Anything not measurable from an image (hex values from a compressed screenshot, font family, exact spacing) is an **estimate**. Label it so in the spec.

## Decisions that shape the workflow

### Before writing the spec

Read every image, then inspect the project. Detect the stack from `package.json`, config files and folder structure, and find where tokens live (CSS variables, Tailwind theme, SCSS variables, theme files). Reuse existing components and tokens and restyle them. Create new ones only when nothing suitable exists.

Handle these cases when they appear:

| Situation                                                          | What to do                                                                                                                                                   |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| No image, or too small or blurry to read                           | Ask for another one. Do not invent a design.                                                                                                                 |
| Several references that contradict each other                      | Ask which one wins for which region.                                                                                                                         |
| Only one viewport shown (usually desktop)                          | Decide mobile and tablet yourself and list that in the spec's open questions.                                                                                |
| Project has no tokens or design system                             | Propose a minimal token set in the spec and say where it will live. Do not scatter literals.                                                                 |
| Working tree has uncommitted changes                               | Tell the user and suggest committing or using a branch before a multi-file redesign. Do not create branches yourself.                                        |
| Reference shows one theme (light or dark) and the project has both | Derive the other theme from the same palette roles and list it in the spec as an estimate.                                                                   |
| Reference icons, or icons supplied by the user                     | Keep the project's icon set and match its stroke weight and fill style. Use user-supplied icons exactly as given.                                            |
| Whole app or many pages at once                                    | Split by page or region, with one spec and one approval each. Start with the shared shell (layout, navbar, tokens), because every other piece depends on it. |

### Does the refactor warning apply?

List every current component or element that will not exist, or will change function, after the redesign. If that list is **not empty**, the spec must open with a refactor warning that names them. This holds regardless of how similar the layouts look, because the user should never discover removed pieces after the fact. If the list is empty, adapt the existing pieces and skip the warning.

### Approval gate

MANDATORY: read [references/spec-template.md](references/spec-template.md) before drafting the spec. It defines the structure: palette, type scale, spacing, components (new, reused, removed), states, estimates.

Present the spec and ask whether to proceed. Do not touch any file until the user approves. If they do not approve, ask what to change, update the spec and present it again.

## Implementing

Apply the approved spec with the project's own conventions. Map palette and scale onto existing tokens, and add new tokens in the place the project already keeps them. Data, copy, routes and behavior stay intact.

Accessibility wins over fidelity. Keep text contrast at WCAG AA, give interactive elements a visible focus style, and keep tap targets usable on mobile. If a reference color pair fails contrast, adjust it minimally and report it.

## Verifying

MANDATORY: read [references/verification.md](references/verification.md) before opening the browser. Do NOT load it before the spec is approved, because it is not needed until then.

Do not build a side-by-side comparison against the reference. It lengthens the task with little benefit, and the user will judge the visual match.

### One correction round, then stop

The first implementation is attempt one. If verification finds problems, make **one** round of corrections and re-check only what changed. If problems remain, stop. Say the work is finished, list what is still off and ask the user to review manually. More rounds rarely converge and burn the user's time.

## Final report

Keep it short. In the user's language:

1. **Summary** of what was implemented, in a few lines.
2. **Screenshots** from verification, with their location and the browser used.
3. **Known differences** from the reference: estimates, accessibility adjustments, removed components, states not applicable, anything that could not be verified.

## NEVER

- NEVER hardcode hex or px when a token exists. It breaks theming and dark mode, and makes later palette changes a search-and-replace.
- NEVER swap the project's font family for the reference's look without listing it in the spec. Fonts change layout everywhere, and the reference's font is only an estimate.
- NEVER add a new UI library to match a component. It bloats the bundle and splits the codebase into two styling systems.
- NEVER mix two icon sets in one view. Different stroke weights and corner styles read as a bug, even when each icon looks fine alone.
- NEVER touch logic, routes, API calls or i18n keys. Only the design is in scope, and behavior bugs would be invisible in screenshots.
