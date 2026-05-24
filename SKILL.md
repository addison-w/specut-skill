---
name: specut
description: Break a milestone feature into a scaffold card plus parallel vertical-slice fill cards. Uses worktree spike to validate architecture, then outputs cards optimized for independent development and easy PR review. Use when breaking down a large feature, splitting work for a team, or creating Jira/GitHub cards from a spec.
---

# Specut

Break a milestone feature into cards that enable parallel development and easy PR review.

## Quick start

1. User describes a milestone feature
2. Clarify: feature description, spike mode, core user paths, tech stack
3. Spike in worktree (if mode != none)
4. Cut all cards to markdown file
5. Cleanup worktree

## Workflows

See [REFERENCE.md](REFERENCE.md) for detailed workflows, card templates, and spike modes.

### Clarify phase

Gather from user:
1. **Feature description** — what are we building?
2. **Spike mode** — full / light / none (see REFERENCE.md)
3. **Core user paths** — the vertical paths this feature enables
4. **Tech stack** — AI auto-detects, user confirms or corrects

If any of these are already clear from context, skip asking.

### Spike phase

Create a worktree, implement a thin skeleton that validates the architecture can run. If spike reveals a blocking problem, stop and discuss with user before proceeding.

### Cut phase

Produce one markdown file with all cards. See [REFERENCE.md](REFERENCE.md) for card format.

**Scaffold card**: locked intent + interface direction (fuzzy, adjustable by fill cards). Organized by user path — each path lists the layers it crosses.

**Fill cards**: narrow vertical slices, one review context each, independently testable. Intent + acceptance criteria + soft dependency conflict warnings. Hard blocked by scaffold card.

### Cleanup phase

Remove the worktree. Spike code is reference only, never merged.

## Key principles

- **Tech decisions belong to the person picking up the card**, not the spec
- **Interface direction is fuzzy** — fill cards adjust as needed, merge conflicts are acceptable cost
- **Each fill card = one review context** — reviewer holds one concept, not many
- **Cards are small enough to review in one sitting**