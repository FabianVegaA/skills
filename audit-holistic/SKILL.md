---
name: audit-holistic
description: Holistic audit of computational artifacts — verifies not only that code is built correctly, but that it is the right thing, correctly specified, and conceptually sound. Orchestrates Philosopher → Mathematician → Engineer perspectives with adversarial pre-mortem.
---

Audit a computational artifact holistically: verify it correctly implements its specification (implementation → spec), that the specification fulfills the intention (spec → intention), and that the design is conceptually sound, formally valid, and practically survivable.

> **Relationship to `audit-code`**: `audit-holistic` answers a prior question — *is this the right thing?* — before `audit-code` asks *is it built right?* The holistic audit runs first. If it passes, `audit-code` applies at the implementation level. The Engineer phase of this skill incorporates `audit-code`'s checklist as its implementation sub-step.

---

## Input Format

```
Artifact:       [code / module / system — paste or reference]
Specification:  [what it should do — informal prose is fine]
W (World):      [domain facts and environment assumptions]
R (Requirements): [functional + non-functional + constraints]
Risk Profile:   Standard | Critical | Mission-Critical  (default: Standard)
```

**W and R are required.** Specification is required. If any of these are missing or contradictory → HALT immediately (see HALT Protocol). Do not invoke any sub-agent.

---

## HALT Protocol

If W, R, or Specification is missing, underspecified, or contradictory:

1. DO NOT invoke any sub-agent.
2. Surface to the user in plain language:

```
HALT: [plain-language description of the problem]
Gap:  [specific missing or contradictory element]
To proceed: [concrete action the user should take]
```

**Examples:**

```
HALT: The specification states orders are immutable after creation, but the
      World assumes orders can be cancelled. These cannot both be true.
Gap:  W (World) — mutability contract for Order is undefined.
To proceed: Clarify whether Order.cancel() creates a new CancelledOrder entity
            or mutates the existing Order. Then re-invoke.
```

```
HALT: No specification provided. Correctness cannot be assessed without knowing
      what "correct" means for this artifact.
Gap:  Specification — missing entirely.
To proceed: Describe what this module is supposed to do, even informally.
```

---

## Levels of Abstraction

Every computational artifact exists at a level:

1. **Intention** — What problem are we solving?
2. **Specification** — What should the artifact do?
3. **Implementation** — How does it actually work?

`audit-holistic` audits all three levels. Audit at the right level — code should correctly implement its specification, not redefine it.

---

## Invocation Depth

| Risk Profile | Lenses | Pre-mortem |
|---|---|---|
| **Standard** | Philosopher (phases 1–2) + Engineer (phases 1–2) | No |
| **Critical** | All three lenses, all 4 phases each | Yes |
| **Mission-Critical** | All three lenses, all 4 phases + extended safety case | Yes |

Standard legitimately skips @mathematician and pre-mortem — this is not a violation.

---

## Execution Sequence

### Step 1 — Validate input

Check W, R, and Specification. If any are missing or contradictory → HALT. Otherwise determine Risk Profile.

### Step 2 — Identify artifact context

```bash
git diff --name-only          # modified files
```

Determine abstraction level of each modified file: is this spec, implementation, or intent?

### Step 3 — Invoke sub-agents in sequence

Pass prior outputs as context to each subsequent agent. Wait for each output before proceeding.

```
@philosopher  Analyze from conceptual foundations using VT + Abduction + KADS + Means-Ends.
              Phases 1–2 only if Standard.
              Artifact: [artifact]  Specification: [spec]  WRSPM: [W, R]

@mathematician  Validate formal properties using CSP + Natural Deduction + Tableaux + Hoare.
                [Skip if Standard]
                Artifact: [artifact]  Specification: [spec]  WRSPM: [W, R]
                Philosopher: [output]

@engineer  Assess feasibility using CBR + AHP + TRIZ + Six Thinking Hats.
           Apply audit-code checklist in Phase 1 (CBR) as the implementation sub-step.
           Artifact: [artifact]  Specification: [spec]  WRSPM: [W, R]
           Philosopher: [output]  Mathematician: [output or N/A]

@engineer pre-mortem  Assume this artifact is in production and has failed.
                      Identify top 5 failure modes adversarially.
                      DO NOT reference constructive analysis. Treat design as a black box.
                      [Skip if Standard]
```

### Step 4 — Spot-check each agent output

Before proceeding to synthesis, verify:

| Agent | Required section |
|---|---|
| Philosopher | "Synthesis" present |
| Mathematician | "Invariants Identified" present |
| Engineer | "Trade-offs" present |
| Engineer pre-mortem | "Top Failure Modes" present |

If any spot-check fails → reject and request revision before continuing.

### Step 5 — Conflict detection

Check all three before referee phase:

- **Type system disagreement** — Mathematician's type definitions conflict with Engineer's implementation model
- **Ontological mismatch** — Philosopher's entities don't map to Engineer's data structures
- **Feasibility objection** — Engineer raises concerns about Mathematician's invariants or formal properties

