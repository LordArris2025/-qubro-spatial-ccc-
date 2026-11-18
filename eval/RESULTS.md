# Evaluation results: Core AI order router (offline, deterministic fallback)

Dataset: `eval/orders.json`, 74 hand-labelled English / Malay orders. Reproduce: `python3 eval/routing_eval.py`.

| Metric | Value |
|---|---|
| Sector routing accuracy | **98.6%** (73/74) |
| Macro F1 (5 classes) | **0.985** |
| Authorize-word safety flag accuracy | **100.0%** (74/74) |
| keyword_route latency p50 / p95 | **0.023 ms / 0.040 ms** |

| Class | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| AI 1 (cyber) | 1.00 | 1.00 | 1.00 | 15 |
| AI 2 (drones/robots) | 1.00 | 0.92 | 0.96 | 13 |
| AI 3 (lidar/orbital) | 1.00 | 1.00 | 1.00 | 14 |
| AI 4 (cctv/doors) | 1.00 | 1.00 | 1.00 | 18 |
| CORE | 0.93 | 1.00 | 0.97 | 14 |

## Misrouted orders (1)

- "patrol robots to the L24 airlock": expected AI 2 (drones/robots), got CORE

Scope: this measures the keyword fallback only. With a key, the AI model writes the answer; its live latency, token use and error counts are recorded by the in-app monitor (`/monitor/`, VR panel key N).
