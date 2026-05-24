# Specut Reference

## Spike modes

| Mode | What it does | When to use |
|------|-------------|-------------|
| **full** | Write a runnable skeleton in worktree: interfaces, stubs, fake returns, basic wire-up. Build and run passes. | Large features where architecture uncertainty is high |
| **light** | Write only critical interface boundaries and key dependency validations. Not meant to run end-to-end. | Medium features, or when you're confident about most of the architecture |
| **none** | Skip spike entirely. Go straight to cutting cards based on reasoning. | Small features or when the tech lead already has clear architecture in mind |

### Spike failure

If the spike reveals a blocking problem (infeasible approach, library limitation, contradictory requirements):
1. Stop. Do not proceed to cutting cards.
2. Report the finding to the user.
3. Suggest alternative directions.
4. Wait for user input before re-spieking or adjusting the feature description.

## Card templates

### Scaffold card

```markdown
## [SCAFFOLD] {feature name}

### Intent

{1-2 sentences: what this feature enables, for whom}

### Core user paths

**Path 1: {path name}**
- Layers: {e.g., DB → Service → API → UI}
- Interface direction: {fuzzy boundary, e.g., "UserService exposes a way to look up users by email"}
- Key stubs: {what skeleton methods/functions will exist, with fake returns}

**Path 2: {path name}**
- ...

### Acceptance criteria

- [ ] All stubs compile and run (if full spike mode)
- [ ] Each user path has a clear entry point and exit point in the skeleton
- [ ] No real business logic implemented — only stubs and fake data
- [ ] Interface direction is documented but not locked — fill cards may adjust

### Notes

{Any architectural insights from spike, known constraints, technology choices that fill-card authors should be aware of}
```

### Fill card

```markdown
## [FILL] {narrow vertical slice description}

### Intent

{1 sentence: what this slice enables}

### User path

{Which core user path from the scaffold card this fills}

### Acceptance criteria

- [ ] {specific, testable criterion 1}
- [ ] {specific, testable criterion 2}
- [ ] {tests pass independently without other fill cards}

### Blocked by

- SCAFFOLD {feature name} (hard dependency)

### ⚠️ Potential conflicts

- {List of other fill cards this may conflict with and why, e.g., "FILL-3: both modify user authentication flow"}
- Or: "None identified"
```

## Cutting fill cards

### Sizing rule

Each fill card should require **one review context** — a reviewer can understand the PR without holding multiple concepts in their head. Indicators of too-large cards:

- "implement X and Y" → split into two cards
- Changes span >1 domain concept → split by concept
- Acceptance criteria >5 items → consider splitting

Indicators of too-small cards:

- Card description is a subset of another card's acceptance criteria → merge
- Implementation is trivially 5 lines → merge with adjacent slice

### Vertical slice alignment

Each fill card fills **one narrow vertical pipe** through the layers defined in the scaffold. Example:

| Too broad (bad) | Narrow (good) |
|-----------------|---------------|
| "Implement login" | "Add email+password login method" |
| "Implement user management" | "Add user profile retrieval API" |
| "Implement notifications" | "Add email notification for order confirmation" |

### Conflict detection

When cutting fill cards, check for potential conflicts between cards:
- Two cards modifying the same module/interface
- Two cards adding to the same database table schema
- Two cards depending on the same external service API

List these as soft dependency warnings. Do NOT create hard dependencies between fill cards.

## Tech stack detection

Auto-detect from project files:
- `package.json` → TypeScript/JavaScript, framework, test runner
- `Cargo.toml` → Rust
- `go.mod` → Go
- `pom.xml` / `build.gradle` → Java
- `requirements.txt` / `pyproject.toml` → Python

Present detected stack to user for confirmation. If detection fails or is ambiguous, ask.

## Output format

Single markdown file: `specut-{feature-slug}.md`

Structure:
```
# {feature name}

## SCAFFOLD
{scaffold card content}

---

## FILL 1: {title}
{fill card content}

---

## FILL 2: {title}
{fill card content}

---

...
```

The feature slug is derived from the feature description (kebab-case, 3-5 words max).