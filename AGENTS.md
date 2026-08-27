# Repository instructions

## Mission

Build a secure internal wealth-management platform for a small firm. Optimize for correctness, auditability, understandable operations, and incremental delivery.

## Sources of truth

1. The human-approved active OpenSpec change defines intended behavior for its branch.
2. `openspec/specs/` defines current implemented behavior.
3. `docs/adr/` records accepted hard-to-reverse decisions.
4. `docs/product/` describes product direction, not current behavior.
5. Code and tests are implementation and evidence. They do not silently redefine requirements.

If sources conflict, stop and report the conflict. Do not guess.

## OpenSpec workflow

- Every material behavior change requires one active change under `openspec/changes/<change-id>/`.
- Use one independently deployable vertical slice per change.
- Do not implement until a human records `PLAN APPROVED` in the draft PR.
- Revise changed intent in the active artifacts before changing code.
- Never edit generated `openspec-*` agent skills.
- Archive the change before merge and inspect the resulting canonical spec diff.
- Run `npx openspec validate --all --strict` before requesting final review.

## Agent roles

- Claude Code normally authors or integrates specifications and handles high-risk cross-cutting work.
- Codex normally challenges specifications, implements bounded tasks, and reviews Claude-authored work.
- The agent that did not author a code change performs its final review.
- Reviewers are read-only and use fresh context. Reviewers report findings but do not fix them.
- The original author fixes accepted findings.
- A human approves plans, triages conflicting findings, merges, and deploys.
- Only one write-capable agent may work on a branch at a time.

## Architecture

- TypeScript modular monolith in a monorepo.
- React in `apps/web`, Express in `apps/api`, PostgreSQL for persistence.
- Domain logic does not depend on HTTP, React, or database frameworks.
- Shared request and response contracts live in `packages/contracts`.
- Keep module ownership and trust boundaries explicit.
- Do not introduce a service, queue, cache, or framework without a demonstrated requirement.

## Security and data

- Never use real client or financial data in prompts, fixtures, logs, screenshots, or tests.
- Use synthetic, clearly marked test data.
- Treat authorization, auditability, import provenance, backup, and recovery as product behavior.
- Do not expose secrets, `.env` files, tokens, or production data.
- Do not weaken tests, permissions, or validation to make a task pass.

## Change discipline

- Work only on the supplied OpenSpec change ID and accepted task.
- Keep diffs focused. Report unrelated problems without opportunistic rewrites.
- Do not merge, deploy, force-push, publish, or alter repository protection.
- Add dependencies only when justified in the active design.
- Use an ADR only for a hard-to-reverse choice with real alternatives.
- Keep `CONTEXT.md` a glossary, not a chronological log.

## Verification

Before declaring implementation complete, run:

```bash
./scripts/verify.sh
```

State which checks ran, their results, any skipped checks, and residual risk. Never claim success from inspection alone.