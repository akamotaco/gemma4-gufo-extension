# Changelog

Notable user-facing changes are recorded here. Versions follow `version` in
[extension.json](extension.json).

## Unreleased

### Bug Fixes

* **moe routing:** a token whose router logits are NaN routes to experts in
  range. Before, it selected an expert id past the expert table (128 at 128
  experts, `0x7FFFFFFF` at 256), and the routed products, the expert counts
  and the expert scales were indexed out of bounds
  ([#4](https://github.com/franzmoca/gemma4-gufo-extension/pull/4), by
  [@mduka](https://github.com/mduka)).
* **moe routing:** a +INF router logit gives its row finite weights, split
  over its +INF experts. Before, every weight of the row was NaN
  ([#5](https://github.com/franzmoca/gemma4-gufo-extension/pull/5)).

### Build

* **releases:** each release is tagged `vX.Y.Z` and published as a GitHub
  release with its changelog section, so a flake can pin a validated release.

## 0.2.0 (2026-11-17)

Built on Gufo v0.35.0 (`0260e116`); needs extension API 0.3.20 or later.

### Dependencies

* **Gufo:** pinned to v0.35.0. The extension now needs extension API 0.3.20 or
  later, which requires every text runner to declare a scheduling profile
  ([gufo-org/gufo#1175](https://github.com/gufo-org/gufo/pull/1175)). The
  runner declares 512 prompt tokens per pass beside decoders, and decode steps
  batch up to 8 sessions.

### Bug Fixes

* **prefill:** each prefill step ends at the engine's chunk, so the host
  reports progress during a long prompt and schedules other requests between
  its chunks. Before, a prompt with no peers ran as one step.
* **tool calls:** tool calls and their history keep the model's number
  spelling ([@mduka](https://github.com/mduka)).

### Performance

* **prompt cache:** cache transfer leases are sized from the serving slots:
  two sessions share 3 native streams instead of owning 64.

## 0.1.0 (2026-11-17)

Built on Gufo `49380168`; needs extension API 0.2.14 or later.

The first release: Gemma 4 31B, 31B QAT and 26B-A4B on Strix Halo `gfx1151`
with MTP drafting, prompt lookup, the BF16 vision sidecar, batched sessions,
native tool calls and the shared serving cache.