If any match → trigger referee phase (TRIZ + PMI + Six Thinking Hats). Apply veto authority table.

### Step 6 — Synthesize holistic audit report

---

## Output Format

### Design Gap Summary

Plain-language paragraph naming structural issues that cannot be expressed as line-level findings. Written in the style of the HALT protocol — specific, concrete, actionable.

```
[Design gap summary — 2–4 sentences. Name the gap, explain why it matters,
suggest one concrete resolution path. If no structural gaps: "No structural
gaps detected."]
```

### Per-Finding Audit

Each finding carries a lens tag so origin is clear:

```
[🔴/🟠/🟡/🟢] [Lens] <file>:<line>
- Finding:   ...
- Evidence:  [citation — e.g. Philosopher:Phase2:Abduction]
- Suggestion: ...
```

**Lens tags:**

| Tag | Origin |
|---|---|
| `[Philosopher]` | Ontology, semantics, conceptual coherence |
| `[Mathematician]` | Type system, invariants, formal properties |
| `[Engineer]` | Implementation, feasibility, operations |
| `[PreMortem]` | Adversarial failure mode (Critical/Mission-Critical only) |
| `[Implementation]` | audit-code checklist finding (via Engineer phase) |

### Holistic Analysis

```
Philosopher:   [specific claim]  [Philosopher:PhaseN:MethodName]
Mathematician: [specific claim]  [Mathematician:PhaseN:MethodName]
Engineer:      [specific claim]  [Engineer:PhaseN:MethodName]
Pre-mortem:    [top failure modes]  [Engineer:PreMortem]   ← Critical/Mission-Critical only
```

### Conflict Detection

```
Conflict detection: type system | ontology | feasibility | none
Referee resolution: [TRIZ + PMI analysis + veto applied — or "N/A"]
```

### Confidence

```
Confidence: High | Medium | Low
Rationale:  [one sentence]
```

---

## Severity

| Level | Description |
|---|---|
| 🔴 Critical | Security vulnerability, data loss, breakage, incorrect results, formal contradiction |
| 🟠 High | Performance issue, maintainability blocker, type unsafety, unprovable invariant |
| 🟡 Medium | Ontological confusion, code smell, improvement opportunity |
| 🟢 Low | Style preference, naming nitpick |

---

## Lenses

### Philosopher (VT + Abduction + KADS + Means-Ends)

Audits whether the artifact's entities, names, relationships, and boundaries map coherently to domain concepts.

Key questions:
- Do the module names reflect real domain roles (agent, object, transfer, transaction)?
- Are any domain concepts conflated into a single type that should be distinct?
- Does the abstraction boundary match the conceptual boundary?
- Is there a gap between what the design intends and what it specifies?

HALT CANDIDATE signals: contradictory world assumptions, ontological conflation that makes the specification incoherent.

### Mathematician (CSP + Natural Deduction + Tableaux + Hoare)

Audits whether invariants are provable, state machines are complete, and type algebra is sound.

Key questions:
- Are all stated invariants derivable from the world assumptions?
- Are there reachable invalid states?
- Do pre/post conditions compose correctly across the call graph?
- Is `Result<T>` used where a state machine is needed (or vice versa)?

HALT CANDIDATE signals: unprovable invariant, mutually exclusive requirements, formal contradiction in the model.

### Engineer (CBR + AHP + TRIZ + Six Thinking Hats)

**Constructive phase** — audits implementation feasibility, testing strategy, operational concerns, and maintenance burden. Applies `audit-code` checklist as the implementation sub-step in Phase 1 (CBR).

**Pre-mortem phase** (separate invocation, Critical/Mission-Critical only) — adversarial. Assumes the artifact is in production and has failed. Identifies top 5 failure modes ordered by likelihood × impact.

Key questions (constructive):
- Does the implementation match the specification at this abstraction level?
- Are there N+1 queries, missing indexes, or bulk-operation violations?
- Is complexity pushed down appropriately or leaking at boundaries?
- What would make this hard to change in 6 months?

Key questions (pre-mortem):
- What assumption, if wrong, causes the most damage?
- What failure mode is hardest to detect in production?
- What does the monitoring not cover?

HALT CANDIDATE signals: required capability does not exist in the target stack, implementation is physically impossible given stated constraints.

---

## Design Principles (carried from audit-code)

### Complexity

1. **Complexity is incremental** — small complexities compound; eliminate trivial complexity early
2. **Working code is not enough** — must be maintainable, not just functional
3. **No technical debt** — continuously invest in design
4. **Push complexity down** — hide complexity in internal modules, not at boundaries
5. **Parsimony** — prefer the simplest solution that works; do not add elements not strictly necessary
6. **Elegance** — transparent, correct, explanatory, uniform, generic

### Modules

7. **Deep modules** — small interface, complete implementation
8. **Information hiding** — encapsulate design decisions
9. **General-purpose > special-purpose** — more general interfaces are simpler and deeper
10. **Layers with different abstractions** — each layer provides a distinct abstraction; don't mix levels
11. **Separate general from specific** — specialize without polluting

