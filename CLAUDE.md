# Qlib-with-Claudex

Microsoft Qlib fork for use with Claude Code and the RD-Agent-with-Claudex research loop.

## Overview

Qlib is a quantitative investment platform. This fork has **no code changes** from upstream — Qlib itself has zero OpenAI dependencies. The `with-Claudex` suffix indicates it is configured for use with the Claude Code orchestration layer in RD-Agent-with-Claudex.

## Key Paths

- `qlib/` — Core Qlib library (data, model, backtest, workflow)
- `qlib/contrib/` — Community contributions (models, strategies)
- `examples/` — Example notebooks and configs
- `scripts/` — Data download and utility scripts

## Data Setup

```bash
# Download CSI300 market data (China A-shares)
python -m qlib.run.get_data qlib_data --target_dir ~/.qlib/qlib_data/cn_data --region cn

# Download US market data
python -m qlib.run.get_data qlib_data --target_dir ~/.qlib/qlib_data/us_data --region us
```

## Qlib Initialization

```python
import qlib
qlib.init(provider_uri="~/.qlib/qlib_data/cn_data", region_type="cn")
```

## Factor Backtest

The RD-Agent factor loop generates `factor.py` files that:
1. Read `source_data.h5` (market data: open, close, high, low, volume, vwap)
2. Compute a factor value per (date, instrument) pair
3. Write `result.h5` with key `"data"`

Evaluation uses IC (Information Coefficient), IR, and Rank IC metrics via Qlib's built-in evaluator.

## Common Commands

```bash
# Run a Qlib workflow config
qrun examples/benchmarks/LightGBM/workflow_config_lightgbm_Alpha158.yaml

# Run factor backtest
cd <workspace_path> && python factor.py
```

## Conventions

- MIT license (inherited from Microsoft Qlib)
- No OpenAI dependencies in this repository
- Data directory: `~/.qlib/qlib_data/`
