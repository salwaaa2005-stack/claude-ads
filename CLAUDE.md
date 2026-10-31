# Claude Ads Repository Instructions

@AGENTS.md

`AGENTS.md` is the canonical portable repository contract. `ads/SKILL.md` is
the product entrypoint. Load focused skills and references only when the task
routes to them.

## Operator account work

For any work on the repository owner's own store and ad accounts (Google Ads,
Google Shopping, Performance Max, Merchant Center, product feeds, Keyword
Planner, search terms, negatives, bids, budgets, ROAS, campaign reviews, or
supplier freight questions), always invoke the `personal-google-ads-marketing`
skill first. Do this even when the request only names a brand or asks how a
campaign is doing.

- That skill holds the store's business rules, account profile, live campaign
  state, change log, lessons learned, and tested helper scripts. Its business
  rules govern that account.
- The repository's safety and evidence rules still apply alongside it:
  read-only by default, explicit approval for each exact change, validate
  before applying, verify, log, and keep a rollback.
- Keep account IDs, margins, exports, and change records out of this
  repository. They belong in the personal skill or the session scratchpad.
- If the skill is not available in the session, say so and continue read-only
  using `ads/SKILL.md`.

## Architecture

- `ads/`: primary `/ads` orchestration skill and shared references.
- `skills/`: focused platform and workflow skills.
- `agents/`: bounded workers that return results to the conductor.
- `claude_ads_core/`: deterministic schemas, normalization, scoring, reporting,
  and adapter contracts.
- `control-plane/`: product boundaries, evidence and capability manifests,
  safety policy, maturity, ecosystem decisions, and release gates.
- `tests/` and `evals/`: deterministic and model-facing verification.

## Development

- Keep skill entrypoints concise and use progressive disclosure.
- Keep platform facts and volatile guidance in dated references, not permanent
  root instructions.
- Python tools expose a CLI, structured output, and tests proportional to risk.
- Agents return schema-valid results; the conductor owns final artifacts.
- Use manifest-derived counts and capability status instead of hand-maintained
  claims.

## Verification

Run the focused tests for the changed surface, then the complete local suite.
Release claims additionally require every gate in
`control-plane/RELEASE_REQUIREMENTS.md` and required remote CI.