### Interface Design

12. **Simple interface** — prioritize common use case
13. **Define errors out of existence** — make invalid states unrepresentable

### Process

14. **Design twice** — the first design is rarely optimal
15. **Strategic > Tactical** — invest in design, not just features

---

## Code Quality Checklist (applied in Engineer phase)

### Readability

- [ ] Descriptive, consistent names
- [ ] Small functions (single responsibility)
- [ ] No duplication (DRY)
- [ ] No unnecessary comments
- [ ] Code in English
- [ ] Parsimonious — no unnecessary code or abstraction
- [ ] Elegant — transparent, correct, explanatory, uniform, generic

### Architecture

- [ ] Separation of concerns
- [ ] Appropriate abstractions
- [ ] Explicit dependencies
- [ ] No unnecessary coupling

### Type Safety

- [ ] Explicit type annotations in public APIs
- [ ] No `any` or `unknown` without justification
- [ ] Generics used correctly
- [ ] Type-safe data boundaries
- [ ] Ambiguous types eliminated

### Correctness Verification

- [ ] Object creation: cannot exist in invalid state
- [ ] State transition: maintains consistency
- [ ] Deletion: no orphaned records
- [ ] Query: always returns consistent data
- [ ] Preconditions validated before operation
- [ ] Postconditions guaranteed after operation
- [ ] Impossible states unrepresentable

### Properties

- [ ] **Totality** — function handles all valid inputs
- [ ] **Determinism** — same input → same output
- [ ] **Atomicity** — operations are all-or-nothing
- [ ] **Consistency** — data never in invalid state

### Security & Performance

- [ ] Input validation
- [ ] No injection vulnerabilities
- [ ] No hardcoded secrets
- [ ] No magic numbers (use constants)
- [ ] No N+1 queries (use eager loading)
- [ ] No queries in loops (use bulk operations)
- [ ] Adequate indexes where needed

---

## Red Flags

- **Change amplification** — simple change requires modifications in many places
- **Cognitive load** — developer needs to know too much to make changes
- **Obscurity** — important information not obvious
- **Shallow modules** — interface as complex as implementation
- **Information leakage** — design decisions exposed at module boundary
- **Special-purpose classes** — over-specialized, hard to reuse
- **Redundant code** — duplication that could be abstracted
- **Ontological conflation** — two domain concepts merged into one type (Philosopher lens)
- **Unprovable invariant** — invariant asserted but not derivable from world assumptions (Mathematician lens)
- **Invisible failure mode** — failure that bypasses all monitoring (pre-mortem lens)

---

## Verification Techniques

### Data Flow Analysis

- Trace input → output
- Verify no "impossible" states
- Confirm exceptions don't break invariants

### State Machine Analysis

1. Identify valid states
2. Diagram transitions
3. Verify completeness (all transitions handled)
4. Verify reachability (no orphaned states)
5. Verify safety (invalid states never reached)

### Ontology Check

1. List all module/type names
2. Map each to a domain role (agent, object, transfer, transaction)
3. Identify any name that could belong to two roles → likely conflation
4. Verify abstraction boundary matches conceptual boundary

---

## Smart Re-Audit Routing

When the user provides feedback after reviewing the holistic audit report, classify before re-running:

| Feedback type | Re-run from |
|---|---|
| "The concept is wrong / doesn't fit the domain" | @philosopher (all downstream follow) |
| "The types / invariants are off" | @mathematician → @engineer |
| "The implementation approach won't work" | @engineer only |
| "The whole thing missed the point" | Full pipeline |
| Ambiguous / unclassifiable | Full pipeline (safe default) |

State the classification explicitly before re-running:

```
Feedback classified as: [type]
Re-running from: [agent]
```

---

## Referee Decision Authority

| Conflict type | Veto authority |
|---|---|
| Safety-critical | Engineer |
| Mathematical correctness | Mathematician |
| Conceptual integrity | Philosopher |
| General trade-offs | Majority or User |

---

## Project Rules

- **English naming** — functions, variables, classes in English
- **Lints required** — run project linters before considering done
- **No lint disables** — fix root cause, don't suppress

---

## Validation Checklist

- [ ] W, R, and Specification present and non-contradictory (or HALT issued)
- [ ] Risk Profile determined
- [ ] Invocation depth matches Risk Profile
- [ ] Philosopher output received and spot-checked
- [ ] Mathematician output received and spot-checked (skip if Standard)
- [ ] Engineer output received and spot-checked
- [ ] Engineer pre-mortem received and spot-checked (skip if Standard)
- [ ] Phase-level citations extracted from all reasoning-flow files
- [ ] Conflict detection performed (type system | ontology | feasibility | none)
- [ ] Referee resolution applied if conflicts detected
- [ ] Design Gap Summary present
- [ ] Per-finding list uses [Lens] tags and phase-level citations
- [ ] Output matches schema
