# Reinforcement Learning for Adaptive Traffic Signal Control

This repository is a starter project for a group Reinforcement Learning course project on adaptive traffic signal control under nonstationary traffic demand.

## Status

Implemented now:

- single-intersection simulator with stochastic arrivals
- three heuristic baselines: fixed-cycle, queue-threshold, max-pressure
- DQN training loop with replay buffer and target network
- JSON result outputs for baselines and DQN training/evaluation
- smoke tests for the environment and the main scripts

Planned but not included yet:

- finished analysis notebooks
- presentation-ready plots and figures
- checkpoint resume / experiment tracking
- multi-intersection extensions

## Project Goal

We study a single-intersection control problem where an agent decides whether to keep or switch the signal phase at each time step. The goal is to reduce long-term congestion under stationary and nonstationary traffic regimes.

Core research question:

Can an RL controller learn a more robust long-horizon traffic signal policy than fixed-cycle and queue-based rule baselines, especially when traffic demand changes over time?

## Why This Scope Works

- The task is a clean sequential decision-making problem with delayed effects.
- The simulator is lightweight enough for a course project.
- Strong baselines are easy to define and compare against.
- Results are interpretable through waiting time, queue length, throughput, and switch counts.

## Repository Layout

```text
reinforcement-learning-traffic-signal-control/
├── configs/
│   └── default.yaml
├── docs/
│   └── proposal_draft.md
├── notebooks/
│   └── README.md
├── results/
├── scripts/
│   ├── run_baselines.py
│   └── train_dqn.py
├── src/
│   └── traffic_rl/
│       ├── __init__.py
│       ├── baselines.py
│       ├── config.py
│       ├── dqn.py
│       ├── env.py
│       └── evaluation.py
├── tests/
│   └── test_env.py
├── .gitignore
└── requirements.txt
```

## Environment Summary

- Intersection: single intersection, two phases
- Phase 0: north-south green
- Phase 1: east-west green
- Action space:
  - `0`: keep current phase
  - `1`: switch phase
- Default state:
  - queue lengths for `N, S, E, W`
  - current phase
  - phase duration
  - current arrival rates for `N, S, E, W`

The environment includes:

- stochastic arrivals from configurable Poisson regimes
- per-step departure capacity
- switch loss through yellow-time cooldown
- queue-based or waiting-based reward shaping

## Baselines

Implemented baselines:

- Fixed-cycle controller
- Queue-threshold heuristic
- Max-pressure style heuristic

RL starter:

- DQN with replay buffer
- target network
- epsilon-greedy exploration

## Quick Start

Create an environment and install dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Optional extras for notebooks, plotting, alternative YAML parsing, and local test tooling:

```bash
pip install -r requirements-optional.txt
```

Run the baselines:

```bash
python3 scripts/run_baselines.py --config configs/default.yaml
python3 scripts/summarize_results.py results/baseline_summary.json
```

Train the DQN starter:

```bash
python3 scripts/train_dqn.py --config configs/default.yaml
python3 scripts/summarize_results.py results/dqn_summary.json
```

Run the smoke tests:

```bash
python3 -m unittest discover -s tests
```

## Suggested Project Milestones

1. Validate the simulator and baseline behavior.
2. Run DQN on the stationary training regime.
3. Compare against baselines on asymmetric and nonstationary regimes.
4. Add ablations for reward design and state representation.
5. Turn the results into report figures and presentation slides.

## Suggested Team Split

- Environment and baselines: simulator, arrival regimes, fixed-cycle and heuristic policies
- RL training: DQN implementation, tuning, checkpointing
- Evaluation and presentation: metrics, plots, ablation studies, report/slides

## Outputs

Main generated artifacts:

- `results/baseline_summary.json`
- `results/dqn_summary.json`
- `results/checkpoints/dqn_policy.pt`

## Immediate Next Files To Edit

- [`configs/default.yaml`](configs/default.yaml): experiment settings
- [`docs/proposal_draft.md`](docs/proposal_draft.md): proposal text
- [`src/traffic_rl/env.py`](src/traffic_rl/env.py): environment dynamics
- [`scripts/train_dqn.py`](scripts/train_dqn.py): RL training loop

## Notes

This starter repo is intentionally scoped around a single intersection. If the project progresses smoothly, a `2x2` grid can be added later as an extension rather than the main deliverable.
