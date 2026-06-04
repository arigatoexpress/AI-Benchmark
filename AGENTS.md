# AGENTS.md — AI-Benchmark

## What this repo does

Local LLM benchmarking toolkit for Ollama-compatible models. Measures speed, latency, VRAM, and output quality. Experimental and hardware-specific — results are valid only for the machine they were run on.

## Key files

| Path | Role |
|---|---|
| `accurate_benchmark.py` | GPU-isolated benchmark (recommended entry point) |
| `quality_evaluator.py` | Output quality scoring |
| `improved_visualizations.py` | Chart generation |
| `llm_benchmark.py` | Original benchmark script |
| `analyze_results.py` | Result parser and summary printer |
| `benchmark_results_*.json` | Timestamped run artifacts |

## How to run

```bash
# Verify Ollama is running
ollama list

# Run benchmark
python accurate_benchmark.py

# Evaluate quality of existing outputs
python quality_evaluator.py

# Generate charts
python improved_visualizations.py
```

## Safety boundaries

1. **Do NOT** add API keys or cloud inference endpoints; this is a local-Ollama-only suite
2. **Do NOT** delete historical `benchmark_results_*.json` files; they are the dataset
3. **Do NOT** hardcode hardware-specific assumptions into shared utility functions
4. Keep Matplotlib/Seaborn usage in visualization scripts only

## Current status

- Entry point: `accurate_benchmark.py`
- Requires Ollama running locally
- GPU monitoring is optional (falls back gracefully if NVML missing)

---

# AGENTS.md — Operating Charter

> Guiding principles for any AI agent (or human) working in this repo. Derived from the Andrej Karpathy engineering philosophy. Tool-neutral: applies whether you drive this repo with Claude Code, goose, or by hand.

## The four rules
1. **Simplicity first.** Write the minimum code that solves the task. No speculative abstractions, no unrequested features, no single-use platforms. Extract a shared module only when there are >= 2 real call-sites today.
2. **Surgical changes, one concern per PR.** Touch only what the task requires. Do not opportunistically reformat, bump unrelated deps, or fix adjacent dead code. Small, reviewable, independently revertable diffs.
3. **Evals are the spec.** Define and run the repo verification (tests, build, typecheck, smoke) BEFORE and AFTER a change. Nothing merges unless it stays green. Keep the generate->verify loop tight and reversible.
4. **Delete > add; fewer dependencies.** Removing code, repos, and dependencies is the highest-leverage move. Every dependency is attack surface you own. Pin and lock what remains. Humans stay in the loop for irreversible / outward-facing / production steps (deletes, credential rotation, infra teardown, deploys).

## Safety
- Never use `git add .` or `git add -A` — stage changed files by explicit path (avoids sweeping in WIP or secrets).
- Never commit secrets; `.env*` stays gitignored (except `.env.example`).
- Treat anything outward-facing or irreversible as draft-then-confirm.
