# 🐙 Gthulhu: Steering the Cloud-Native Depthstext
  ^(;,;)^
The Orchestrable Heart of Linux Scheduling


[![CNCF Landscape](https://img.shields.io/badge/CNCF-Landscape-blue?style=for-the-badge&logo=cncf)](https://landscape.cncf.io/)[![](https://img.shields.io/badge/eBPF-Application-orange?style=for-the-badge&logo=ebpf)](https://ebpf.io/applications/)[![Go Version](https://img.shields.io/badge/Go-1.22+-00ADD8?style=for-the-badge&logo=go&logoColor=white)](https://go.dev/)[![License](https://img.shields.io/badge/License-Apache_2.0-green.svg?style=for-the-badge)](https://opensource.org/licenses/Apache-2.0)

**Gthulhu** is an orchestrable, distributed scheduler platform designed for the cloud-native ecosystem. By leveraging the **Linux Scheduler Extension (sched_ext)** and **eBPF**, Gthulhu enables developers to bypass the limitations of traditional fair-share scheduling (CFS/EEVDF) and optimize for specific, high-performance workloads like trading systems, big data analytics, and 5G network processing.

---

## 🏛️ Philosophy: Technical Prowess & Cultural Roots

- **The Name**: **Gthulhu** is a portmanteau of **Go** and **Cthulhu**. Its "tentacles" symbolize the ability to grasp and steer complex distributed systems, much like the ship's wheel (Helm) of Kubernetes.
- **The Core**: Our underlying framework, **qumun**, is named after the **Bunun** (Indigenous people of Taiwan) word for "**Heart**". Just as the heart drives the rhythm of life, qumun serves as the heartbeat of the operating system, orchestrating tasks and resource cycles.

---

## 🏗️ Architecture: Distributed Control Plane

Gthulhu ensures coordinated scheduling across a multi-node cluster through three key layers:

1.  **Manager (Global Plane)**: Central control point that transforms high-level "scheduling intents" (via REST API) into actionable policies.
2.  **Decision Maker (Local Plane)**: A sidecar running on each node that identifies target processes (PIDs) and maps global policies to local execution.
3.  **Scheduler Agent & qumun (Execution Plane)**: The eBPF-powered agent that interacts directly with the Linux kernel to apply millisecond-level scheduling decisions.

---

## 🗺️ Repository Map

| Repository | Role | Tech Stack |
| :--- | :--- | :--- |
| [**Gthulhu/Gthulhu**](https://github.com/Gthulhu/Gthulhu) | **Main Repo**: The orchestrable distributed scheduler core. | Go, C, eBPF |
| [**Gthulhu/qumun**](https://github.com/Gthulhu/qumun) | **Framework**: Go interface for custom Linux schedulers. | C, Go |
| [**Gthulhu/api**](https://github.com/Gthulhu/api) | **Interface**: Communication protocols and policy definitions. | Go (REST) |
| [**Gthulhu/plugin**](https://github.com/Gthulhu/plugin) | **Strategies**: Pre-built user-space scheduling plugins. | Go |
| [**Gthulhu/chart**](https://github.com/Gthulhu/chart) | **Deployment**: Helm charts for K8s integration. | Helm |
| [**Gthulhu/mcp**](https://github.com/Gthulhu/mcp) | **Future**: AI-driven scheduling via MCP protocol. | TypeScript |

---

## 🚀 Optimized Workloads

- ⚖️ **Low-Latency**: Financial trading systems and gaming servers requiring zero-preemption.
- 📊 **High-Throughput**: Big data (Spark) and Machine Learning training to maximize CPU cycles.
- 📶 **5G Telecom**: Optimized Data Plane packet processing to reach the URLLC.

---

## 🤝 Join the Cult(ure)

We welcome acolytes from all corners of the open-source world!

- **Documentation**: Visit [gthulhu.org](https://gthulhu.org) for technical deep dives.
- **Prerequisites**: Ensure you are running **Linux Kernel 6.12+** with `sched_ext` enabled.
- **Contribute**: Feel free to open [Issues](https://github.com/Gthulhu/Gthulhu/issues) or join our(https://github.com/Gthulhu/Gthulhu/discussions).