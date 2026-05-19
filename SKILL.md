# Product Spec Builder

A command-driven skill for building structured product specification artifacts one step at a time. Each command produces one or more output files, pauses for review, and stops.

All outputs are written into a `./spec/` folder hierarchy in the current working directory.

---

## CRITICAL BEHAVIOUR RULES

These rules apply to every command without exception:

1. **Always ask for input before doing anything.** When any `spec.*` command runs, the first action is always to ask the user the input question and wait for their reply. Do not read files. Do not draft. Do not proceed until the user responds.

2. **One command, one output, then stop.** After writing output files, end with the stop phrase and wait. Do not continue to the next command automatically.

3. **If input is provided inline with the command**, treat that as the answer to the input question and proceed directly to validation. Do not ask again.

4. **"nothing"** is a valid answer to the input question. It means: draft from whatever is already in `./spec/` and mark all inferences as assumptions.

5. **Never skip the input question**, even when chaining commands or when prior outputs exist.

---

## Output folder structure

```
./spec/
  brief.md
  value-proposition.md
  design.md
  actions.md
  prepare-brief.md
  readiness-review.md
  patterns.md          optional — add after first screens are generated

  /personas/
    [archetype-name].md          one file per persona
    permissions.md               permission matrix across all personas
                                 e.g. andy-the-app-user.md

  /journeys/
    overview.md                  journey index
    [persona]-[scenario].md      one file per journey
                                 e.g. andy-discovers-the-app.md

  /flows/
    [persona]-[scenario].md      one file per flow
                                 e.g. andy-submits-a-claim.md

  /screens/
    inventory.md
    contracts.md
    [screen-name].md     one file per screen, named by slug
                         e.g. map-workspace.md, mob-movement.md

  /data/
    requirements.md
    states.md
```

---

## Persona naming convention

Personas use alliterative names — a first name and a role descriptor sharing the same starting letter. Generate these automatically from the role.

**Examples:**
- App user → Andy the App User
- Receptionist → Rachel the Receptionist
- Farm worker → Aroha the Agile Farm Hand
- Manager → Marcus the Manager
- Administrator → Alice the Administrator
- Field technician → Felix the Field Technician
- Driver → Danny the Driver
- Supervisor → Sam the Supervisor

**Rules:**
- Name must be memorable and human, not clinical
- Role descriptor should reflect how the persona actually spends their day, not just their job title
- Prefer names that feel natural in a design conversation — "what does Andy need here?" should be easy to say
- File name is slugified: `andy-the-app-user.md`
- Use names that feel appropriate to the product's cultural context if known

---

## Commands

### spec.help

Print the full command reference. No file reading required — print this output directly.

```
Product Spec Builder — commands

  spec.help      Show this reference
  spec.status    Show current progress across the spec folder

  --- shortcuts ---
  spec.oneshot   One-pass full prototype spec from a single input — no pausing
  spec.sprint    5-step condensed flow — pauses between steps, groups related commands

  --- step by step ---
  spec.brief     Product brief — what it is, who it's for, why it exists
  spec.value     Value proposition and design principles (optional for prototyping)
  spec.personas  Personas — one file each, alliterative names, light or full depth
  spec.journeys  Journey maps — overview index + one file per journey
  spec.flows     User flows — elaborates journey stages into precise steps
  spec.states    Workflow state model — run before screens, informs state rendering
  spec.design    Design system — run after flows and states; light or full depth
  spec.map       Screen inventory and sitemap — all screens, connections, entry points
  spec.screens   Screen specifications — one file per screen, decisions embedded
  spec.prepare   Generation brief — packages spec for v0, Lovable, Bolt, Claude Design, Stitch
  spec.generate  Single-screen generation prompt — packages design, screen, and brief into one paste
  spec.change    Ingest a change or new feature, analyse impact, update affected artifacts

  --- engineering handoff (after prototype is validated) ---
  spec.contracts Page contracts — decisions and actions per screen
  spec.data      Data requirements — fields, sources, loading states
  spec.actions   Action specifications — full detail per user action
  spec.review    Readiness review — scores, gaps, traceability check

Recommended prototype path:
  spec.brief → spec.personas → spec.journeys → spec.flows → spec.states → spec.design → spec.map → spec.screens → spec.prepare

Outputs go into ./spec/
Run any command standalone or pass input inline:
  spec.brief
  Read prd.md and run spec.brief
  spec.design — using Shadcn, Tailwind, dark mode
  spec.personas — nothing
  spec.screens — target tool: v0
  spec.oneshot — target tool: Lovable
  Read prd.md and run spec.sprint
```

### spec.status

Scan `./spec/` and print a structured progress view. Do not ask for input — read the folder and print the output immediately.

Group files by section. Show each file as done (✓) or missing (✗). List persona, journey, and flow files individually. Show a done count and next recommended command at the bottom.

If `./spec/` does not exist, print: `No spec folder found. Run spec.brief to start.`

Output format:
```
─────────────────────────────────────────────
  Product Spec Builder — current progress
─────────────────────────────────────────────

  Foundation
  spec/brief.md                    ✓  done
  spec/value-proposition.md        ✓  done
  spec/design.md                   ✗  missing

  Personas
  spec/personas/andy-the-app-user.md         ✓  done
  spec/personas/rachel-the-receptionist.md   ✓  done
  spec/personas/permissions.md               ✓  done

  Journeys
  spec/journeys/overview.md                  ✗  missing
  spec/journeys/andy-discovers-the-app.md    ✗  missing

  Flows
  spec/flows/andy-onboarding.md              ✗  missing

  States
  spec/data/states.md              ✗  missing

  Screens
  spec/screens/inventory.md        ✗  missing
  spec/screens/[screen-name].md    ✗  missing  (one per screen after spec.screens runs)

  Patterns (optional)
  spec/patterns.md                 ✗  not yet — run spec.patterns after first screens

  Prototype handoff
  spec/prepare-brief.md            ✗  missing

  Engineering handoff (optional)
  spec/screens/contracts.md        ✗  missing
  spec/data/requirements.md        ✗  missing
  spec/actions.md                  ✗  missing
  spec/readiness-review.md         ✗  missing

─────────────────────────────────────────────
  Done: 4 of 18+   Next: spec.design
─────────────────────────────────────────────
```

### spec.oneshot
Produce the full prototype artifact chain in one pass from a single input. No pausing between steps.

### spec.sprint
Run the 5-step condensed prototype flow. Pauses between steps for review. Groups related commands within each step.

### spec.brief
Produce `./spec/brief.md`

### spec.value
Produce `./spec/value-proposition.md`

### spec.design
Produce `./spec/design.md`

### spec.personas
Produce one file per persona in `./spec/personas/` and `./spec/personas/permissions.md`

### spec.journeys
Produce `./spec/journeys/overview.md` and one file per journey in `./spec/journeys/`

### spec.flows
Produce one file per flow in `./spec/flows/`

### spec.states
Produce `./spec/data/states.md`

### spec.map
Produce `./spec/screens/inventory.md` and `./spec/screens/sitemap.md`

### spec.contracts
Produce `./spec/screens/contracts.md`

### spec.data
Produce `./spec/data/requirements.md`

### spec.actions
Produce `./spec/actions.md`

### spec.screens
Produce one file per screen at `./spec/screens/[screen-name].md`

### spec.prepare
Produce `./spec/prepare-brief.md`

### spec.generate
Produce a single-screen generation prompt for a named screen. Usage: `spec.generate [screen-name]`

### spec.change
Ingest a change description or new feature, analyse impact across existing artifacts, confirm with user, then update only affected files.

### spec.patterns
Build and maintain `./spec/patterns.md` — a behavioural pattern library. Optional and skippable — run after generating screens when you have real feedback on what worked and what did not.

### spec.review
Produce `./spec/readiness-review.md`

---

## Standard step pattern

Every command follows these six steps in order.

