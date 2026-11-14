# Orient — local (flowspace3-vk)
**Scope**: Vaughan's fork work on flowspace3 · o-prime single writer · government-root `/Users/vaughanknight/GitHub/flowspace3-gov/.harness/government`

## What this project is
flowspace3: local semantic code search for agents (Rust daemon + CLI, Postgres/pgvector, JSON envelopes).
We are an **outside contributor**: work lands as PRs from `vaughanknight/flowspace3` into `AI-Substrate/flowspace3` main.
Upstream owns product direction (its PRD, rulings, prime pij-spatial-skink on Jordan's fleet). We follow its doctrine.

## Mandatory orient reads
- Upstream `AGENTS.md`, `CONTRIBUTING.md` (on `origin/main`)
- `origin/prime-governance`: `government/worker-roster.md`, `government/briefs/backlog.md` (read only)
- Repo skill `.agents/skills/add-language` (for grammar work); upstream plan 015 (ts-grammar) as prior art

## What matters here
- Conventional commits are binding (release-please). Behavioural changes need tests that fail without them.
- CONTRIBUTING: "For anything substantial, open an issue first". Outward-facing acts (issue, PR) need Vaughan's go.
- Dogfood: `flowspace3 search` before grep; report friction with `harness observe`.
- Never stop, reconfigure or re-root the running 0.7.0 daemon (:7373, db :5433). It serves dodo-gogo and live-transcripts.
- Never touch `~/GitHub/flowspace3/docker-compose.override.yml` (Vaughan's untracked file) or that checkout's `main`.
- PRD reconciliation owner: n/a (upstream owns its PRD); the o-prime reconciles our plan receipts.

## Repo mechanics
| Question | Answer |
|---|---|
| Gate | `harness checks` = `cargo fmt --all --check`, `cargo clippy --all-targets -- -D warnings`, `cargo test --all` (niced, C12) |
| Worktree root / naming | `~/GitHub/fs3-<ord>-<slug>`, branch `<ord>-<slug>` |
| Base | freshly fetched `origin/main` |
| Push / PR | push branch to `fork`; PR `vaughanknight:<branch>` → `AI-Substrate:main` only after Vaughan's go |
| Batons | test Postgres (compose test service :5434) if two seats run `cargo test` at once; Azure spend |
| Never-stage | `the-flow.json`, `the-flow.md` (harness CLI only), `target/` |
| Human channel | live-transcripts dashboard folder `flowspace3` (chat-ask pointers) + this pane |

## Portfolio
Store project `flowspace3-vk`. Order (Vaughan, 2026-11-14): 1) JavaScript support → PR upstream; 2) Azure resources in MCAPS-Hybrid-REQ-177459-2026-vknight (plan + cost → go first).
