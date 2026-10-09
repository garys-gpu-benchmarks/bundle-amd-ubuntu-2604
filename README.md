# AMD ROCm - Ubuntu 26.04 benchmark bundle

All 32 benchmarks for this platform, each pinned to its tested v1.0.6 commit as a git submodule.
Project home: https://github.com/garymichaelbass

## Install

On a fresh Ubuntu machine, this downloads all 32 benchmarks into /opt/benchmarks:

```bash
git clone --recurse-submodules https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604 /opt/benchmarks
```

As a normal user (not root), first install git and create the folder:

```bash
sudo apt-get update && sudo apt-get install -y git
sudo mkdir -p /opt/benchmarks && sudo chown "$USER":"$USER" /opt/benchmarks
git clone --recurse-submodules https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604 /opt/benchmarks
```

Each benchmark is then in its own folder, for example /opt/benchmarks/301-…, next to run.sh and run_benchmark_suite.sh. Check with:

```bash
cd /opt/benchmarks && ./run.sh list
```

`./run.sh list` should show all 32 benchmarks as `ready`. To download only the benchmarks you run, leave out `--recurse-submodules`: `./run.sh` then fetches each benchmark the first time it is used.

Do not use Download ZIP: GitHub's ZIP files leave the benchmark folders empty.

## Run

```bash
cd /opt/benchmarks
./run.sh 305                 # setup, then a smoke run of benchmark 305
./run.sh 305 --baseline      # standard run
./run.sh all                 # all 32, logs in results/
```

The first benchmark's setup may install the GPU software stack, ask for your sudo password, and need a reboot. Read each benchmark's README.md before running it.

## Run the whole suite

`run_benchmark_suite.sh` runs every installed benchmark with each profile (smoke, then baseline, then extended), prints one short block per run, keeps the full output in a log file, and ends with a pass/fail summary.

```bash
cd /opt/benchmarks
./run_benchmark_suite.sh                         # all 32 benchmarks, smoke + baseline + extended
./run_benchmark_suite.sh -p smoke                # quick check of everything
./run_benchmark_suite.sh -w 301,307,321 -p baseline  # chosen benchmarks only
./run_benchmark_suite.sh -r 20                   # repeat the whole set 20 times
./run_benchmark_suite.sh --help                  # all options
```

## Update

```bash
cd /opt/benchmarks
git pull && git submodule update --init --recursive
```

If your copy keeps its benchmarks in a benchmarks/ subfolder (bundles published before v1.0.5), delete it and clone again instead, as shown under Install.

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

