# AMD ROCm - Ubuntu 26.04 benchmark bundle

All 32 benchmarks for this platform, each pinned to its tested v1.0.3 commit as a git submodule.
Project home: https://github.com/garymichaelbass

```bash
git clone --recurse-submodules https://github.com/garys-gpu-benchmarks/bundle-amd-ubuntu-2604.git
cd bundle-amd-ubuntu-2604
./run.sh list                # the 32 benchmarks
./run.sh 305                 # smoke run of benchmark 305
./run.sh 305 --baseline      # standard run
./run.sh all                 # all 32, logs in results/
```

Clone without --recurse-submodules to download only what you run: ./run.sh fetches each benchmark on first use.

Do not use Download ZIP: GitHub's ZIP files leave the benchmarks/ folders empty.

## Benchmarks

- **301** - `benchmarks/amd-u26-301` - [301-sys-bench-amd-rocm-stack-validation-ubu2604](https://github.com/garys-gpu-benchmarks/301-sys-bench-amd-rocm-stack-validation-ubu2604)
- **302** - `benchmarks/amd-u26-302` - [302-sys-bench-amd-rocm-health-validation-ubu2604](https://github.com/garys-gpu-benchmarks/302-sys-bench-amd-rocm-health-validation-ubu2604)
- **303** - `benchmarks/amd-u26-303` - [303-sys-bench-amd-system-stress-stability-ubu2604](https://github.com/garys-gpu-benchmarks/303-sys-bench-amd-system-stress-stability-ubu2604)
- **304** - `benchmarks/amd-u26-304` - [304-gpu-bench-amd-sdc-ecc-integrity-ubu2604](https://github.com/garys-gpu-benchmarks/304-gpu-bench-amd-sdc-ecc-integrity-ubu2604)
- **305** - `benchmarks/amd-u26-305` - [305-gpu-bench-amd-pytorch-tensor-correctness-ubu2604](https://github.com/garys-gpu-benchmarks/305-gpu-bench-amd-pytorch-tensor-correctness-ubu2604)
- **306** - `benchmarks/amd-u26-306` - [306-sys-bench-amd-fio-nvme-sweep-ubu2604](https://github.com/garys-gpu-benchmarks/306-sys-bench-amd-fio-nvme-sweep-ubu2604)
- **307** - `benchmarks/amd-u26-307` - [307-sys-bench-amd-stream-ddr5-bandwidth-ubu2604](https://github.com/garys-gpu-benchmarks/307-sys-bench-amd-stream-ddr5-bandwidth-ubu2604)
- **308** - `benchmarks/amd-u26-308` - [308-sys-bench-amd-iperf3-network-performance-ubu2604](https://github.com/garys-gpu-benchmarks/308-sys-bench-amd-iperf3-network-performance-ubu2604)
- **309** - `benchmarks/amd-u26-309` - [309-sys-bench-amd-multichase-numa-latency-ubu2604](https://github.com/garys-gpu-benchmarks/309-sys-bench-amd-multichase-numa-latency-ubu2604)
- **310** - `benchmarks/amd-u26-310` - [310-sys-bench-amd-numa-cache-performance-ubu2604](https://github.com/garys-gpu-benchmarks/310-sys-bench-amd-numa-cache-performance-ubu2604)
- **311** - `benchmarks/amd-u26-311` - [311-sys-bench-amd-linux-perf-pmu-ubu2604](https://github.com/garys-gpu-benchmarks/311-sys-bench-amd-linux-perf-pmu-ubu2604)
- **312** - `benchmarks/amd-u26-312` - [312-sys-bench-amd-lmbench-microbench-suite-ubu2604](https://github.com/garys-gpu-benchmarks/312-sys-bench-amd-lmbench-microbench-suite-ubu2604)
- **313** - `benchmarks/amd-u26-313` - [313-sys-bench-amd-gups-random-memory-ubu2604](https://github.com/garys-gpu-benchmarks/313-sys-bench-amd-gups-random-memory-ubu2604)
- **314** - `benchmarks/amd-u26-314` - [314-gpu-bench-amd-hipmemcpy-transfer-bandwidth-ubu2604](https://github.com/garys-gpu-benchmarks/314-gpu-bench-amd-hipmemcpy-transfer-bandwidth-ubu2604)
- **315** - `benchmarks/amd-u26-315` - [315-gpu-bench-amd-babelstream-hbm-bandwidth-ubu2604](https://github.com/garys-gpu-benchmarks/315-gpu-bench-amd-babelstream-hbm-bandwidth-ubu2604)
- **316** - `benchmarks/amd-u26-316` - [316-gpu-bench-amd-rccl-bandwidth-test-ubu2604](https://github.com/garys-gpu-benchmarks/316-gpu-bench-amd-rccl-bandwidth-test-ubu2604)
- **317** - `benchmarks/amd-u26-317` - [317-gpu-bench-amd-gemm-rocblas-micro-ubu2604](https://github.com/garys-gpu-benchmarks/317-gpu-bench-amd-gemm-rocblas-micro-ubu2604)
- **318** - `benchmarks/amd-u26-318` - [318-gpu-bench-amd-miopen-convolution-micro-ubu2604](https://github.com/garys-gpu-benchmarks/318-gpu-bench-amd-miopen-convolution-micro-ubu2604)
- **319** - `benchmarks/amd-u26-319` - [319-gpu-bench-amd-torch-micro-suite-ubu2604](https://github.com/garys-gpu-benchmarks/319-gpu-bench-amd-torch-micro-suite-ubu2604)
- **320** - `benchmarks/amd-u26-320` - [320-gpu-bench-amd-linpack-rochpl-fp64-ubu2604](https://github.com/garys-gpu-benchmarks/320-gpu-bench-amd-linpack-rochpl-fp64-ubu2604)
- **321** - `benchmarks/amd-u26-321` - [321-gpu-bench-amd-resnet50-pytorch-training-ubu2604](https://github.com/garys-gpu-benchmarks/321-gpu-bench-amd-resnet50-pytorch-training-ubu2604)
- **322** - `benchmarks/amd-u26-322` - [322-gpu-bench-amd-resnet50-pytorch-inference-ubu2604](https://github.com/garys-gpu-benchmarks/322-gpu-bench-amd-resnet50-pytorch-inference-ubu2604)
- **323** - `benchmarks/amd-u26-323` - [323-gpu-bench-amd-bert-base-inference-ubu2604](https://github.com/garys-gpu-benchmarks/323-gpu-bench-amd-bert-base-inference-ubu2604)
- **324** - `benchmarks/amd-u26-324` - [324-gpu-bench-amd-sdxl-diffusers-latency-ubu2604](https://github.com/garys-gpu-benchmarks/324-gpu-bench-amd-sdxl-diffusers-latency-ubu2604)
- **325** - `benchmarks/amd-u26-325` - [325-gpu-bench-amd-distilbert-hf-classification-ubu2604](https://github.com/garys-gpu-benchmarks/325-gpu-bench-amd-distilbert-hf-classification-ubu2604)
- **326** - `benchmarks/amd-u26-326` - [326-gpu-bench-amd-jax-xla-forwardpass-ubu2604](https://github.com/garys-gpu-benchmarks/326-gpu-bench-amd-jax-xla-forwardpass-ubu2604)
- **327** - `benchmarks/amd-u26-327` - [327-gpu-bench-amd-vllm-kvcache-stress-ubu2604](https://github.com/garys-gpu-benchmarks/327-gpu-bench-amd-vllm-kvcache-stress-ubu2604)
- **328** - `benchmarks/amd-u26-328` - [328-gpu-bench-amd-vllm-throughput-latency-ubu2604](https://github.com/garys-gpu-benchmarks/328-gpu-bench-amd-vllm-throughput-latency-ubu2604)
- **329** - `benchmarks/amd-u26-329` - [329-gpu-bench-amd-vllm-mistral-rocm-ubu2604](https://github.com/garys-gpu-benchmarks/329-gpu-bench-amd-vllm-mistral-rocm-ubu2604)
- **330** - `benchmarks/amd-u26-330` - [330-gpu-bench-amd-sglang-prompt-response-ubu2604](https://github.com/garys-gpu-benchmarks/330-gpu-bench-amd-sglang-prompt-response-ubu2604)
- **331** - `benchmarks/amd-u26-331` - [331-gpu-bench-amd-sglang-serving-latency-ubu2604](https://github.com/garys-gpu-benchmarks/331-gpu-bench-amd-sglang-serving-latency-ubu2604)
- **332** - `benchmarks/amd-u26-332` - [332-gpu-bench-amd-rag-faiss-end2end-ubu2604](https://github.com/garys-gpu-benchmarks/332-gpu-bench-amd-rag-faiss-end2end-ubu2604)