### Step 1 — Ask for input

**This is mandatory. Do it before anything else.**

Ask the user:
> Do you have existing content for this artifact? Share a file path, paste content directly, or give me a URL. If you have nothing, say "nothing" and I will draft from scratch.

Then wait. Do not proceed until the user replies.

If the user provided input inline with the command (e.g. `Read prd.md and run spec.brief`), treat that as their answer and skip the question.

### Step 2 — Validate

Check the input (or draft) against the required fields for this command. Mark each field as **present**, **incomplete**, or **missing**.

### Step 3 — Draft or complete

If the user said "nothing": draft all fields from prior outputs in `./spec/`. Mark every inferred item as an assumption.

If the user provided input: fill only the missing or incomplete fields. Preserve everything the user supplied unchanged.

### Step 4 — List assumptions

List every assumption made:
- `[assumed — high confidence]` — safe default, unlikely to need correction
- `[assumed — low confidence]` — materially affects downstream artifacts, needs confirmation

### Step 5 — Ask about gaps

Ask only about fields that are missing or marked low confidence. Name each field and explain in one sentence why it matters. Do not re-ask about fields already supplied.

### Step 6 — Output and stop

Before writing the file, determine the content origin:

- **Supplied** — the user provided this content directly (file, paste, or URL). The artifact reflects their input.
- **Estimated** — the content was drafted from scratch or largely inferred from other artifacts without direct user input.

If the artifact is **estimated**, add this notice as the first line of the file, before any other content:

```
> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.
```

If the artifact is **supplied** (or supplied with minor gaps filled), do not add the notice. The absence of the notice means the content came from the user.

If an artifact starts as estimated and the user later supplies corrections, remove the notice when rewriting the file.

Write the completed artifact(s) to the correct path(s) in `./spec/`. Then end with:

```
Reply with answers, corrections, or "accept assumptions" to finalise this artifact.
Type the next spec.* command when you are ready to continue.
```

Do not generate any other artifact. Do not continue to the next step automatically.

---

## Artifact chain and dependencies

Shortcut commands:

| Command | Equivalent to | Notes |
|---|---|---|
| spec.oneshot | All prototype steps in one pass | No pausing. All outputs marked provisional. Clarification questions at end. |
| spec.sprint step 1 | spec.brief + spec.personas | Asks for input once, produces both artifacts |
| spec.sprint step 2 | spec.journeys + spec.flows | Asks for input once, produces journey and flow files |
| spec.sprint step 3 | spec.states | Produces states model |
| spec.sprint step 4 | spec.design | Standard spec.design behaviour |
| spec.sprint step 5 | spec.map + spec.screens + spec.prepare | Produces inventory, sitemap, one file per screen, and prepare brief |

Step-by-step commands:

