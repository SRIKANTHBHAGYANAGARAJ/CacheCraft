# Changelog

All notable changes to CacheCraft are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- `bench --only <kinds...>` restricts a run to the named strategies, for
  quick single-policy checks without a config file.
- Project governance and contributor documentation (`CONTRIBUTING.md`,
  `CODE_OF_CONDUCT.md`, `SECURITY.md`, `SUPPORT.md`).
- Architecture and design references (`ARCHITECTURE.md`, `DESIGN.md`).
- Packaging metadata (`pyproject.toml`), developer `Makefile`, and CI workflow.

## [0.8.0] - 2026-09-02

### Added
- `route` strategy: exact-first matching with a route-scoped `overlap`
  fallback, modeling routers that never reuse an answer across endpoints.
- Per-route breakdown in `ReplayResult.per_route()` and in both the table and
  JSON reports, so you can see which endpoint carries the caching wins.
- Native Go companion runner (`craft/`) with `craftbench` mirroring the
  `exact` and `overlap` policies for large-trace throughput.

### Changed
- Trade-off score reweighted to `0.5*avoided + 0.3*hit_rate + 0.2*quality`
  so avoided latency, not raw hit rate, drives the ranking.

## [0.7.0] - 2025-11-18

### Added
- `signals` subcommand exposing the derived normalization signals for a
  single prompt, so operators can debug why two phrasings did or did not
  collide in a strategy.

### Changed
- Normalizer now reports per-token canonicalization provenance in the JSON
  report, making NFKC folding and stop-word removal auditable.

## [0.6.0] - 2024-06-21

### Added
- `overlap` strategy using Jaccard similarity over token sets with a
  configurable acceptance threshold and bounded LRU store.
- Match `quality` score in `[0, 1]` carried by every hit so reports can
  penalize reckless fuzzy matching.

### Changed
- Replay engine now models a fixed `hit_cost_ms` per hit and reports
  `avoided_ms` floored at zero, reflecting that caches are not free.

## [0.5.0] - 2023-08-09

### Added
- `prefix` strategy keyed on the first N normalized tokens for templated
  prompts that share a stem.
- Bench configuration loader (`config.py`) so an experiment can be versioned
  as JSON alongside its trace.

### Fixed
- Deterministic canonicalization via Unicode NFKC folding, ensuring signals
  are byte-identical across platforms and locales.

## [0.4.0] - 2022-09-04

### Added
- JSONL trace loader with 1-based line-number errors (`TraceError`), comment
  and blank-line skipping, and `trace_stats` summaries.
- `exact` strategy keyed on the SHA-1 fingerprint of the canonical text as the
  zero-false-hit safety baseline.

## [0.3.0] - 2021-06-14

### Added
- Initial CLI with `bench`, `inspect`, `signals`, and `strategies` commands.
- First report rendering: hit rate and avoided milliseconds per strategy in a
  fixed-width table.

## [0.2.0] - 2020-04-22

### Added
- The replay engine (`engine.py`): simulate one caching policy over a recorded
  trace and produce hits, misses, and avoided latency.
- Trace format spec with example JSONL fixtures under `examples/`.

## [0.1.0] - 2019-03-11

### Added
- The problem statement in prose: semantic caching is easy to turn on and
  dangerous to get wrong; the only honest way to choose a policy is to measure
  on your own traffic.
- Normalization module (`normalize.py`) with case folding and token
  splitting for the earliest exact-match experiments.

## [0.0.1] - 2018-05-17

### Added
- Initial project scaffold: package skeleton, README, and the Apache-2.0
  licence.
- A single sample trace and the first hand-rolled comparison loop for exact
  matching.