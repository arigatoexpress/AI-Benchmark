# AI-Benchmark

**Local LLM benchmarking suite for Ollama-compatible models.**

Measures tokens-per-second, latency, VRAM usage, and output quality across reasoning, coding, and general-knowledge tasks. Designed for repeatable, GPU-isolated comparisons on local hardware.

## What this does

A collection of Python scripts that benchmark local LLMs via Ollama. The suite emphasizes fair comparison (GPU memory cleared between tests) and quality evaluation (correctness, reasoning, completeness) rather than raw speed alone.

**Note:** This is a personal/experimental benchmarking toolkit. Results are hardware-specific and should not be treated as universal model rankings.

## Quick start

```bash
# Ensure Ollama is running and models are pulled
ollama list
ollama pull qwen3:14b

# Run GPU-isolated benchmark (~15-20 min)
python accurate_benchmark.py

# Evaluate output quality
python quality_evaluator.py

# Generate clean visualizations
python improved_visualizations.py
```

## Architecture

| Script | Purpose |
|---|---|
| `accurate_benchmark.py` | GPU-isolated speed + latency benchmarking |
| `quality_evaluator.py` | Output quality scoring (correctness, reasoning, format) |
| `improved_visualizations.py` | Matplotlib charts with clean spacing |
| `llm_benchmark.py` | Original benchmark suite (historical) |
| `analyze_results.py` | Result parsing and summary |

## Key features

- **GPU isolation** — clears VRAM between each model test
- **Quality scoring** — evaluates correctness and reasoning, not just speed
- **VRAM tracking** — peak usage per model via NVML
- **Visualization** — speed rankings, VRAM analysis, competitive landscape
- **Modular** — add new models by editing the model list in `accurate_benchmark.py`

## Tech stack

- Python 3.11+
- Ollama (local inference server)
- Matplotlib, Seaborn (visualizations)
- NVML / pynvml (GPU monitoring, optional)

## Data strategy

Benchmarks are run locally against your own Ollama instance. No external API keys required. Results are written as timestamped JSON files in the repo root.

## Governance notes

- **Hardware-specific** — results from RTX 5070 Ti do not generalize to other GPUs
- **Synthetic tests** — prompts are fixed; real-world performance varies
- **No secrets required** — Ollama runs locally without API keys

## Merged: kadima-bench

[`kadima-bench`](https://github.com/arigatoexpress/kadima-bench) has been folded
into this canonical repo and now lives under [`vendor/kadima-bench/`](vendor/kadima-bench/),
imported via `git subtree` so its full commit history is preserved. The upstream
`kadima-bench` repo will be archived after this merge lands.

**What kadima-bench is:** a packaged, CLI-driven local-LLM benchmarking framework
(`pip install -e vendor/kadima-bench`, then `kadima-bench run ...`). It covers the
same problem space as the scripts in this repo root — local models via Ollama, GPU
isolation, quality scoring, and matplotlib visualizations — but with a cleaner,
modular architecture (backends / suites / metrics / visualize), streaming latency
metrics (TTFT, ITL percentiles), Pareto-frontier composite scoring, an optional
lm-eval-harness bridge, and TOML config.

### Overlap vs. this repo's root scripts

| Concern | Root scripts (this repo) | `vendor/kadima-bench` |
|---|---|---|
| Entry point | many ad-hoc `*.py` scripts | single `kadima-bench` CLI |
| Focus | Qwen-family comparisons | general consumer-GPU models |
| GPU isolation | yes (clears VRAM) | yes (`ollama stop` between models) |
| Quality scoring | `quality_evaluator.py` | `metrics/quality.py` (8 categories, pass/fail) |
| Latency | aggregate t/s | streaming TTFT + ITL p50/p95/p99 |
| Charts | `improved_visualizations.py` | `visualize/charts.py` (7 charts) |
| Config | hard-coded model lists | `*.toml` |

### Dedup / reconciliation TODO (deferred — separate PR)

The two suites substantially overlap and should be reconciled. Heavy dedup is
intentionally **out of scope** for this merge (one concern per PR). Follow-ups:

- [ ] Decide the canonical engine: migrate the root `*.py` scripts onto the
      `kadima-bench` framework (preferred) and retire the duplicated ad-hoc scripts.
- [ ] Fold the Qwen-specific suites/prompts into `kadima_bench/suites/` as a preset.
- [ ] Unify result JSON schemas and chart generators (drop the duplicate chart code).
- [ ] Consolidate the two READMEs / docs once the engine is chosen.
- [ ] Move `vendor/kadima-bench` to a top-level package (or keep as vendored) per
      the chosen layout.

## Agent collaborators

See [AGENTS.md](AGENTS.md) for script descriptions and conventions.
