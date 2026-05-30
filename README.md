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

## Agent collaborators

See [AGENTS.md](AGENTS.md) for script descriptions and conventions.
