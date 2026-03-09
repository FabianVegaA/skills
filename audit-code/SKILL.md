---
name: audit-code
description: Audit computational artifacts against design principles
---

Audit code as a computational artifact: verify it correctly implements its specification at the appropriate abstraction level.

## Foundations

### Levels of Abstraction

Every computational artifact exists at a level:

1. **Intention** - What problem are we solving?
2. **Specification** - What should the artifact do?
3. **Implementation** - How does it actually work?

Audit at the right level. Code should correctly implement its specification, not redefine it.

### Correctness

A computational artifact is correct when:
- Implementation matches specification
- Specification fulfills intention
- No impossible states exist

---

## Design Principles

### Complexity

1. **Complexity is incremental** - Small complexities compound. Eliminate trivial complexity early.
2. **Working code is not enough** - Must be maintainable, not just functional.
3. **No technical debt** - Continuously invest in design.
4. **Push complexity down** - Hide complexity in internal modules, not at boundaries.
5. **Parsimony** - Prefer the simplest solution that works. Do not add elements not strictly necessary. A parsimonious solution matches the specification exactly—it does not solve more than required by the specification.
6. **Elegance** - The right solution. Clear, simple, no unnecessary parts. Elegant code is: transparent (obvious intent), correct (works for all cases), explanatory (documents itself), uniform (consistent patterns), and generic (general-purpose over special-purpose).

### Modules

5. **Deep modules** - Small interface, complete implementation. Prefer few deep modules over many shallow ones.
6. **Information hiding** - Encapsulate design decisions. Users should not need to know how internals work.
7. **General-purpose > Special-purpose** - More general interfaces are simpler and deeper. Avoid over-specialization.
8. **Layers with different abstractions** - Each layer provides a distinct abstraction. Don't mix levels.
9. **Separate general from specific** - Specialize without polluting.

### Interface Design

10. **Simple interface** - Prioritize common use case. It is more important to have a simple interface than a simple implementation.
11. **Define errors out of existence** - Validate first, don't handle after. Make invalid states unrepresentable.

### Process

12. **Design twice** - Consider alternatives. The first design is rarely optimal.
13. **Strategic > Tactical** - Invest in design, not just features. Short-term speed leads to long-term debt.

---

## Code Quality

### Readability

- [ ] Descriptive, consistent names
- [ ] Small functions (single responsibility)
- [ ] No duplication (DRY)
- [ ] No unnecessary comments
- [ ] Code in English
- [ ] Parsimonious - No unnecessary code or abstraction
- [ ] Elegant - Transparent, correct, explanatory, uniform, generic

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

---

## Correctness Verification

### Invariants

- [ ] Object creation: Cannot exist in invalid state
- [ ] State transition: Maintains consistency
- [ ] Deletion: No orphaned records
- [ ] Query: Always returns consistent data

### Preconditions & Postconditions

- [ ] Preconditions validated before operation
- [ ] Postconditions guaranteed after operation
- [ ] Impossible states unrepresentable

### Properties

- [ ] **Totality** - Function handles all valid inputs
- [ ] **Determinism** - Same input → same output
- [ ] **Atomicity** - Operations are all-or-nothing
- [ ] **Consistency** - Data never in invalid state

---

## Security & Performance

### Security

- [ ] Input validation
- [ ] No injection vulnerabilities
- [ ] No hardcoded secrets
- [ ] No magic numbers (use constants)

### Performance

- [ ] No N+1 queries (use eager loading)
- [ ] No queries in loops (use bulk operations)
- [ ] Adequate indexes where needed

---

## Project Rules

- **English naming** - Functions, variables, classes in English
- **Lints required** - Run project linters before considering done
- **No lint disables** - Fix root cause, don't suppress

---

## Execution

1. **Identify modified files**: `git diff --name-only`
2. **Determine abstraction level**: Is this spec, implementation, or intent?
3. **Verify correctness**: Does code at this level match level above?
4. **Apply design principles**: Check complexity, module depth, information hiding
5. **Run linters**: Project-specific commands
6. **Run type checker**: `mypy`, `tsc --noEmit`, etc.
7. **Report findings**: Classify by severity

---

## Severity

| Level | Description |
|-------|-------------|
| 🔴 Critical | Security vulnerability, data loss, breakage, incorrect results |
| 🟠 High | Performance issue, maintainability blocker, type unsafety |
| 🟡 Medium | Code smell, improvement opportunity |
| 🟢 Low | Style preference, nitpick |

---

## Output Format

```
[🔴/🟠/🟡/🟢] <file>:<line>
- Problem: ...
- Suggestion: ...
```

---

## Red Flags

Identify these indicators of poor design:

- **Change amplification** - Simple change requires modifications in many places
- **Cognitive load** - Developer needs to know too much to make changes
- **Obscurity** - Important information not obvious
- **Shallow modules** - Interface as complex as implementation
- **Information leakage** - Design decisions exposed at module boundary
- **Special-purpose classes** - Over-specialized, hard to reuse
- **Redundant code** - Duplication that could be abstracted

---

## Verification Techniques

### Data Flow Analysis

- Trace input → output
- Verify no "impossible" states
- Confirm exceptions don't break invariants

### State Machine Analysis

For code handling state transitions:
1. Identify valid states
2. Diagram transitions
3. Verify completeness (all transitions handled)
4. Verify reachability (no orphaned states)
5. Verify safety (invalid states never reached)
