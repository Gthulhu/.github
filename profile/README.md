# 🐙 Gthulhu

**Kubernetes places the workload. Gthulhu controls how it gets CPU time.**

[![CNCF Landscape](https://img.shields.io/badge/CNCF-Landscape-blue?style=for-the-badge&logo=cncf)](https://landscape.cncf.io/)
[![eBPF Application](https://img.shields.io/badge/eBPF-Application-orange?style=for-the-badge&logo=ebpf)](https://ebpf.io/applications/)
[![License](https://img.shields.io/badge/License-Apache_2.0-green.svg?style=for-the-badge)](https://opensource.org/licenses/Apache-2.0)

Gthulhu is a cloud-native **runtime scheduling** project built around Kubernetes, eBPF, and Linux `sched_ext`.

Kubernetes is good at deciding **where** a workload should run and **what resources** it should receive. Gthulhu focuses on what happens next: mapping that workload intent to the Linux tasks that actually need CPU service, applying node-local scheduling policy, and observing whether runtime behavior matches the intent.

> **Kubernetes decides where. Gthulhu controls how CPU time is delivered.**

[**Get started →**](https://gthulhu.org/k8s/) · [**How it works →**](https://gthulhu.org/how-it-works/) · [**Claim2Core roadmap →**](https://gthulhu.org/claim2core/)

## From workload intent to CPU dispatch

![Gthulhu workflow from workload intent to per-node sched_ext CPU dispatch](assets/gthulhu-workflow.gif)

Read the flow in four steps:

1. **Express intent** — a user or controller creates a scheduling strategy through the Web/API and Kubernetes CRDs.
2. **Resolve the workload** — the Manager watches Kubernetes state and turns the strategy into policy for the nodes actually running the workload.
3. **Decide per node** — every node has its own Decision Maker and Gthulhu Scheduler, so policy is resolved against node-local workload/task state.
4. **Apply at CPU dispatch** — the node-local scheduler programs eBPF `sched_ext`, which controls dispatch queues and CPU placement.

The result is a clean split of responsibility: Kubernetes keeps ownership of **placement and resource allocation**; Gthulhu handles **runtime CPU scheduling for the selected tasks**.

## Why Gthulhu exists

A Pod can be on the right Node and still miss its SLO. Under CPU contention, an `EngineCore`, feeder thread, packet-processing worker, IRQ-related task, or other critical thread may wait behind background work even though Kubernetes already allocated the workload's resources.

Gthulhu closes that execution gap with three capabilities:

- **Observe** — use eBPF to see task waiting, execution, migration, and CPU contention.
- **Target** — resolve Kubernetes workload context to the process or thread that actually matters.
- **Control** — use Linux `sched_ext` to apply bounded scheduling policy without maintaining a kernel fork.

## Evidence, not blanket performance claims

- **free5GC / 5G user plane:** in a published CPU-stress case study, average UE ping latency fell from **88.98 ms to 2.079 ms**, with maximum latency falling from **130.95 ms to 8.45 ms**. [Read the case study →](https://free5gc.org/blog/20251126/20251126/)
- **vLLM / host-side scheduling:** our experiments show that CPU contention can become part of the inference critical path in some setups, while other workloads remain GPU- or network-bound. [Read the benchmark analysis →](https://gthulhu.org/vllm-cpu-pressure/)

Gthulhu is most useful when **CPU scheduling is actually part of the workload bottleneck**. The project aims to make that measurable instead of assuming every workload needs a custom scheduler.

## What exists today

- pod-level scheduling observability with eBPF;
- Prometheus / Grafana / KEDA integration;
- distributed scheduling intent through a Manager and per-node Decision Makers;
- custom CPU scheduling on Linux 6.12+ with `sched_ext`;
- TID-aware node-policy matching for non-leader worker threads;
- user-space and kernel-mode priority handling with explicit non-boosting semantics.

## Claim2Core: connect allocation to execution

Our current direction is **Claim2Core**: carry Kubernetes' actual resource allocation all the way to runtime task scheduling.

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

The principle is simple: observe `ResourceClaim` allocation, preserve the CPU/resource boundaries Kubernetes established, map that allocation to the right Linux tasks, then apply and verify runtime policy.

See [Gthulhu 2026 Roadmap — Claim2Core](https://github.com/Gthulhu/Gthulhu/issues/141).

## Repository map

| Repository | Role |
|---|---|
| [**Gthulhu/Gthulhu**](https://github.com/Gthulhu/Gthulhu) | Main runtime/control-plane implementation |
| [**Gthulhu/qumun**](https://github.com/Gthulhu/qumun) | Go framework for custom `sched_ext` schedulers |
| [**Gthulhu/plugin**](https://github.com/Gthulhu/plugin) | User-space scheduling strategies |
| [**Gthulhu/docs**](https://github.com/Gthulhu/docs) | Official documentation at [gthulhu.org](https://gthulhu.org) |
| [**Gthulhu/gtp5g-operator**](https://github.com/Gthulhu/gtp5g-operator) | Telecom / GTP5G integration work |
| [**Gthulhu/libbpfgo**](https://github.com/Gthulhu/libbpfgo) | Project-maintained libbpfgo fork |

Archived experimental or historical repositories remain available under the organization for reference.

## Join the project

- **Main repository:** [Gthulhu/Gthulhu](https://github.com/Gthulhu/Gthulhu)
- **Documentation:** [gthulhu.org](https://gthulhu.org)
- **Roadmap:** [Issue #141](https://github.com/Gthulhu/Gthulhu/issues/141)
- **Contributing:** [Contribution guide](https://gthulhu.org/contributing/)
- **Discussions:** [GitHub Discussions](https://github.com/Gthulhu/Gthulhu/discussions)

Gthulhu is Apache-2.0 licensed. Contributions across Kubernetes, DRA, eBPF, `sched_ext`, NUMA/topology, observability, telecom, and accelerator workloads are welcome.
