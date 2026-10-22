# V2X Misbehavior Analysis — Industrial and Vehicular Networks

Two experiments on misbehavior analysis services for V2X, both evaluated offline on the
[VeReMi Extension](https://zenodo.org/records/20090854) dataset:

- **[Beacon-Confidence](Beacon-Confidence/)**: assigns a confidence to each kinematic beacon (CAM)
  from its physical consistency with the sender's predicted motion. It includes a
  detection-quality experiment and a computational-cost experiment.
- **[Object-Confidence](Object-Confidence/)**: assigns a confidence to each perceived object in
  CAMs and CPMs. Reports are associated with fused tracks (Hungarian algorithm + UKF), and each
  track is scored by how consistently the reports agree with it.

## Getting the code

Each experiment, and each of their libraries, is a git submodule. Clone with all of them:

```sh
git clone --recurse-submodules https://github.com/gustavovonn/v2x-project-implementation.git
```

or, in an existing clone:

```sh
git submodule update --init --recursive
```

## Running the experiments

Requirements: CMake ≥ 3.16, a C++17/C++20 compiler and Python 3.

**Beacon-Confidence** (detection experiment; needs `libarrow-dev` and `libparquet-dev`):

```sh
cd Beacon-Confidence
./veremi-extension/run_pipeline.sh           # download and convert VeReMi Extension (very slow)
cmake -S detection-eval -B detection-eval/build && cmake --build detection-eval/build
./run_veremi_batch.sh --sweep                # results in detection-eval/results/
```

**Object-Confidence**:

```sh
cd Object-Confidence
cmake -S . -B build && cmake --build build
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
scripts/run_preliminary.sh                   # 8 fault subsets, detection and association tables -> results/lust-full-run0/
```

Downloading the VeReMi Extension dataset takes a long time. The computational-cost experiments of
Beacon-Confidence require a StarFive VisionFive 2 SBC.

Each experiment's README has the full workflow: the computational-cost and RISC-V benchmarks for
Beacon-Confidence, and ground-truth generation for Object-Confidence.
