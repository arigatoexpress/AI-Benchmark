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
