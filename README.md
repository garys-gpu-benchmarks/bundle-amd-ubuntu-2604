# AMD ROCm - Ubuntu 26.04 benchmark bundle

32 system and GPU benchmarks for AMD GPU machines running Ubuntu 26.04,
each pinned to its tested v1.0.9 release.

Use this bundle only on that platform. The others have their own bundle:
[AMD 24.04](https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2404) ·
[NVIDIA 24.04](https://github.com/garys-gpu-benchmarks/bundle-nvidia-ubuntu-2404) ·
[AMD 26.04](https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604) ·
[NVIDIA 26.04](https://github.com/garys-gpu-benchmarks/bundle-nvidia-ubuntu-2604)

## Before you start

- A freshly installed **Ubuntu 26.04** machine with AMD GPUs, connected to the internet.
- **root** access: log in as root, or run `sudo -i` first.
- Plenty of free disk space. Each benchmark installs its own software the first
  time it runs, and checks for at least 20 GiB free before it does. The AI
  benchmarks also download models.
- For the AI model benchmarks (323 to 331): if a model's license on Hugging
  Face requires it, accept the license there and set a token first:
  `export HF_TOKEN=hf_...`

## Install

```bash
apt-get update && apt-get install -y git tmux
git clone --recurse-submodules https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604 /opt/benchmarks
cd /opt/benchmarks
./run.sh list
```

`./run.sh list` should show all 32 benchmarks as `ready`.
Do not use GitHub's **Download ZIP**: it leaves the benchmark folders empty.

## Getting started: three quick benchmarks

```bash
cd /opt/benchmarks
./run_benchmark_suite.sh -w 301,302,315 -p smoke
```

This runs the short `smoke` profile of three benchmarks:

| Benchmark | What it checks |
|---|---|
| 301 | the GPU software stack is installed and working |
| 302 | GPU health |
| 315 | GPU memory (HBM) bandwidth |

The first run takes much longer than later ones: it installs ROCm and each
benchmark's software. The install may restart the machine once. If it does,
log in again after the restart, wait a few minutes for setup to finish on its
own, and run the same command again.

Each run prints a short block, and the suite ends with a pass/fail summary.
The full output is in `/opt/benchmarks/benchmark_suite_log/`.

To run a single benchmark with its output on screen:

```bash
./run.sh 305                # setup, then the smoke profile of benchmark 305
./run.sh 305 --baseline     # the standard profile
```

## Run everything

Run the full suite inside `tmux`, so it keeps running if your connection drops.
It takes many hours.

```bash
tmux new -s bench
cd /opt/benchmarks
./run_benchmark_suite.sh
```

This runs all 32 benchmarks with the `smoke` profile, then all with
`baseline`, then all with `extended`. Detach with **Ctrl+B** then **D**;
reattach later with `tmux attach -t bench`.

Run "Getting started" first on a new machine, so the one-time install (and any
restart) happens before the long run.

| Option | What it does |
|---|---|
| `-p smoke` | profiles to run, in order (`smoke`, `baseline`, `extended`; comma-separated) |
| `-w 301,307,321` | only these benchmarks; ranges work too: `-w 301-310` |
| `-r 20` | repeat the whole set 20 times (soak test) |
| `--fail-fast` | stop at the first failed run |
| `-n` | show what would run, without running it |
| `--help` | all options |

## Results

| What | Where |
|---|---|
| Summary of the suite | end of the screen output |
| Full output of every run | `/opt/benchmarks/benchmark_suite_log/` |
| Each benchmark's results | `/opt/benchmarks/<benchmark>/results/` (`raw/` and `parsed/`) |
| One line per run (time, exit code) | `/var/opt/benchmarks/runtime_ledger.csv` |

To copy everything to a Windows laptop, use `get_remote_info.sh` from
[gpu-bench-suite](https://github.com/garys-gpu-benchmarks/gpu-bench-suite).

## Update to a newer release

```bash
cd /opt/benchmarks
git pull && git submodule update --init --recursive
```

Copies made before v1.0.5 keep their benchmarks in a `benchmarks/` subfolder;
delete those and clone again as shown under Install.

## Benchmarks

- **301** - [301-sys-bench-amd-rocm-stack-validation-ubu2604](https://github.com/garys-gpu-benchmarks/301-sys-bench-amd-rocm-stack-validation-ubu2604)
- **302** - [302-sys-bench-amd-rocm-health-validation-ubu2604](https://github.com/garys-gpu-benchmarks/302-sys-bench-amd-rocm-health-validation-ubu2604)
- **303** - [303-sys-bench-amd-system-stress-stability-ubu2604](https://github.com/garys-gpu-benchmarks/303-sys-bench-amd-system-stress-stability-ubu2604)
- **304** - [304-gpu-bench-amd-sdc-ecc-integrity-ubu2604](https://github.com/garys-gpu-benchmarks/304-gpu-bench-amd-sdc-ecc-integrity-ubu2604)
- **305** - [305-gpu-bench-amd-pytorch-tensor-correctness-ubu2604](https://github.com/garys-gpu-benchmarks/305-gpu-bench-amd-pytorch-tensor-correctness-ubu2604)
- **306** - [306-sys-bench-amd-fio-nvme-sweep-ubu2604](https://github.com/garys-gpu-benchmarks/306-sys-bench-amd-fio-nvme-sweep-ubu2604)
- **307** - [307-sys-bench-amd-stream-ddr5-bandwidth-ubu2604](https://github.com/garys-gpu-benchmarks/307-sys-bench-amd-stream-ddr5-bandwidth-ubu2604)
- **308** - [308-sys-bench-amd-iperf3-network-performance-ubu2604](https://github.com/garys-gpu-benchmarks/308-sys-bench-amd-iperf3-network-performance-ubu2604)
- **309** - [309-sys-bench-amd-multichase-numa-latency-ubu2604](https://github.com/garys-gpu-benchmarks/309-sys-bench-amd-multichase-numa-latency-ubu2604)
- **310** - [310-sys-bench-amd-numa-cache-performance-ubu2604](https://github.com/garys-gpu-benchmarks/310-sys-bench-amd-numa-cache-performance-ubu2604)
- **311** - [311-sys-bench-amd-linux-perf-pmu-ubu2604](https://github.com/garys-gpu-benchmarks/311-sys-bench-amd-linux-perf-pmu-ubu2604)
- **312** - [312-sys-bench-amd-lmbench-microbench-suite-ubu2604](https://github.com/garys-gpu-benchmarks/312-sys-bench-amd-lmbench-microbench-suite-ubu2604)
- **313** - [313-sys-bench-amd-gups-random-memory-ubu2604](https://github.com/garys-gpu-benchmarks/313-sys-bench-amd-gups-random-memory-ubu2604)
- **314** - [314-gpu-bench-amd-hipmemcpy-transfer-bandwidth-ubu2604](https://github.com/garys-gpu-benchmarks/314-gpu-bench-amd-hipmemcpy-transfer-bandwidth-ubu2604)
- **315** - [315-gpu-bench-amd-babelstream-hbm-bandwidth-ubu2604](https://github.com/garys-gpu-benchmarks/315-gpu-bench-amd-babelstream-hbm-bandwidth-ubu2604)
- **316** - [316-gpu-bench-amd-rccl-bandwidth-test-ubu2604](https://github.com/garys-gpu-benchmarks/316-gpu-bench-amd-rccl-bandwidth-test-ubu2604)
- **317** - [317-gpu-bench-amd-gemm-rocblas-micro-ubu2604](https://github.com/garys-gpu-benchmarks/317-gpu-bench-amd-gemm-rocblas-micro-ubu2604)
- **318** - [318-gpu-bench-amd-miopen-convolution-micro-ubu2604](https://github.com/garys-gpu-benchmarks/318-gpu-bench-amd-miopen-convolution-micro-ubu2604)
- **319** - [319-gpu-bench-amd-torch-micro-suite-ubu2604](https://github.com/garys-gpu-benchmarks/319-gpu-bench-amd-torch-micro-suite-ubu2604)
- **320** - [320-gpu-bench-amd-linpack-rochpl-fp64-ubu2604](https://github.com/garys-gpu-benchmarks/320-gpu-bench-amd-linpack-rochpl-fp64-ubu2604)
- **321** - [321-gpu-bench-amd-resnet50-pytorch-training-ubu2604](https://github.com/garys-gpu-benchmarks/321-gpu-bench-amd-resnet50-pytorch-training-ubu2604)
- **322** - [322-gpu-bench-amd-resnet50-pytorch-inference-ubu2604](https://github.com/garys-gpu-benchmarks/322-gpu-bench-amd-resnet50-pytorch-inference-ubu2604)
- **323** - [323-gpu-bench-amd-bert-base-inference-ubu2604](https://github.com/garys-gpu-benchmarks/323-gpu-bench-amd-bert-base-inference-ubu2604)
- **324** - [324-gpu-bench-amd-sdxl-diffusers-latency-ubu2604](https://github.com/garys-gpu-benchmarks/324-gpu-bench-amd-sdxl-diffusers-latency-ubu2604)
- **325** - [325-gpu-bench-amd-distilbert-hf-classification-ubu2604](https://github.com/garys-gpu-benchmarks/325-gpu-bench-amd-distilbert-hf-classification-ubu2604)
- **326** - [326-gpu-bench-amd-jax-xla-forwardpass-ubu2604](https://github.com/garys-gpu-benchmarks/326-gpu-bench-amd-jax-xla-forwardpass-ubu2604)
- **327** - [327-gpu-bench-amd-vllm-kvcache-stress-ubu2604](https://github.com/garys-gpu-benchmarks/327-gpu-bench-amd-vllm-kvcache-stress-ubu2604)
- **328** - [328-gpu-bench-amd-vllm-throughput-latency-ubu2604](https://github.com/garys-gpu-benchmarks/328-gpu-bench-amd-vllm-throughput-latency-ubu2604)
- **329** - [329-gpu-bench-amd-vllm-mistral-rocm-ubu2604](https://github.com/garys-gpu-benchmarks/329-gpu-bench-amd-vllm-mistral-rocm-ubu2604)
- **330** - [330-gpu-bench-amd-sglang-prompt-response-ubu2604](https://github.com/garys-gpu-benchmarks/330-gpu-bench-amd-sglang-prompt-response-ubu2604)
- **331** - [331-gpu-bench-amd-sglang-serving-latency-ubu2604](https://github.com/garys-gpu-benchmarks/331-gpu-bench-amd-sglang-serving-latency-ubu2604)
- **332** - [332-gpu-bench-amd-rag-faiss-end2end-ubu2604](https://github.com/garys-gpu-benchmarks/332-gpu-bench-amd-rag-faiss-end2end-ubu2604)

