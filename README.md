# Collaborative Perception

Confidence attribution for V2X information from physical and trajectory consistency: two papers
and the dataset generator behind the second.

| Directory | What it is | Experiment |
|---|---|---|
| [`Beacon-Confidence/`](Beacon-Confidence) | Paper 1: per-sender confidence of CAM motion vectors (CTRA/OLS prediction, consistency and predictive checks, Beta reputation) | Detection on VeReMi Extension (`detection-eval/`); cost against F2MD, also on a RISC-V VisionFive 2 (`computational-eval/`, `vf2-setup/`) |
| [`Object-Confidence/`](Object-Confidence) | Paper 2 (MVP): CAM and CPM motion vectors associated to fused object tracks (UKF, CTRA), confidence per track | Preliminary: VeReMi Extension position/speed faults, one subset per type, track-level AUC (`scripts/run_preliminary.sh`) |
| ↳ [`libs/Artery-Dataset-Generator/`](Object-Confidence/libs/Artery-Dataset-Generator) | Submodule of Paper 2: Artery + LuST engine that records honest CAM/CPM ground truth (ETSI TS 103 324 CPM), with an offline injector of VeReMi Extension sensor errors and faults | Produces `data/groundtruth/` and the fault subsets for Paper 2 |
| [`diagrams/`](diagrams) | Diagrams of the work | — |

Shared code: [`v2x-misc`](https://github.com/gustavovonn/v2x-misc), a submodule of both papers and
of the generator (motion models, UKF, consistency check, geodesy, transport theorem).

Data flow for Paper 2:

```
libs/Artery-Dataset-Generator -> data/groundtruth -> inject_faults.py (one subset per fault type)
                                                                   |
Object-Confidence: flatten_dataset.py -> replay (engine) -> evaluate.py (AUC table, figure)
```