| # | Command | Output path(s) | Reads from | Notes |
|---|---|---|---|---|
| 1 | spec.brief | spec/brief.md | user input | |
| 2 | spec.value | spec/value-proposition.md | brief.md | Optional for prototyping |
| 3 | spec.personas | spec/personas/[name].md + permissions.md | brief.md, value-proposition.md | |
| 4 | spec.journeys | spec/journeys/overview.md + [persona]-[scenario].md | personas/ | |
| 5 | spec.flows | spec/flows/[persona]-[scenario].md | personas/, journeys/ | Elaborates journey stages into precise steps |
| 6 | spec.states | spec/data/states.md | flows/ | Run before screens — informs state rendering |
| 7 | spec.design | spec/design.md | flows/, data/states.md, brief.md | Run after flows and states |
| 8 | spec.map | spec/screens/inventory.md + spec/screens/sitemap.md | journeys/, flows/ | |
| 9 | spec.screens | spec/screens/[screen-name].md (one per screen) | design.md, screens/inventory.md, data/states.md, journeys/, flows/ | Decisions embedded per screen |
| 10 | spec.prepare | spec/prepare-brief.md | design.md, screens/inventory.md, screens/*.md | Links to individual screen files |
| * | spec.generate | stdout only — no file written | design.md, screens/[name].md, prepare-brief.md, sitemap.md | Run any time after spec.screens |
| * | spec.change | updates affected files in place | all existing spec files | Run any time — analyses impact before writing |
| * | spec.patterns | spec/patterns.md | design.md, screens/*.md, user feedback | Optional — skippable, grows from generation feedback |
| — | spec.contracts | spec/screens/contracts.md | data/states.md, screens/inventory.md | Engineering handoff |
| — | spec.data | spec/data/requirements.md | screens/contracts.md | Engineering handoff |
| — | spec.actions | spec/actions.md | data/states.md, screens/contracts.md | Engineering handoff |
| — | spec.review | spec/readiness-review.md | all | Engineering handoff |

**Recommended prototype path:**
```
spec.brief → spec.personas → spec.journeys → spec.flows → spec.states → spec.design → spec.map → spec.screens → spec.prepare
```

**Engineering handoff** (run after prototype is validated):
```
spec.contracts → spec.data → spec.actions → spec.review
```

**spec.value** can be added between spec.brief and spec.personas once the flow is working — useful for stakeholder alignment but not required for screen generation.

---

## Command definitions

---

### spec.brief

**Output:** `./spec/brief.md`
**Reads:** user input

**Required fields:**
- Product name or working title
- Problem statement — what problem exists, for whom, and why it matters
- Product purpose — what the product does and for whom (two sentences max)
- Target users — high level, not yet detailed personas
- Business goal
- Success criteria
- Scope — in scope and explicitly out of scope
- Known constraints — technical, regulatory, timeline, budget, team
- Key assumptions — treated as true but not yet validated
- Open questions — unknown and could affect direction

**Template:**
```markdown
# Product Brief

## Product name
[Name or working title]

## Problem statement
[What problem, for whom, why it matters now]

## Product purpose
[What it does and for whom. Two sentences max.]

## Target users
[High level. Detailed personas come later.]

## Business goal
[What the business needs this to achieve]

## Success criteria
[How success is measured]

## Scope

### In scope

### Out of scope

## Known constraints

## Key assumptions

## Open questions
```

---

### spec.value

**Output:** `./spec/value-proposition.md`
**Reads:** `spec/brief.md`

**Required fields:**
- Value proposition — structured: for / who needs / the product is / that / unlike / our product
- Design principles — 3 to 6, each product-specific with a name and one-sentence meaning
- Non-goals

**Important:** Principles must be specific to this product. If a principle is generic ("be simple", "delight users"), reframe it as a specific commitment. For example, "be simple" on a finance product becomes "never show a number without context."

**Template:**
```markdown
# Value Proposition and Design Principles

## Value proposition

**For** [target user]
**Who needs** [need or job-to-be-done]
**The product is** [product name and category]
**That** [primary benefit]
**Unlike** [current alternative]
**Our product** [key differentiator]

## Design principles

### 1. [Principle name]
[What this means specifically for this product]

### 2. [Principle name]
[What this means specifically for this product]

### 3. [Principle name]
[What this means specifically for this product]

[Up to 6 total]

## Non-goals
[What this product should deliberately not try to do]
```

---

### spec.design

**Output:** `./spec/design.md`
**Reads:** `spec/flows/`, `spec/brief.md`, user input (existing design file, tokens, style guide)

**When to run:** After spec.flows, not before. The flows reveal what the screens need to do — whether this is a mobile-first field tool, a desktop dashboard, or a multi-step form — which should inform design decisions before committing to tokens and patterns.

**Two paths:**
- **Existing file provided:** validate against required fields, complete gaps only, preserve everything supplied
- **Nothing provided:** draft defaults based on product type and flow context. Present as a block for confirmation — do not ask field by field.

**Required fields (core — needed for screen generation):**
- Framework and target environment (React / Next.js / Vue / mobile / other)
- Component library name, version, and styling approach (CSS variables vs Tailwind)
- Dark mode support
- Visual tone — one sentence describing the intended feel (e.g. "clean and utilitarian for field use", "warm and approachable for consumers")
- Navigation pattern and mobile behaviour
- Primary color, background, and surface tokens — enough for a generation tool to apply consistent colour
- Border radius and elevation style
- Interaction patterns — loading states, empty states, error states, success feedback, confirmation for destructive actions
- Tone and voice — label casing, button copy convention, error message tone
- Icon set and size scale
- Accessibility baseline — WCAG level and minimum contrast

**Extended fields (add when moving toward engineering handoff):**
- Full color token set with foreground variants
- Complete typography scale
- Full spacing system
- Complete breakpoint and layout definitions

**Do not include:** photography style, illustration style, or other brand-only fields not used by code generation tools.

**Template:**
```markdown
# Design System Context

> Referenced by all screen artifacts. Every component in screen specs must be named here.

## Framework
- Framework:
- Component library:
- Library version:
- Documentation URL:
- Styling: (CSS variables / Tailwind / other)
- Dark mode: (yes / no / system)
- Custom component overrides:

## Visual tone
[One sentence. What this product should feel like — e.g. "Clean and utilitarian for field workers who need fast, glanceable information" or "Warm and approachable, built for people who distrust software".]

## Color tokens

| Token | Light | Dark | Usage |
|---|---|---|---|
| primary | | | Main actions, links |
| primary-foreground | | | Text on primary |
| secondary | | | Secondary actions |
| secondary-foreground | | | |
| background | | | Page background |
| foreground | | | Default text |
| card | | | Card backgrounds |
| card-foreground | | | |
| muted | | | Subtle backgrounds |
| muted-foreground | | | Placeholder text |
| border | | | Default borders |
| error | | | Error states |
| error-foreground | | | |
| success | | | |
| success-foreground | | | |
| warning | | | |
| warning-foreground | | | |
| info | | | |
| info-foreground | | | |

## Typography

| Role | Family | Size | Weight | Line height |
|---|---|---|---|---|
| Heading 1 | | | | |
| Heading 2 | | | | |
| Heading 3 | | | | |
| Body | | | | |
| Body small | | | | |
| Label | | | | |
| Caption | | | | |
| Code | | | | |

## Spacing
- Base unit:
- Scale:
- Component padding:
- Section gap:

## Layout
- Grid:
- Breakpoints:
- Container max width:
- Page padding mobile:
- Page padding desktop:

## Shape
- Border radius default:
- Border radius scale:
- Elevation style:

## Navigation
- Pattern:
- Sidebar width:
- Mobile navigation:
- Active state style:

## Interaction patterns

### Loading
- Approach: (skeleton / spinner / both)
- Button loading:

### Empty states
- Structure:
- Tone:

### Error states
- Inline field:
- Form level:
- Page level:

### Success feedback
- Style:
- Toast duration:

### Modals
- Sizes:
- Overlay:
- Dismiss on overlay click:

### Toasts
- Position:
- Duration:

### Confirmation (destructive)
- Style:
- Require typing:

## Tone and voice
- Label casing:
- Button copy:
- Error tone:
- Help text:

## Icons
- Set:
- Sizes:
- Stroke weight:

## Accessibility
- WCAG level:
- Text contrast:
- UI contrast:
- Focus style:
- Reduced motion:

## Notes
```

---

### spec.personas

**Output:** one file per persona at `./spec/personas/[archetype-name].md` plus `./spec/personas/permissions.md`
**Reads:** `spec/brief.md`, `spec/value-proposition.md`

**Persona naming:**
Generate an alliterative name automatically from the role. The name combines a first name and a role descriptor that share the same starting letter. The name must feel natural in conversation — "what does Andy need here?" should be easy to say. File name is slugified from the archetype name: `andy-the-app-user.md`.

**Required fields per persona:**
- Composite summary — one short paragraph written as a character sketch
- Identity — actor group, access level, scope, archetype name, example role titles
- Context of use — primary device, secondary device, connectivity, physical environment, environmental constraints
- Technical profile — technical literacy, software familiarity, data entry comfort, map/spatial comfort, tolerance for complexity
- Time and attention — typical work hours, peak usage times, session duration, interruption frequency, time pressure, attention availability
- Decision authority — strategic, operational, financial, delegation patterns, approval dependencies
- Workday shape — morning, midday, afternoon, evening, seasonal variation
- Goals — primary job goal, secondary goals, success criteria, quality signals
- Frustrations — current pain points, legacy system complaints, workflow friction, information gaps
- Constraints for flow design — each constraint mapped to its design implication
- Jobs this persona performs — job ID, job statement, frequency, notes
- Scenarios involving this persona — scenario ID, name, role

Personas are operational, not demographic. Every attribute must have a direct implication for design or flow decisions.

**Two persona depths:**

**Light persona** — use for rapid prototyping or early-stage ideas. Six fields only:
- Archetype name (alliterative)
- Role and access level
- Primary device and connectivity
- Primary job — what they are trying to accomplish
- Key constraint — the single biggest thing that limits how they use the product
- What they distrust — what would make them abandon or ignore the product

Generate a light persona when the user says "quick", "fast", "draft", or when spec.brief indicates early-stage exploration. Ask: "Do you want a light persona (6 fields, fast) or a full persona (all sections)?"

**Full persona** — use when the product concept is established and the personas will drive flow and screen decisions across multiple sessions.

**Template (one file per persona):**
```markdown
# [Archetype name] — [Role]

> File: spec/personas/[archetype-name].md

**Composite summary:** [One short paragraph. Who this person is, how they move through their day, and their relationship to the product. Written as a character sketch, not a demographic profile.]

## Identity

| Attribute | Value |
|---|---|
| Actor group | |
| Access level | |
| Scope | |
| Archetype name | [e.g. Andy the App User] |
| Example role titles | |

## Context of Use

| Attribute | Value |
|---|---|
| Primary device | |
| Secondary device | |
| Typical connectivity | |
| Physical environment | |
| Environmental constraints | |

## Technical Profile

| Attribute | Value |
|---|---|
| Technical literacy | |
| Software familiarity | |
| Data entry comfort | |
| Map / spatial comfort | |
| Tolerance for complexity | |

## Time and Attention

| Attribute | Value |
|---|---|
| Typical work hours | |
| Peak usage times | |
| Session duration | |
| Interruption frequency | |
| Time pressure level | |
| Attention availability | |

## Decision Authority

| Attribute | Value |
|---|---|
| Strategic decisions | |
| Operational decisions | |
| Financial authority | |
| Delegation patterns | |
| Approval dependencies | |

## Workday Shape

| Period | Description |
|---|---|
| Morning | |
| Midday | |
| Afternoon | |
| Evening | |
| Seasonal variation | |

## Goals

| Goal type | Description |
|---|---|
| Primary job goal | |
| Secondary goals | |
| Success criteria | |
| Quality signals | |

## Frustrations

| Frustration type | Description |
|---|---|
| Current pain points | |
| Legacy system complaints | |
| Workflow friction | |
| Information gaps | |

## Constraints for Flow Design

| Constraint | Implication |
|---|---|
| Device constraint | |
| Connectivity constraint | |
| Time constraint | |
| Environment constraint | |
| Authority constraint | |
| Literacy constraint | |

## Jobs This Persona Performs

| Job ID | Job statement | Frequency | Notes |
|---|---|---|---|
| J-001 | | | |

## Scenarios Involving This Persona

| Scenario ID | Scenario name | Role in scenario |
|---|---|---|
| SC-01 | | |
```

**Template (light persona — rapid prototyping):**
```markdown
# [Archetype name] — [Role]

> File: spec/personas/[archetype-name].md
> Mode: Light persona

**[Archetype name]** is [one sentence: who they are and what they spend their day doing].

| Attribute | Value |
|---|---|
| Role | |
| Access level | |
| Primary device | |
| Connectivity | |
| Primary job | [What they are trying to accomplish with this product] |
| Key constraint | [The single biggest thing limiting how they use the product] |
| What they distrust | [What would make them abandon or ignore the product] |
```

**Template (permissions.md):**
```markdown
# Permission Matrix

> File: spec/personas/permissions.md

| Capability | [Andy the App User] | [Rachel the Receptionist] |
|---|---|---|
| [Feature or action] | ✓ | — |
| [Feature or action] | ✓ | ✓ |
| [Feature or action] | — | ✓ |
```

---

### spec.journeys

**Output:**
- `./spec/journeys/overview.md`
- `./spec/journeys/[persona]-[scenario].md` — one file per journey

**Reads:** `spec/personas/`

**File naming:** slugify persona archetype name + scenario description.
- `andy-discovers-the-app.md`
- `rachel-approves-a-request.md`
- `felix-logs-a-field-issue.md`

**Relationship to flows:** A journey is the "why and what" — the user's experience, their questions, their emotional stakes, and where friction exists. A flow is the "how" — the precise sequence of steps, branches, and system responses that fulfils that journey. Each journey file has one or more corresponding flow files in `spec/flows/` with the same base name. The flow elaborates the journey stages into precise steps; the overlap is intentional and expected.

**Required fields per journey:**
- Name, trigger, primary persona, end state
- Stages (3–7): each as prose (3–5 sentences) covering what the user is doing, what question is in their head, what they need to know, and what could go wrong
- Data needed and friction risk per stage
- Moments that matter — highest-stakes transitions where trust is built or broken
- Screens implied
- Flows that implement this journey (list of flow file names)

**Format:** Write stages as prose paragraphs, not table rows. No emotion column.

**Template (overview.md):**

> The Flow file(s) column starts empty when journeys are generated. spec.flows fills it in as each flow is written. Do not invent flow file names when generating this file.

```markdown
# Journey Overview

> File: spec/journeys/overview.md

| Journey file | Journey name | Persona | Flow file(s) | Trigger | End state |
|---|---|---|---|---|---|
| andy-discovers-the-app.md | | Andy the App User | — | | |
| rachel-approves-a-request.md | | Rachel the Receptionist | — | | |
```

**Template (per journey file):**
```markdown
# Journey: [Name]

> File: spec/journeys/[persona]-[scenario].md
> Persona: [Archetype name]
> Trigger: [What causes this journey to start]
> End state: [What success looks like for the user]

## Stages

### Stage 1: [Name]
[Prose: what the user is doing, what question is in their head, what they need to see, what could go wrong. 3–5 sentences.]

**Data needed:** [What information must be available at this stage]
**Friction risk:** [What could cause the user to stall or fail]

### Stage 2: [Name]
[Same structure. 3–7 stages total.]

## Moments that matter
- **[Stage or transition]:** [Why it matters and what is at stake]

## Screens implied

| Stage | Screen needed |
|---|---|
| [Stage name] | [Screen name] |
```

---

### spec.flows

**Output:** one file per flow at `./spec/flows/[persona]-[scenario].md`
**Reads:** `spec/personas/`, `spec/journeys/`

**File naming:** Match the journey file name where possible — one journey, one primary flow. Use a suffix for secondary flows on the same journey.
- `andy-discovers-the-app.md` — primary flow for that journey
- `andy-discovers-the-app-offline.md` — variant flow covering the offline branch
- `rachel-approves-a-request.md`

**Relationship to journeys:** A flow is the precise elaboration of a journey. Each flow maps its steps back to the journey stages they implement. The overlap in content is intentional — the journey captures intent and experience, the flow captures the exact sequence the system must support. A design agent reading both gets context (journey) and precision (flow).

User flows are expected to be large. Each flow should be complete.

**Required fields per flow:**
- Journey reference — which journey file this flow implements
- Trigger
- Entry point
- Preconditions
- Steps — numbered list, each step noting which journey stage it belongs to
- Decision points
- Branches — condition: consequence pairs
- Error paths
- Exit states
- Completion criteria

**After writing each flow file:** update `spec/journeys/overview.md` to add this flow's filename to the Flow file(s) column for the matching journey row. If the flow implements a journey not yet in the overview, add a new row with the journey file name and leave Trigger and End state blank for the user to fill in. This is the only place the journey-to-flow link is maintained — the journey file itself does not reference flows.

**Format rules:**
- Flat bullet list per flow, consistent named fields — no tables, no nested headers
- Steps numbered inline, annotated with journey stage reference where helpful
- Branches as condition: consequence on one line
- Keep each field scannable — one line where possible

**Template (per flow file):**
```markdown
# Flow: [Name]

> File: spec/flows/[persona]-[scenario].md
> Persona: [Archetype name]
> Journey: [Journey file name]
> Implements: [Journey stage names this flow covers, comma-separated]

- Trigger: [What causes this flow to start]
- Entry point: [Screen or state where the flow begins]
- Preconditions: [What must be true; separate with semicolons]
- Steps:
  1. [First action] ← [Journey stage name]
  2. [Second action]
  3. [Third action] ← [Journey stage name]
  4. [Continue for all steps]
- Decision points: [Comma-separated list of choices the user makes]
- Branches:
  - [Condition]: [Consequence]
  - [Condition]: [Consequence]
- Error paths: [Comma-separated list of specific failure conditions]
- Exit states: [All possible end states, separated by semicolons]
- Completion criteria: [One sentence defining what a completed flow looks like]
```

---

### spec.states

**Output:** `./spec/data/states.md`
**Reads:** `spec/flows/`, `spec/personas/`

**When to run:** After spec.flows, before spec.design and spec.screens. Screen specs reference states directly — the States table, Critical Variants, and the screen generation prompt all depend on knowing what states exist and what the user can do in each one. Running this before screens means the screen spec draws from a defined model rather than guessing.

**Required fields per object:**
- Object name
- All states with entry/exit conditions
- Transitions: from, trigger, to, actor, guard condition
- Actions per state: allowed / blocked (show disabled) / hidden
- Terminal states

**Distinguish precisely:**
- Allowed — available and enabled
- Blocked — exists but cannot be taken in this state (render disabled)
- Hidden — does not appear at all in this state

**Template:**
```markdown
# Workflow State Model

> File: spec/data/states.md

## Objects

| Object | States | Terminal states |
|---|---|---|

---

## Object: [Name]

### States

| ID | Name | Description | Entry | Exit |
|---|---|---|---|---|
| S1 | | | | |

### Transitions

| From | Trigger | To | Actor | Guard |
|---|---|---|---|---|

### Actions per state

| State | Allowed | Blocked | Hidden |
|---|---|---|---|

### Diagram

```
[S1: Name] --(trigger)--> [S2: Name]
[S2: Name] --(trigger)--> [S3: Name]
```

### Terminal states

| State | Description | UI behaviour |
|---|---|---|
```

---

### spec.decisions

**Note:** spec.decisions no longer exists as a standalone command. Decisions are now embedded within each screen file under the "Decisions on this screen" section, generated as part of spec.screens.

---

### spec.map

**Output:** `./spec/screens/inventory.md` and `./spec/screens/sitemap.md`
**Reads:** `spec/journeys/`, `spec/flows/`, `spec/personas/`

Collect all screens from each flow's exit states and steps, and each journey's screens-implied table. Deduplicate.

**Screen types:** List / Detail / Form / Dashboard / Empty / Error / Confirmation / Settings / Auth

**Priority:** Must-have / Should-have / Nice-to-have

Ask the user to confirm prototype scope before finalising.

After producing `inventory.md`, also produce `sitemap.md` showing the navigation structure — how screens connect to each other. The sitemap shows canonical screens only, not state variants. Each screen appears once regardless of how many states it has.

**Template:**
```markdown
# Screen Inventory

> File: spec/screens/inventory.md

Total: [N] | In scope: [N] | Out of scope: [N]

## Screen list

| ID | Name | Type | Priority | Scope | File | Flow | Persona | State |
|---|---|---|---|---|---|---|---|---|
| PG01 | | | Must-have | In | — | | | |

> The File column starts empty. spec.screens fills it in as each screen file is written.
> Route column comes from spec.screens too — leave blank initially.

---

## [Screen name] — PG01

**Type:** | **Priority:** | **Scope:** In / Out
**Flow:** [Flow file] | **Journey stage:** | **Persona:** [Archetype name] | **State shown:**

**Purpose:** [One sentence]
**Entry points:**
**Exit points:**
**Notes:**
```

**Template (sitemap.md):**
```markdown
# Sitemap

> File: spec/screens/sitemap.md
> Canonical screens only. State variants are not separate nodes.

## Entry points

- [Screen name] — [how users arrive: app launch, direct URL, invitation link, etc.]

## Screen connections

[One line per connection: from-screen → to-screen (trigger)]

- [screen-name] → [screen-name] ([what triggers the navigation])
- [screen-name] → [screen-name] ([trigger])

## Orphaned screens

[Screens with no inbound path — may need an entry point or are deep-link only.]

- [Screen name]: [note]
```

---

### spec.contracts

**Output:** `./spec/screens/contracts.md`
**Reads:** `spec/data/states.md`, `spec/screens/inventory.md`, `spec/personas/`

Produce contracts only for in-scope screens. Actions must be consistent with allowed/blocked/hidden rules in the state model.

**Template:**
```markdown
# Page Contracts

> File: spec/screens/contracts.md

---

## [Screen name] — PG01

**Flow:** [Flow file] | **Persona:** [Archetype name] | **State:** [Name]

### Purpose
[One sentence]

### Current state displayed
- Object shown:
- Status:
- What happened:
- What is pending:
- What needs attention:

### Primary decision
- Decision:
- Options:
- Information required:
- Risk if wrong:
- Reversible: Yes / No

### Secondary decisions

| Decision | Options | Info required |
|---|---|---|

### Available actions

| Action | Priority | Preconditions | Result | Next state | Next screen |
|---|---|---|---|---|---|
| | Primary | | | | |
| | Disabled | [condition] | — | — | — |

Priority key: Primary / Secondary / Destructive / Disabled / Hidden

### If the user does nothing

### Edge cases

| Scenario | Behaviour |
|---|---|
| Missing data | |
| Permission denied | |
| Conflicting state | |
| Action fails | |
| Session expired | |
```

---

### spec.data

**Output:** `./spec/data/requirements.md`
**Reads:** `spec/screens/contracts.md`, `spec/screens/inventory.md`, `spec/data/states.md`

For missing-data behaviour, be specific. "Show error" is insufficient — state whether the action is blocked, the field is hidden, a placeholder is shown, or the section degrades.

**Template:**
```markdown
# Data Requirements

> File: spec/data/requirements.md

## Data-to-decision matrix

| Decision | Data required | Display | Source | If missing |
|---|---|---|---|---|

---

## Per-screen data

### [Screen] — PG01

| Field | Type | Source | Required | Display | If missing |
|---|---|---|---|---|---|

---

## Shared objects

### [Object]

| Field | Type | Nullable | Description |
|---|---|---|---|
| id | string | No | |
| status | enum | No | See states.md |

---

## Loading states

| Screen | Loading | Empty | Error | Partial |
|---|---|---|---|---|
| PG01 | Skeleton | Empty state | Error banner | |
```

---

### spec.actions

**Output:** `./spec/actions.md`
**Reads:** `spec/data/states.md`, `spec/screens/contracts.md`, `spec/personas/`

Produce one full spec per action. Do not collapse similar actions across pages.

**Action types:** Progression / Modification / Investigation / Recovery / Delegation / Exit

**Template:**
```markdown
# Action Specifications

> File: spec/actions.md

## Index

| ID | Action | Type | Page | Persona | Priority |
|---|---|---|---|---|---|
| A01 | | | | | |

---

## [Action] — A01

**Type:** | **Page:** [ID] | **Flow:** [Flow file] | **Persona:** [Archetype name] | **Priority:**

### Intent
[One sentence]

### Preconditions
-

### Permission rules
- Allowed for:
- Blocked for:
- Conditions:

### Validation

| Rule | Message | Behaviour |
|---|---|---|

### Confirmation
- Required: Yes / No
- Style: Modal / Popover / None
- Heading:
- Body:
- Confirm label:
- Cancel label:
- Destructive: Yes / No

### Result

### State transition
[Object] from [State] → [State]

### Destination

### Success feedback
- Style:
- Copy:

### Error feedback
- Style:
- Copy:
- Recovery:

### Undo
- Available: Yes / No
- Window:
- Trigger:

### Analytics
- Event:
- Properties:
```

---

### spec.screens

**Output:** one file per screen at `./spec/screens/[screen-name].md`
**File naming:** slugify the screen name from the inventory. Examples: `map-workspace.md`, `mob-movement.md`, `login.md`, `dashboard.md`
**Reads:** `spec/design.md`, `spec/screens/inventory.md`, `spec/data/states.md`, `spec/journeys/`, `spec/flows/`
**Also reads if present:** `spec/screens/contracts.md`, `spec/data/requirements.md`, `spec/actions.md`

Generate one file per in-scope screen from the inventory. Do not produce a combined file. After writing all screen files, update `spec/screens/inventory.md` to fill in the File column for each screen.

Each screen spec describes what the screen must do and how it must behave — not which components to use or how to implement it. The implementing agent makes those choices. Specifications that name specific libraries or prescribe exact implementations reduce the agent's ability to make good decisions.

Each screen ends with a **screen generation prompt** — one dense paragraph that gives the implementing agent everything it needs: who uses the screen, what they are trying to do, what must be visible, how it must behave, and what constraints apply. The prompt describes intent and behaviour, not implementation choices.

If the user has specified a target tool inline with the command, note it in the prompt header. Otherwise ask: "Which tool are you generating for — v0, Lovable, Bolt, Claude Design, Google Stitch, or other?"

**Template:**
```markdown
# [Screen name]

> File: spec/screens/[screen-name].md
> Screen ID: PG01

| Attribute | Value |
|---|---|
| Route | `/[path]` |
| Access | [ROLE, ROLE] |
| Mobile | Critical / Supported / Not required |
| Scope | In scope / Out of scope |

### Design intent

- Primary purpose: [What this screen exists to do — one sentence]
- Primary actors: [Which personas use this screen]
- Primary job: [The core task the user is trying to complete]
- Principle served: [Which design principle from spec/value-proposition.md this screen most directly expresses]
- Entry points: [How users arrive here]
- Success outcome: [What a completed interaction looks like]
- Content priority: Primary: [what must be immediately visible]. Secondary: [what supports but is not critical]
- Critical variants: [Named rendering modes this screen must support, e.g. offline mode, empty state, permission-restricted view]
- Exit paths: [Where users go after completing or abandoning the task]

### Related flows

- [Flow file name]: [One-line description of the relationship]

### Decisions on this screen

Document decisions with meaningful consequences — approve, submit, delete, assign, configure, move. Not navigation choices. Write "None" if there are no consequential decisions.

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | | High/Med/Low | Yes / No |

For each Medium or High stakes decision:

**[Decision name] — D1**
- What the user is deciding: [Plain language]
- Options: [Choices available and what each means]
- Information required: [What must be visible to decide confidently]
- Risk if wrong: [Consequence]
- Reversible: Yes / No — Recovery: [How to undo or correct]
- Recommended default: [Safe option if one exists]
- Conditions that change this: [State or permission that changes options]

### States

| State | Description | Transitions |
|---|---|---|
| `loading` | | |
| `ready` | | |
| `error` | | |

### Actions

| Action | Description | Precondition |
|---|---|---|
| [Action name] | | Always |
| [Action name] | | [Condition] |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| [Data] | Server | Cached | |
| [Data] | Device | Real-time | |
| [Data] | Local | Local | |
| [Data] | Input | N/A | |

**Offline key:** Cached = available offline / Local = device only / Real-time = requires connection / N/A = derived or user input

### Visual indicators

| Indicator | Meaning |
|---|---|
| [Symbol or colour] | [What it communicates] |

### Critical variants

**[Variant name]**
[What triggers this variant, what changes, and what the user can do in this mode]

### Layout regions

Describe the functional regions of this screen without specifying components or implementation. What must be present and what purpose does each region serve.

| Region | Purpose | Required content |
|---|---|---|
| [Region name] | [What it does for the user] | [What it must show] |

### Content

| Element | Copy |
|---|---|
| Page title | |
| Primary action label | |
| Empty state heading | |
| Empty state body | |
| Empty state CTA | |
| Error heading | |
| Error body | |
| Offline message | |

### Screen generation prompt

> Target tool: [v0 / Lovable / Bolt / Claude Design / Google Stitch / other]

[One dense paragraph. Describe who uses this screen and what they are trying to do. Describe what must be visible and how the screen must behave — not which components to use. Cover the primary action, critical variants, and how the screen handles offline, empty, and error states. Ground every requirement in the persona's context and constraints. No bullet points. No component or library names.]

### Acceptance criteria

Generate from the design intent, critical variants, and persona constraints above. Keep to 5–8 criteria. The persona-job criterion is always first. All criteria must be verifiable by looking at the generated screen — nothing that requires running code.

- [ ] [Persona archetype name] can [primary job] without scrolling on a standard phone screen
- [ ] Primary action is the most visually prominent interactive element
- [ ] [Each critical variant] renders as a visually distinct state
- [ ] Empty state is shown when [condition] and includes [CTA label]
- [ ] Error state is shown when [condition] and explains what went wrong
- [ ] [Offline indicator] is visible when connectivity is unavailable (if applicable)
- [ ] All content uses realistic mock data — no placeholder text

### Heuristic check

For each item mark ✓ (defined), ✗ (missing — flag as gap), or N/A (not applicable).

**1. Visibility of system status** — does every action and state transition have defined feedback?
- [ ] Every action has defined success feedback
- [ ] Every action has defined error feedback and recovery path
- [ ] All loading and saving states are described

**2. Match between system and real world** — does the content use [persona]'s vocabulary, not internal terms?
- [ ] Labels, actions, and copy use the persona's domain language
- [ ] No unexplained internal terminology

**3. User control and freedom** — can the user exit or undo anything consequential?
- [ ] Every destructive action has a cancel or confirmation path
- [ ] The user can return from every exit point listed

**4. Error prevention** — does the spec prevent errors, not just handle them?
- [ ] High-stakes actions have preconditions or confirmation before executing
- [ ] The spec does not rely solely on error states to handle foreseeable bad inputs

**5. Error recovery** — are error states specific enough to produce useful feedback?
- [ ] Every error state names the condition, the message, and the recovery path
- [ ] No error state is described only as "show error"

**6. Flexibility and efficiency** — given [persona]'s session length and constraints, is the path efficient?
- [ ] The primary path fits within the persona's typical session duration
- [ ] No mandatory steps that don't serve the primary job

**Heuristic gaps:**
[List failing items with what is missing. Write "None" if all pass.]
```

---

### spec.prepare

**Output:** `./spec/prepare-brief.md`
**Reads:** `spec/design.md`, `spec/screens/inventory.md`, `spec/screens/sitemap.md`, `spec/screens/[screen-name].md` (all in-scope screen files), `spec/flows/`
**Also reads if present:** `spec/screens/contracts.md`

Self-contained alongside `spec/design.md`. A generation tool with only these two files should be able to generate the prototype without reading anything else.

For mock data: use realistic specific values. "Jane Okafor, Meridian Publishing, 14 titles, Active" not "[Name], [Company], [Count], [Status]".

**Template:**
```markdown
# Prototype Build Brief

> File: spec/prepare-brief.md
> Self-contained generation brief for v0, Lovable, Bolt, Claude Design, Google Stitch. Use alongside spec/design.md.

## Overview

**Product:**
**Goal:** [What someone should be able to do]
**Fidelity:** Lo-fi / Mid-fi / Hi-fi
**Target tool:** v0 / Lovable / Bolt / Claude Design / Google Stitch / other
**Primary flow:** [Flow file name]
**Screens:** [Page IDs]

## Design system summary

- Framework:
- Component library + version:
- Styling:
- Dark mode:
- Navigation:
- Primary color:
- Border radius:
- Font:

(Full detail in spec/design.md)

## Screens to build

| Order | ID | Name | Spec file | Entry? | Notes |
|---|---|---|---|---|---|
| 1 | PG01 | | spec/screens/[screen-name].md | Yes | |

## Core flow

1. User at [PG01] — [what they see]
2. [Action] → [PG02]
3. [Continue]

## Mock data

### [Object]

| Field | Value |
|---|---|

## States to show

| Screen | State | How |
|---|---|---|
| PG01 | Loaded | Default |
| PG02 | Empty | Zero records |

## Implement

| From | Trigger | To |
|---|---|---|

## Stub (appear but not function)
- [Action] on [PG03]: toast only
- [Action] on [PG04]: disabled

## Edge cases

| Case | Screen | How |
|---|---|---|

## Do not
- Generate screens not in the inventory
- Use components not in spec/design.md
- Use lorem ipsum
- Add navigation not in the design context

## Acceptance
- [ ] All screens render
- [ ] Primary flow completes end to end
- [ ] Visual style is consistent with spec/design.md requirements
- [ ] Empty, loading, and error states present on each screen
- [ ] Mobile layout correct at sm breakpoint
- [ ] Realistic mock data throughout
```

---

### spec.generate

**Output:** stdout only — no file is written to disk
**Usage:** `spec.generate [screen-name]` e.g. `spec.generate map-workspace`
**Reads:** `spec/design.md`, `spec/screens/[screen-name].md`, `spec/prepare-brief.md`
**Also reads if present:** `spec/screens/sitemap.md`, `spec/patterns.md`

Packages the context from three or four files into a single ready-to-paste generation prompt for a named screen. The output is formatted to paste directly into Claude Design, Google Stitch, v0, Lovable, Bolt, or any other UI generation tool.

**Behaviour:**
- Does not follow the standard step pattern — no input question, no assumptions, no stop phrase
- Reads the named screen file and the supporting files, then immediately produces the prompt
- If the screen file does not exist, list the available screens from `spec/screens/inventory.md` and ask which one to generate
- If `spec/prepare-brief.md` does not exist, generate the prompt without it and note that mock data is unavailable
- If `spec/screens/sitemap.md` does not exist, generate the prompt without navigation context

**Output format:**

Print the prompt directly — no preamble, no explanation. Just the prompt, ready to copy.

The prompt has four sections assembled from the source files:

**Section 1 — Product and design context** (from design.md, and patterns.md if present)
A short paragraph covering: what the product is, who uses it, the visual tone, platform and environment, and the key interaction and accessibility requirements. Keep it to 3–5 sentences. If patterns.md exists, append a short "Patterns to follow" paragraph with the relevant patterns and anti-patterns for this screen type — omit patterns that don't apply.

**Section 2 — Screen context** (from sitemap.md if present)
One or two sentences covering where this screen sits: what leads to it and where it goes. Derived from the Screen connections list in sitemap.md. Omit if sitemap.md is not present.

**Section 3 — Screen specification** (from [screen-name].md)
The full design intent, states, actions, data, variants, layout regions, and content table from the screen file. Paste these sections in full — do not summarise. The generation prompt section from the screen file becomes the closing instruction paragraph.

**Section 4 — Mock data** (from prepare-brief.md)
The relevant mock data object(s) for this screen. Extract only the data that applies to this screen — do not include the full brief. One or two sentences on fidelity target and target tool if specified.

**Separator between sections:** `---`

**After the prompt**, on a new line, print:
```
--- copied prompt ends here ---
Generating: [screen-name] | Tool: [target tool or "not specified"] | Files used: design.md, [screen-name].md[, prepare-brief.md][, sitemap.md]
```

This trailer is for the user's reference and should not be included when pasting into the generation tool.

---

### spec.change

**Output:** updates affected files in place — no new files created
**Usage:** `spec.change [description of change or new feature]`
**Reads:** all existing files in `./spec/`

Ingests a change description or new feature request, analyses which existing artifacts are affected, confirms the impact list with the user, then updates only the affected files in dependency order.

This command does not follow the standard step pattern. It has its own four-step process.

---

#### Step 1 — Accept the change description

The change description comes inline with the command:

```
spec.change add offline support for the mob movement screen
spec.change new persona: farm owner who reviews weekly reports
spec.change the source connection flow needs a QR code scan option
spec.change rename "paddock" to "block" throughout
```

If the user runs `spec.change` with no description, ask:
> What is changing? Describe the feature, fix, or addition — or paste a requirements document, ticket, or notes.

Wait for a response before proceeding.

---

#### Step 2 — Impact analysis

Read all files in `./spec/` and determine which artifacts are affected by the change. Reason through the dependency chain — a change to a persona may affect journeys, flows, and screens. A new flow may affect the sitemap, inventory, and screen files.

For each affected file, identify specifically what needs to change and why.

Print the impact analysis in this format — do not update any files yet:

```
─────────────────────────────────────────────
  spec.change impact analysis
─────────────────────────────────────────────
  Change: [description]

  Files to update:
  ✎ spec/[file]        [what changes and why]
  ✎ spec/[file]        [what changes and why]

  Files not affected:
  ✓ spec/[file]
  ✓ spec/[file]

  New files to create:
  + spec/[file]        [what it is]

─────────────────────────────────────────────
  [N] files to update, [N] to create

  Reply "yes" to proceed, adjust the list, or "cancel".
─────────────────────────────────────────────
```

Stop and wait for the user's response before writing anything.

---

#### Step 3 — Apply updates

If the user replies "yes" or "proceed", update files in dependency order:

1. Brief and value proposition (if affected)
2. Design (if affected)
3. Personas (if affected)
4. Journeys (if affected)
5. Flows (if affected)
6. States (if affected)
7. Screens — inventory, sitemap, then individual screen files (if affected)
8. Prepare brief (if affected)

If the user adjusts the list (removes or adds files), honour their changes before proceeding.

For each file updated, add a change log entry as the first line of the file, immediately after any existing provisional notice:

```
> 📝 Changed: [brief description of what changed] — spec.change "[original input]"
```

If a file already has change log entries, prepend the new one so the most recent is always first.

Do not regenerate files from scratch. Make targeted edits — preserve all existing content that is not affected by the change.

Print progress as each file is updated:

```
✎ spec/flows/aroha-moves-mob.md       updated — offline branch added
✎ spec/screens/mob-movement.md        updated — offline variant and queued state added
+ spec/screens/mob-movement-offline.md created
✓ spec/screens/sitemap.md             updated — new screen connection added
```

---

#### Step 4 — Summary and stop

After all updates are complete, print:

```
─────────────────────────────────────────────
  spec.change complete
─────────────────────────────────────────────
  [N] files updated, [N] files created

  Changed files:
  - spec/[file]
  - spec/[file]

  Run spec.generate [screen-name] to produce a
  generation prompt for any updated screens.
  Run spec.review to check overall readiness.
─────────────────────────────────────────────
```

---

#### Edge cases

**Rename or terminology change** — if the change is a rename (e.g. "paddock" → "block"), perform a targeted find-and-replace across all affected files. List every file touched in the impact analysis. Confirm before executing — renames touch many files.

**Conflicting changes** — if the requested change contradicts something in an existing artifact, flag the conflict in the impact analysis rather than silently overwriting. Example: "This change adds an offline queue state to mob-movement, but spec/data/states.md currently defines mob-movement as having no offline states. Updating states.md will change the allowed actions table — confirm this is intended."

**New persona** — if the change adds a new persona, run the full persona generation flow (using the medium persona template) for that persona, then identify which existing flows and screens the new persona participates in and update those.

**New screen** — if the change requires a new screen, add it to the inventory, update the sitemap, and generate the new screen file. Do not generate a screen spec for screens that are out of scope unless the change explicitly brings them into scope.

**Change affects the prepare brief** — always check whether mock data, the core flow, or the screen list in prepare-brief.md needs updating. This is easy to miss but important for generation quality.

**Change introduces a new interaction pattern** — if the change adds a new interaction type, flag in the impact analysis whether patterns.md exists and whether it needs a new entry. Do not create patterns.md if it doesn't exist — suggest the user run spec.patterns instead.

---

### spec.patterns

**Output:** `./spec/patterns.md`
**Reads:** `spec/design.md`, `spec/screens/*.md`, user input
**Optional:** yes — skippable. Run after generating screens when you have real feedback, not speculatively upfront.

`spec/patterns.md` is a behavioural pattern library — named, reusable design decisions and constraints that apply across the product. It is not a component catalogue. It describes intent and constraints, not implementation. Generation tools use it to make consistent decisions across screens.

**When to run:**
- After generating the first few screens, when you can see what worked and what did not
- When a recurring interaction has been decided and should be applied consistently
- When spec.change flags a new interaction type that needs a pattern defining
- Not before generating screens — patterns written speculatively before seeing real output tend to be generic

**Three modes:**

**Mode 1 — Start from scratch** (default when no patterns.md exists)
Ask: Do you have generation feedback, screen outputs, or interaction decisions to capture? Paste them, or say "nothing" to create an empty patterns file with instructions for when to return.
If feedback provided: extract patterns from it.
If "nothing": create a minimal patterns.md with structure in place and a note to return after first screens.

**Mode 2 — Add a pattern**
Usage: `spec.patterns add [description]`
Adds one new entry to patterns.md. Prompts for: name, when to use, when not to use, what it must achieve, anti-pattern statement.

**Mode 3 — Review and refine**
Usage: `spec.patterns review`
Reads patterns.md and all screen specs. Checks for: patterns no longer reflected in screen specs, recurring screen spec decisions not yet captured as patterns, contradictions between patterns. Produces a short review with suggestions.

**Required fields per pattern:**
- Pattern name
- What it is — one sentence
- When to use — specific conditions
- When not to use — explicit exclusions
- What it must achieve — the behavioural outcome
- Anti-pattern — what to avoid and why

**Template:**
```markdown
# Pattern Library

> File: spec/patterns.md
> Behavioural patterns — intent and constraints, not implementation.
> Grows from real generation feedback. Add patterns when you see recurring decisions, not speculatively.

## Patterns

### [Pattern name]
**What it is:** [One sentence]
**When to use:** [Specific conditions]
**When not to use:** [Explicit exclusions]
**Must achieve:** [Behavioural outcome — what the user must be able to do or understand]
**Anti-pattern:** [What to avoid and why]

---

[Repeat for each pattern]

## Anti-patterns

Things this product must never do, regardless of context.

- [Anti-pattern]: [Why]

## Open questions

Recurring decisions not yet resolved into a pattern.

- [Decision]: [What is unresolved and why it matters]
```

---

### spec.review

**Output:** `./spec/readiness-review.md`
**Reads:** all files in `./spec/`

No external input needed. Read all files and run the checks below.

**Checks to run:**
- Traceability: for each in-scope screen verify `Screen → Component → Action → Decision → Data → Flow → Journey → Persona → Product Goal`
- Component coverage: every component in screen files exists in design.md
- Action coverage: every action in contracts.md has a spec in actions.md
- State coverage: every state in contracts.md is defined in data/states.md
- Data coverage: every field in screen files appears in data/requirements.md
- Scope consistency: every screen file in screens/ is marked in-scope in inventory.md
- Persona coverage: every flow references a persona file in personas/

**Score 1–5:** 1 = not started / 2 = major gaps / 3 = adequate / 4 = strong / 5 = complete / N/A = intentionally skipped

**Before scoring:** check which optional artifacts exist. Mark spec.states, spec.data, and spec.actions as N/A if they were intentionally skipped for rapid prototyping — do not score them as gaps. Only flag them as missing if the prototype cannot function without them.

**Dimensions:** product clarity, user clarity, persona usefulness, journey completeness, flow completeness, design system completeness, screen completeness, prototype readiness.

**Engineering handoff dimensions (only score if those artifacts exist):** state coverage, decision clarity, data completeness, action completeness.

**Categorise issues:** Critical (blocks prototype build) / Important (affects quality or correctness) / Minor (polish or completeness).

**Template:**
```markdown
# Readiness Review

> File: spec/readiness-review.md

## Scores

| Dimension | Score | Notes |
|---|---|---|
| Product clarity | /5 | |
| User clarity | /5 | |
| Persona usefulness | /5 | |
| Journey completeness | /5 | |
| Flow completeness | /5 | |
| Design system completeness | /5 | |
| Screen completeness | /5 | |
| **Prototype readiness** | /5 | |

**Engineering handoff dimensions (score N/A if artifacts were intentionally skipped)**

| Dimension | Score | Notes |
|---|---|---|
| State coverage | /5 or N/A | |
| Decision clarity | /5 or N/A | |
| Data completeness | /5 or N/A | |
| Action completeness | /5 or N/A | |

**Verdict:** Ready / Needs fixes / Not ready

## Traceability

| Screen | Complete | Break point |
|---|---|---|
| PG01 | ✓ | |

## Critical gaps
- [ ] [Gap] — [file] — fix: [action]

## Important issues
- [ ] [Issue] — [file]

## Minor issues
- [ ] [Issue]

## Missing components
- [Component] — used in [Screen] — not in spec/design.md

## Risky assumptions

| Assumption | File | Risk |
|---|---|---|

## Next actions
1.
2.

## Open questions
1.
```

---

---

### spec.sprint

**Output:** All files from the 5-step condensed prototype flow
**Runs:** spec.brief + spec.personas → spec.journeys + spec.flows → spec.states → spec.design → spec.map + spec.screens + spec.prepare

spec.sprint groups the 10 prototype commands into 5 steps. Each step asks for input once, runs its grouped commands, outputs all files for that step, then pauses for review before continuing. The standard checkpoint pattern applies within each step — validate, draft or complete, list assumptions, ask about gaps — but runs across all commands in the group before pausing.

When running grouped commands within a step:
- Ask for input once at the start of the step, not once per command
- If the input clearly covers one command but not another, draft the uncovered command from context and mark it provisional
- Output all files for the step, then list all assumptions across the group together
- Ask only the most important gap questions across the group — not one round per command
- End each step (except the last) with the standard stop phrase plus the next step preview

**Step 1 — Foundation**
Runs: spec.brief + spec.personas (full depth)
Input question: Do you have a product brief, PRD, or idea to share? Paste it, give a file path, or say "nothing."
Outputs: `spec/brief.md`, `spec/personas/[name].md`, `spec/personas/permissions.md`
Stop phrase: "Step 1 complete. Reply with corrections or 'accept' to continue to Step 2 — journeys and flows."

**Step 2 — User understanding**
Runs: spec.journeys + spec.flows
Input question: Do you have existing journey maps or flow documentation? Share a file path, paste content, or say "nothing."
Outputs: `spec/journeys/overview.md`, `spec/journeys/[persona]-[scenario].md` (one per journey), `spec/flows/[persona]-[scenario].md` (one per flow)
Note: After writing flow files, update overview.md with flow references as per the standard spec.flows rule.
Stop phrase: "Step 2 complete. Reply with corrections or 'accept' to continue to Step 3 — states and decisions."

**Step 3 — Structure**
Runs: spec.states
Input question: Do you have an existing state model or workflow documentation? Share a file path, paste content, or say "nothing."
Outputs: `spec/data/states.md`
Stop phrase: "Step 3 complete. Reply with corrections or 'accept' to continue to Step 4 — design system."

**Step 4 — Design**
Runs: spec.design
Input question: Do you have an existing design system, design.md, or component library to reference? Share a file path, paste content, or say "nothing."
Outputs: `spec/design.md`
Stop phrase: "Step 4 complete. Reply with corrections or 'accept' to continue to Step 5 — screens and build."

**Step 5 — Screens and handoff**
Runs: spec.map + spec.screens + spec.prepare
Input question: Do you have an existing screen list, screen specs, or prototype brief? Share a file path, paste content, or say "nothing." Also: which tool are you generating for — v0, Lovable, Bolt, Claude Design, Google Stitch, or other?
Outputs: `spec/screens/inventory.md`, `spec/screens/sitemap.md`, `spec/screens/[screen-name].md` (one per screen), `spec/prepare-brief.md`
Stop phrase: "Sprint complete. Your prototype spec is ready in ./spec/ — run spec.prepare to generate the preparation brief, or spec.review for a readiness check."

---

### spec.oneshot

**Output:** All prototype artifact files in one pass
**Reads:** Single input provided inline or at the start

spec.oneshot takes one input — a product idea, PRD, notes, or file — and generates the complete prototype artifact chain without pausing. It makes all assumptions, marks every output as provisional, and collects all clarification questions at the end.

**Behaviour rules:**
- Ask for input once at the very start. Do not pause again until all artifacts are written.
- Use full personas (not light mode) — a one-shot run needs maximum context for screen generation.
- Mark every output file with the provisional notice (the standard Step 6 rule applies to all files).
- Generate artifacts in the correct dependency order: brief → personas → journeys → flows → states → decisions → design → pages → screens → build.
- After all files are written, print a consolidated list of all assumptions grouped by artifact, followed by all clarification questions grouped by artifact.
- The clarification questions section should be actionable — each question names the artifact and field, explains why it matters, and suggests a default if the user wants to accept without answering.

**Input question (asked once):**
> Share your product idea, PRD, or notes — paste content directly, give a file path, or give me a URL. Also tell me which tool you're generating for (v0, Lovable, Bolt, Claude Design, Google Stitch, or other) and whether you have an existing design system to reference. If you have nothing on design, I'll draft defaults.

**Output order:**
1. `spec/brief.md`
2. `spec/personas/[name].md` (one per persona) + `spec/personas/permissions.md`
3. `spec/journeys/overview.md` + `spec/journeys/[persona]-[scenario].md` (one per journey)
4. `spec/flows/[persona]-[scenario].md` (one per flow) — update overview.md with flow references
5. `spec/data/states.md`
7. `spec/design.md`
8. `spec/screens/inventory.md`
9. `spec/screens/[screen-name].md` (one per screen)
10. `spec/prepare-brief.md`

**End of output:**
Print a summary in this format:

```
─────────────────────────────────────────────
  spec.oneshot complete — 10 files written
─────────────────────────────────────────────

All outputs are provisional. Review before treating as authoritative.

Assumptions by artifact:
  brief.md — [list]
  personas/ — [list]
  ...

Clarification questions:
  1. [Artifact: field] — [question] — default if accepted: [default]
  2. ...

Run spec.status to see all files. Run spec.review for a readiness check.
─────────────────────────────────────────────
```

---

## AGENTS.md snippet

Add this to your project's `AGENTS.md` to activate spec commands automatically:

```markdown
Use the skill at ~/.codex/skills/product-spec-builder/SKILL.md

All spec.* commands are available. Outputs go into ./spec/
```
