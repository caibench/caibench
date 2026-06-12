<div align="center">
  <img src="assets/cai-logo.png" width="160" alt="CAI Benchmark">

  # CAI Benchmark

  **The open benchmark for AI storage & memory.**

  Measures how client SSDs and DRAM actually perform under local-AI workloads —
  model load (TTFT), KV-cache offload, RAG queries, tensor streaming — not
  synthetic transfer tests.

  [**Download**](https://github.com/caibench/caibench/releases/latest) ·
  [Website](https://caibench.org) ·
  [Methodology](https://caibench.org/methodology.html) ·
  [Report an issue](https://github.com/caibench/caibench/issues)
</div>

---

## What it measures

| Suite | Workloads |
|---|---|
| **Storage** | Sequential model read · Tensor burst I/O (QD1 + think time) · Model load TTFT (real llama.cpp) · KV-cache offload (ShareGPT trace replay) · RAG mixed R/W · Image-gen offload · Checkpoint write · Model swap · LoRA adapter load · Speculative decoding · Concurrent model load |
| **Memory** | STREAM bandwidth (Copy/Scale/Add/Triad) · Random-access latency · Tensor allocation · L1/L2/L3/DRAM cache hierarchy · Performance under memory pressure · DRAM-tier KV cache |

Three SPEC-style scores: **Storage**, **Memory**, and **AI Readiness**
(45% storage + 55% memory, weighted for memory-bound LLM inference).
Score 50 = the published reference system (good Gen4 NVMe + DDR5-4800).
The default CLI suite runs the core workloads; checkpoint write, cache
hierarchy, memory pressure, and the advanced storage workloads are
selectable in the GUI. The KV-cache workloads use a real ShareGPT
conversation trace when present (fetch it with the model downloader) and
a statistical model of the same shape otherwise.

## Quickstart

1. Download the latest release for your platform below.
2. Run elevated (cache clearing needs it):
   - **Windows:** right-click `CAI_Benchmark_GUI.exe` → *Run as administrator*
   - **Linux:** `sudo ./cai_benchmark_cli --all`
3. Get your three scores. Export details with `--json` / `--xlsx`.

```text
CAI_Benchmark_CLI --all --runs 3 --json results.json
```

## Why trust the numbers

- **Direct I/O** — the OS cache is bypassed; reads come from the device.
- **Cache purging** — page cache and standby list cleared between runs, with
  success/failure recorded in the results.
- **Physical-limit validation** — headline results (model load, sequential
  read) exceeding the PCIe 5.0 ×4 single-drive ceiling (~15.75 GB/s) are
  rejected or warning-flagged as cache contamination, never celebrated as
  records.
- **Versioned methodology** — scores are labeled (e.g. `CAI v1.0`) and reference
  values are frozen per version, so published numbers never silently change
  meaning. Full methodology: [caibench.org/methodology](https://caibench.org/methodology.html)

## Verify your download

Every release includes `SHA256SUMS.txt`:

```powershell
# Windows
Get-FileHash .\CAI_Benchmark_GUI.exe -Algorithm SHA256
```

```bash
# Linux
sha256sum -c SHA256SUMS.txt
```

## About this repository

This repository hosts **releases, documentation, and issue tracking** for CAI
Benchmark. Use [Issues](https://github.com/caibench/caibench/issues) for bug
reports and [Discussions](https://github.com/caibench/caibench/discussions) for
results, questions, and methodology feedback.

## License

CAI Benchmark is distributed under the MIT License. © 2026 CAI Benchmark Project.
