# 🐙 Gthulhu

**From Kubernetes resource allocation to Linux task scheduling.**

[![CNCF Landscape](https://img.shields.io/badge/CNCF-Landscape-blue?style=for-the-badge&logo=cncf)](https://landscape.cncf.io/)
[![eBPF Application](https://img.shields.io/badge/eBPF-Application-orange?style=for-the-badge&logo=ebpf)](https://ebpf.io/applications/)
[![License](https://img.shields.io/badge/License-Apache_2.0-green.svg?style=for-the-badge)](https://opensource.org/licenses/Apache-2.0)

Gthulhu is a cloud-native runtime scheduling project built around Kubernetes, eBPF, and Linux `sched_ext`.

Our current direction is **Claim2Core**:

> **DRA chooses what and where; Gthulhu controls how it actually runs.**

Kubernetes can allocate a workload to a Node, GPU, NIC, CPU set, NUMA domain, or other device. Gthulhu focuses on the execution gap that follows: mapping workload intent and actual allocation to the Linux tasks that need CPU service, then applying bounded runtime policy through `sched_ext` and verifying the result with eBPF telemetry.

```text
Kueue / Workload API
        │ admission / quota
        ▼
kube-scheduler / DRA
        │ Node + device + topology allocation
        ▼
Gthulhu Runtime Plane
        │ Claim → Pod/cgroup → TGID/TID
        ▼
sched_ext + eBPF
        │ runtime policy + verification
        ▼
Delivered workload SLO
```

## What exists today

- pod-level scheduling observability with eBPF;
- Prometheus / Grafana / KEDA integration;
- distributed scheduling intents through a Manager and per-node Decision Makers;
- custom CPU scheduling on Linux 6.12+ with `sched_ext`;
- TID-aware node-policy matching for non-leader worker threads;
- user-space and kernel-mode priority handling with explicit non-boosting semantics.

The next roadmap steps focus on DRA semantic correctness, read-only `ResourceClaim` observation, Claim-to-Task provenance, and a static DRA-aware execution policy. See [Gthulhu 2026 Roadmap — Claim2Core](https://github.com/Gthulhu/Gthulhu/issues/141).

## Repository map

| Repository | Status | Role |
|---|---|---|
| [**Gthulhu/Gthulhu**](https://github.com/Gthulhu/Gthulhu) | Active | Main runtime/control-plane implementation |
| [**Gthulhu/qumun**](https://github.com/Gthulhu/qumun) | Active | Go framework for custom `sched_ext` schedulers |
| [**Gthulhu/plugin**](https://github.com/Gthulhu/plugin) | Active | User-space scheduling strategy implementation |
| [**Gthulhu/docs**](https://github.com/Gthulhu/docs) | Active | Official documentation at [gthulhu.org](https://gthulhu.org) |
| [**Gthulhu/gtp5g-operator**](https://github.com/Gthulhu/gtp5g-operator) | Active | Telecom / GTP5G integration work |
| [**Gthulhu/kina**](https://github.com/Gthulhu/kina) | Active | Related runtime experimentation |
| [**Gthulhu/libbpfgo**](https://github.com/Gthulhu/libbpfgo) | Active | Project-maintained libbpfgo fork |
| [**Gthulhu/api**](https://github.com/Gthulhu/api) | Archived | Historical standalone API repository; code has moved into the main repository |
| [**Gthulhu/chart**](https://github.com/Gthulhu/chart) | Archived | Historical standalone Helm chart repository; deployment assets now live with the main project |
| [**Gthulhu/mcp**](https://github.com/Gthulhu/mcp) | Archived | Experimental MCP work |

## Current research / engineering themes

- **Claim2Core** — `ResourceClaim → Pod/cgroup → TGID/TID → sched_ext` execution lineage.
- **Device-local execution domains** — respect allocated CPU/cgroup boundaries while preferring NUMA / PCIe locality.
- **Scheduling provenance** — preview, explain, intended-vs-actual runtime state, and stale-state verification.
- **Workload adapters** — free5GC/UPF first for fast end-to-end validation; LLM prefill/decode/NCCL roles for accelerator-focused research.
- **Closed-loop runtime control** — only after static policy and provenance are trustworthy.

## Join the project

- **Main repository:** [Gthulhu/Gthulhu](https://github.com/Gthulhu/Gthulhu)
- **Roadmap:** [Issue #141](https://github.com/Gthulhu/Gthulhu/issues/141)
- **Documentation:** [gthulhu.org](https://gthulhu.org)
- **Contributing:** [Contribution guide](https://gthulhu.org/contributing/)
- **Discussions:** [GitHub Discussions](https://github.com/Gthulhu/Gthulhu/discussions)

Gthulhu is Apache-2.0 licensed and welcomes contributions across Kubernetes, DRA, eBPF, `sched_ext`, NUMA/topology, observability, telecom, and accelerator workloads.
