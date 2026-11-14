# Stream brief — s020-js-grammar
**From**: pij-straightforward-walrus (o-prime, flowspace3-vk) · **Date**: 2026-11-14 · **Lifecycle**: provisional

## Structure tree
```text
Vaughan (human; reaches o-prime via this pane + live-transcripts dashboard folder "flowspace3")
└─ o-prime pij-straightforward-walrus
   └─ this stream (you) · window s020-js-grammar · role pm
```

## Work item
- **Plan folder**: `docs/plans/020-js-grammar/` (create via `/builder`; pass `--plan-dir docs/plans/020-js-grammar` to `harness flow create`)
- **Worktree**: `/Users/vaughanknight/GitHub/s020-js-grammar`
- **Branch**: `s020/js-grammar` · **Base**: `origin/main` at `b7774df` (PR #180)
- **Remotes**: `origin` = AI-Substrate/flowspace3 (READ ONLY for us; never push), `fork` = vaughanknight/flowspace3 (push your branch here)
- **Landing**: PR `vaughanknight:s020/js-grammar` → `AI-Substrate/flowspace3:main`. **Do NOT open the PR or any upstream issue yourself**: report ready-to-ship to the o-prime; Vaughan gives the go.
- **Human ask, verbatim** (Vaughan, 2026-11-14): "let's check out flowspace3 and add Javascript support, it shouldn't be too hard. We can then PR back"
- **Gap (verified)**: `crates/parsers/src/lib.rs` `Language::for_extension` maps `ts|mts|cts` and `tsx` only; `.js .mjs .cjs .jsx` have no grammar, so JS files index file-level only.
- **Prior art**: repo skill `.agents/skills/add-language/SKILL.md` (the 5 steps + 4 traps), `docs/services/scanner.md` (language table + worked examples), upstream plan `docs/plans/015-ts-grammar/` (same shape of change). Read-only context: `git show origin/prime-governance:government/briefs/backlog.md` (rows on plan 015 / row 147).
- **Design call to make in the plan** (record the decision + why): dedicated `tree-sitter-javascript` crate (expected; JSX lives in it) vs routing JS through the TypeScript grammar. Check version compatibility with the workspace tree-sitter version.
- **Real-world proof** (in addition to unit fixtures): `~/GitHub/dodo-gogo` (ImpactJS, ~59 JS files; `game/impact/lib/game/main.js` defines `MyGame` via `ig.module(...).defines(function(){ MyGame = ig.Game.extend({...}) })`). Show that function/method-level elements come out of a real file (e.g. a scan or test-build run against a COPY or read-only path). Note: ImpactJS's `X = Base.extend({ init: function(){} })` object-literal methods are a realistic pattern; decide in the plan whether it's in scope (pair, property-function) and say so.

## Descriptive fence
Canonical: `pij fence show --stream flowspace3-vk:js-grammar`.
- Expected touch set: `crates/parsers/**`, `Cargo.toml`, `Cargo.lock`, `crates/testkit/arch-allowlist.toml`, `docs/services/scanner.md`, `docs/plans/020-js-grammar/**`
- Shared (touch only if the add-language skill's classify rule demands it): `crates/core/src/classify.rs`
- Scratch: `.harness/temp/s020-js-grammar/**`
- Hard exclusions: `~/GitHub/flowspace3` main checkout (and its untracked `docker-compose.override.yml`), `~/GitHub/flowspace3-gov`, `the-flow.json`/`the-flow.md` (harness CLI only), `~/GitHub/dodo-gogo` (read-only).
- **Never** stop, restart, reconfigure or re-root the running flowspace3 0.7.0 daemon (:7373, db container :5433); it serves Vaughan's other repos. Test builds use their own ports/DBs/config dirs (`./target/...`, not `~/.local/bin/flowspace3`).
- New path outside the fence: tell the o-prime and continue.

## Orient stack
1. `/pij prime` → stream triage → `references/prime/orchestrator.md`
2. `references/prime/orient-global.md`
3. Local orient: `/Users/vaughanknight/GitHub/flowspace3-gov/.harness/government/orient-local.md`
4. This brief
5. Upstream `AGENTS.md` + `CONTRIBUTING.md` in your worktree (dogfood flowspace3; conventional commits are binding; tests that fail without the change)
6. Protocol pages only on demand

## Assignment and reporting
- Flow: `/builder` plan (1b) → tasks (5) → **report the validated plan + proposed coder/reviewer profile to the o-prime and wait for the go** → implement (6) → cross-model review (7) → report ready-to-ship.
- Gate before any "done": `harness checks` (fmt, clippy -D warnings, test) in your worktree, niced (C12). Postgres tests: use the repo's test compose service, never the live db.
- Report at preamble, plan-validated, each phase checkpoint, ready-to-ship: `claim · artifacts[] · shas[] · gates[] · observations[] · open[]` (C10). `pij report now` at each edge.
- Commits via `harness commit`; conventional (`feat(parsers): …`). Push to `fork` freely.
- Questions: ask the o-prime (it decides or escalates to Vaughan). Friction with the product or harness: `harness observe` + tell the o-prime.
