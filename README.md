# Screen Requirements Skill

A Claude Code skill that turns a product idea into structured specification artifacts for agentic prototyping. Run one command at a time, or sprint through the whole chain in five steps. Hand off directly to UI generation tools like v0, Lovable, Bolt, Claude Design, or Google Stitch.

---

## What it does

This skill gives Claude Code a set of `spec.*` commands that walk you from a raw idea through personas, journeys, flows, and screen specifications — producing a folder of markdown files that a UI generation agent can work from.

Each command does one job, pauses for review, and stops. You stay in control of every step.

```
spec.brief → spec.personas → spec.journeys → spec.flows →
spec.states → spec.design → spec.map → spec.screens → spec.prepare
```

Or run the whole thing in one shot:

```
spec.oneshot
```

---

## Output structure

All artifacts land in a `./spec/` folder in your project:

```
spec/
  brief.md
  value-proposition.md
  design.md
  prepare-brief.md
  patterns.md              (optional — add after first screens)

  personas/
    andy-the-app-user.md   (one file per persona)
    permissions.md

  journeys/
    overview.md
    andy-discovers-the-app.md

  flows/
    andy-submits-a-claim.md

  screens/
    inventory.md
    sitemap.md
    home-page.md       (one file per screen)
    claim-page.md

  data/
    states.md
    requirements.md
```

## Commands

### Navigation

| Command | What it does |
|---|---|
| `spec.help` | Print the full command reference |
| `spec.status` | Show which artifacts exist and what to run next |

### Shortcuts

| Command | What it does |
|---|---|
| `spec.oneshot` | Full prototype spec in one pass — no pausing |
| `spec.sprint` | 5-step condensed flow — pauses between steps |

### Step by step

| Command | Output | Notes |
|---|---|---|
| `spec.brief` | `spec/brief.md` | Start here |
| `spec.value` | `spec/value-proposition.md` | Optional for prototyping |
| `spec.personas` | `spec/personas/[name].md` | One file per persona, alliterative names |
| `spec.journeys` | `spec/journeys/[persona]-[scenario].md` | One file per journey |
| `spec.flows` | `spec/flows/[persona]-[scenario].md` | One file per flow |
| `spec.states` | `spec/data/states.md` | Run before screens |
| `spec.design` | `spec/design.md` | Run after flows and states |
| `spec.map` | `spec/screens/inventory.md` + `spec/screens/sitemap.md` | Screen list and connection map |
| `spec.screens` | `spec/screens/[screen-name].md` | One file per screen |
| `spec.prepare` | `spec/prepare-brief.md` | Generation handoff brief |

### Generation and iteration

| Command | What it does |
|---|---|
| `spec.generate [screen-name]` | Packages design, screen spec, and brief into a single paste-ready generation prompt |
| `spec.change [description]` | Analyses impact of a change, confirms, then updates affected files |
| `spec.patterns` | Builds and maintains a behavioural pattern library (optional) |

### Engineering handoff (after prototype is validated)

| Command | Output |
|---|---|
| `spec.contracts` | `spec/screens/contracts.md` |
| `spec.data` | `spec/data/requirements.md` |
| `spec.actions` | `spec/actions.md` |
| `spec.review` | `spec/readiness-review.md` |

---

## Usage examples

**Start from a PRD:**
```
Read prd.md and run spec.brief
```

**Start from nothing:**
```
spec.brief
```
Claude will ask for your idea and draft from scratch.

**Run the condensed 5-step flow:**
```
Read prd.md and run spec.sprint
```

**Generate a screen prompt for a specific tool:**
```
spec.generate map-workspace — target tool: Claude Design
```

**Apply a change across the spec:**
```
spec.change add offline support for the mob movement screen
```

**Check where you are:**
```
spec.status
```

---

## How each command works

Every `spec.*` command follows the same pattern:

1. **Asks for input** — existing content, file path, URL, or "nothing" to draft from context
2. **Validates** — checks what's present, incomplete, or missing
3. **Drafts or completes** — fills gaps from prior artifacts, marks inferences as assumptions
4. **Lists assumptions** — flags what was inferred and at what confidence
5. **Asks about gaps** — asks only about what's genuinely missing
6. **Outputs and stops** — writes files and waits for your review

If you provide input inline with the command, the input question is skipped automatically:
```
Read prd.md and run spec.brief
spec.design — using Shadcn, Tailwind, dark mode
spec.personas — nothing
```

Files drafted from context are marked provisional at the top:
```
> ⚠️ Provisional — estimated from context, not supplied or validated.
```
Supplied documents carry no such notice — the absence of the notice means the content came from you.

---

## Screen generation

`spec.generate` packages four files into a single ready-to-paste prompt:

- `spec/design.md` — product feel, platform, and behavioural requirements
- `spec/screens/[screen-name].md` — this screen's spec
- `spec/prepare-brief.md` — mock data and product context
- `spec/screens/sitemap.md` — navigation context (if present)
- `spec/patterns.md` — pattern constraints (if present)

Supported tools: **v0**, **Lovable**, **Bolt**, **Claude Design**, **Google Stitch**, or any other UI generation tool.

Each screen spec includes:
- Design intent — purpose, persona, primary job, content priority
- Decisions on this screen — consequential choices with stakes and reversibility
- States, actions, and data requirements
- Critical variants — named rendering modes
- Layout regions — functional structure without implementation prescription
- Screen generation prompt — one dense paragraph describing behaviour, not components
- Acceptance criteria — verifiable by looking at the generated screen
- Heuristic check — six NNG heuristics evaluated at spec level before generation

---

## Recommended prototype path

```
spec.brief
spec.personas
spec.journeys
spec.flows
spec.states
spec.design
spec.map
spec.screens
spec.prepare
```

Then for each screen you want to generate:
```
spec.generate [screen-name]
```

Add `spec.patterns` after your first generated screens to capture what works and what does not.

---

## Personas

Personas use alliterative names generated automatically from the role — Andy the App User, Rachel the Receptionist, Felix the Field Technician. Names are chosen to feel natural in a design conversation: "what does Andy need here?" works better than "what does Persona 2 need?"

Each persona file covers: composite summary, identity, context of use, technical profile, time and attention, decision authority, workday shape, goals, frustrations, constraints mapped to design implications, jobs performed, and scenarios.

---

## Pattern library

`spec.patterns` is optional and skippable. Run it after generating your first screens when you have real feedback — not speculatively upfront.

The pattern library captures behavioural design decisions, not component choices. It grows from what generation tools get right and wrong on your specific product.

```
spec.patterns                          # start or add from feedback
spec.patterns add [description]        # add a single pattern
spec.patterns review                   # cross-check patterns against screen specs
```

---

## Iterating on a live spec

`spec.change` lets you apply a feature addition or change to an existing spec without re-running the whole chain:

```
spec.change add a QR code scan option to the source connection flow
spec.change new persona: farm owner who reviews weekly reports
spec.change rename paddock to block throughout
```

It analyses which files are affected, shows you the impact list, waits for confirmation, then updates only the affected files in dependency order. Every changed file gets a change log entry so you can track what was modified and why.

---
