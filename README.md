# AMD ROCm - Ubuntu 26.04 benchmark bundle

32 system and GPU benchmarks for AMD GPU machines running Ubuntu 26.04,
each pinned to its tested v1.0.17 release.

Use this bundle only on that platform. The others have their own bundle:
[AMD 24.04](https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2404) ·
[NVIDIA 24.04](https://github.com/garys-gpu-benchmarks/bundle-nvidia-ubuntu-2404) ·
[AMD 26.04](https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604) ·
[NVIDIA 26.04](https://github.com/garys-gpu-benchmarks/bundle-nvidia-ubuntu-2604)

## Before you start

- A freshly installed **Ubuntu 26.04** machine with AMD GPUs, connected to the internet.
- **root** access: log in as root, or run `sudo -i` first.
- Plenty of free disk space. Each benchmark installs its own software (its own
  Python environment) the first time it runs, and checks for at least 20 GiB
  free before it does. That check is per benchmark, not for the whole suite,
  and the AI benchmarks also download models.
- A machine where installing ROCm and one restart are acceptable: the first
  run installs ROCm and restarts the machine once.
- For the AI model benchmarks (323 to 332): if a model's license on Hugging
  Face requires it, accept the license there and set a token first:
  `export HF_TOKEN=hf_...`

## Install

```bash
apt-get update && apt-get install -y git tmux
git clone --recurse-submodules https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604 /opt/benchmarks
cd /opt/benchmarks
./run_benchmark_suite.sh --list
```

`./run_benchmark_suite.sh --list` should show all 32 benchmarks as `ready`.
Do not use GitHub's **Download ZIP**: it leaves the benchmark folders empty.

## Getting started

Each command below ends with a summary like the samples shown. Your times,
host name and log file name will differ; what matters is `0 failed`. If a run
fails, the summary names it under `Failed runs:`; open the log file named on
the summary's last line.

### 1. A CPU benchmark (about a minute, no GPU software)

```bash
cd /opt/benchmarks
./run_benchmark_suite.sh -w 307 -p smoke --show-output
```

307 is STREAM, CPU memory bandwidth. It installs only a compiler and its own
Python environment, not ROCm, and does not restart the machine. It ends
with Copy, Scale, Add and Triad rates in GB/s. Smoke uses 4 threads; the
`baseline` profile is the figure to quote for this machine. If this run
passes, the bundle, the suite runner, the results and the runtime ledger all
work. It ends like this:

```text
======================================================================
SUMMARY: 1 of 1 runs finished: 1 passed, 0 failed
Started:       Mon Oct 19 22:03:51 UTC 2026
Completed:     Mon Oct 19 22:04:35 UTC 2026
Total runtime: 0 hours 0 minutes 44 seconds
Log file:      /opt/benchmarks/benchmark_suite_log/run_benchmark_suite_<host>_20261019_220351.log
======================================================================
```

To see that run in the runtime ledger: `./show_metrics.sh 307`

### 2. The GPU software and two quick GPU checks (5 to 20 minutes the first time)

The first time, 301 installs ROCm, which takes 5 to 20 minutes. Run it
either way:

```bash
./run_benchmark_suite.sh -w 301,302,314 -p smoke --show-output   # shows progress while ROCm installs
./run_benchmark_suite.sh -w 301,302,314 -p smoke                 # quiet: prints nothing for 5 to 20 minutes
```

In quiet mode, watch progress from a second window with
`tail -f /opt/benchmarks/301-*/results/install_status.txt`.

| Benchmark | What it checks |
|---|---|
| 301 | installs ROCm, then checks the installed GPU software (the long step) |
| 302 | GPU health: the GPUs are visible, with temperature and link status |
| 314 | copy bandwidth between the host and the GPU, measured at 256 MiB |

**The machine restarts once.** Installing ROCm on Ubuntu 26.04 restarts the
machine, which closes your connection. With `--show-output`, the output ends
like this (in quiet mode, only the last line appears):

```text
Successfully installed PyYAML-6.0.3
[WARN] Setup is awaiting reboot-resume completion. Rerun bash run_benchmark.sh after setup completes.
Connection to <your-machine-address> closed by remote host.
```

This is planned, not a failure:

1. Log in again as root after a few minutes. Setup continues on its own after
   the restart.
2. To watch it: `tail -f /opt/benchmarks/301-*/results/install_status.txt`
   (Ctrl+C stops watching, not setup).
3. Run the same command again. If it says setup is still running, wait a few
   minutes and run it again. Benchmarks already installed are not installed
   again.

After the restart, the same command ends like this when all three pass:

```text
======================================================================
SUMMARY: 3 of 3 runs finished: 3 passed, 0 failed
Started:       Mon Oct 19 22:27:43 UTC 2026
Completed:     Mon Oct 19 22:28:18 UTC 2026
Total runtime: 0 hours 0 minutes 35 seconds
Log file:      /opt/benchmarks/benchmark_suite_log/run_benchmark_suite_<host>_20261019_222743.log
======================================================================
```

### 3. Smoke is a wiring check, not a measurement

Smoke shows that each benchmark installs, runs and records its results. Most
smoke runs use small sizes, so their numbers are not for reports: for example,
the 315 (GPU memory bandwidth) smoke arrays fit in GPU cache and do not
measure HBM. Quote `baseline` or `extended` results. Smoke is short for most
benchmarks, but the AI benchmarks (323 to 332) download models the first
time, so their first smoke run takes longer.

### 4. Run one benchmark

Each benchmark installs its own software the first time it runs, so a first
run takes longer than later ones. Each run prints a short block, and the suite
ends with the summary shown above. The full output is in
`/opt/benchmarks/benchmark_suite_log/`. To run a single benchmark with its
output on screen:

```bash
./run_benchmark_suite.sh -w 305 -p smoke --show-output      # benchmark 305, smoke profile
./run_benchmark_suite.sh -w 305 -p baseline --show-output   # the standard profile
```

## See your results

`show_metrics.sh` prints one benchmark's runs from the runtime ledger: one row
per run with its metrics, then the mean for each profile.

```bash
cd /opt/benchmarks
./show_metrics.sh 307                # benchmark 307, every profile
./show_metrics.sh 307 --baseline     # baseline runs only
./show_metrics.sh --help             # all options
```

To copy the results to your laptop, see [Results](#results).

## Run everything

Run the full suite inside `tmux`, so it keeps running if your connection drops.
It takes many hours.

```bash
tmux new -s bench
cd /opt/benchmarks
./run_benchmark_suite.sh -p smoke,baseline,extended
```

This runs all 32 benchmarks with the `smoke` profile, then all with
`baseline`, then all with `extended`. Without `-p`, `./run_benchmark_suite.sh`
runs the `smoke` profile only. Detach with **Ctrl+B** then **D**;
reattach later with `tmux attach -t bench`.

Run "Getting started" first on a new machine, so the one-time install (and any
restart) happens before the long run.

| Option | What it does |
|---|---|
| `-p baseline` | profiles to run, in order (`smoke`, `baseline`, `extended`; comma-separated). Default: `smoke` |
| `-w 301,307,321` | only these benchmarks; ranges work too: `-w 301-310` |
| `-r 20` | repeat the whole set 20 times (soak test) |
| `--fail-fast` | stop at the first failed run |
| `--show-output` | show each run's full output on screen too (it always goes to the log) |
| `--list` | list the benchmarks as `ready` or `not fetched`, then exit |
| `-n` | show what would run, without running it |
| `--help` | all options |

## Results that are expected to be `na`

A metric written as `na (reason)` was not measured on this machine; it is
never an invented number. Read the reason before reporting a fault. These are
expected:

| Benchmark | Expected result |
|---|---|
| 316 | On a machine with one GPU, the bandwidth metrics are `na (requires at least two GPUs)` and the run still passes. |
| 311 | Hardware counters are `na` with the reason when `perf` is not installed for the running kernel, or when `perf_event_paranoid` denies access. |
| 306 | Measures a file (`data/fio_target.bin`) on the disk that holds the benchmark, not a raw NVMe device. |
| 308 | Measures the loopback path (`127.0.0.1`), not a network card. |
| 330, 331 | The `smoke` profile may use a stand-in server; only `baseline` and `extended` measure SGLang. |

## Results

| What | Where |
|---|---|
| Summary of the suite | end of the screen output |
| Full output of every run | `/opt/benchmarks/benchmark_suite_log/` |
| Each benchmark's results | `/opt/benchmarks/<benchmark>/results/` (`raw/` and `parsed/`) |
| One line per run (time, exit code) | `/var/opt/benchmarks/runtime_ledger.csv` |
| One benchmark's runs and per-profile means | `./show_metrics.sh <benchmark>` |
| Which release is installed | `git -C /opt/benchmarks describe --tags` |

To copy the ledger, every benchmark's `results/` and the suite logs to your
laptop (Git Bash on Windows), use `get_remote_info.sh` from
[gpu-bench-suite](https://github.com/garys-gpu-benchmarks/gpu-bench-suite). It writes
`RUNTIME_LEDGERS/` and `DIRECTORIES/` in the folder you run it from:

```bash
git clone https://github.com/garys-gpu-benchmarks/gpu-bench-suite && cd gpu-bench-suite
./scripts/get_remote_info.sh --host 203.0.113.10       # this machine's address; --port PORT if ssh is not on 22
./scripts/show_metrics.sh 307 --host 203.0.113.10      # the same table, read over ssh
```

## Update to a newer release

```bash
cd /opt/benchmarks
git pull && git submodule update --init --recursive
```

Do not use `git submodule update --remote`: it moves the benchmarks off the
release they were tested at.

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

