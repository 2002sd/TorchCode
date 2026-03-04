# TorchCode

A self-hosted Jupyter-based environment for practicing PyTorch operator implementations from scratch. Like LeetCode, but for tensors — in a real notebook.

## Quick Start

```bash
make run
```

Open **http://localhost:8888** — that's it.

Works with both Docker and Podman (auto-detected).

## How It Works

Each problem has **two** notebooks:

| File | Purpose |
|------|---------|
| `01_relu.ipynb` | Blank template — write your code here |
| `01_relu_solution.ipynb` | Reference solution — check when stuck |

**Blank templates reset on every `make run`** so you always start fresh.
If you want to keep your work, save it under a different filename.

### Workflow

1. Open a blank notebook (e.g. `01_relu.ipynb`)
2. Read the problem description
3. Implement your solution in the marked cell
4. Debug freely — `print(x.shape)`, check gradients, whatever you need
5. Run the judge cell: `check("relu")`
6. See instant colored feedback with per-test results
7. Stuck? Open `01_relu_solution.ipynb` or call `hint("relu")`

## Problems

### Basics

| # | Problem | Task ID | Difficulty |
|---|---------|---------|------------|
| 1 | Implement ReLU | `relu` | Easy |
| 2 | Implement Softmax | `softmax` | Easy |
| 3 | Simple Linear Layer | `linear` | Medium |
| 4 | Implement LayerNorm | `layernorm` | Medium |
| 7 | Implement BatchNorm | `batchnorm` | Medium |
| 8 | Implement RMSNorm | `rmsnorm` | Medium |

### Attention Mechanisms

| # | Problem | Task ID | Difficulty |
|---|---------|---------|------------|
| 5 | Scaled Dot-Product Attention | `attention` | Hard |
| 6 | Multi-Head Attention | `mha` | Hard |
| 9 | Causal Self-Attention | `causal_attention` | Hard |
| 10 | Grouped Query Attention | `gqa` | Hard |
| 11 | Sliding Window Attention | `sliding_window` | Hard |
| 12 | Linear Self-Attention | `linear_attention` | Hard |

### Full Architecture

| # | Problem | Task ID | Difficulty |
|---|---------|---------|------------|
| 13 | GPT-2 Transformer Block | `gpt2_block` | Hard |

## Commands

```bash
make run    # Build & start (http://localhost:8888)
make stop   # Stop the container
make clean  # Stop + remove volumes + clear progress
```

## In-Notebook API

```python
from torch_judge import check, hint, status

status()                    # Progress dashboard
check("relu")               # Judge your implementation
hint("causal_attention")    # Get a hint
```

## Architecture

```
┌──────────────────────────────────────────┐
│           Docker / Podman Container      │
│                                          │
│  JupyterLab (:8888)                     │
│    ├── templates/  (baked in image)      │
│    ├── solutions/  (baked in image)      │
│    ├── torch_judge/ (judge engine)       │
│    └── PyTorch (CPU), NumPy             │
│                                          │
│  entrypoint.sh:                          │
│    1. Copy templates → notebooks/ (reset)│
│    2. Copy solutions → notebooks/        │
│    3. Start JupyterLab (no auth)         │
│                                          │
│  Volumes:                                │
│    ./notebooks    → working directory    │
│    ./torch_judge  → judge engine (live)  │
│    ./data         → progress.json        │
└──────────────────────────────────────────┘
```

Single container. Single port. No database. No frontend framework.

## Requirements

- Docker or Podman with Compose support
- No GPU needed — runs on CPU

## License

MIT
