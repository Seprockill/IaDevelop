# Technical & Domain Glossary

> Companion to SPEC-ai-compute-platform and ARCHITECTURE-SPINE

| Term | Category | Definition |
| :--- | :--- | :--- |
| **Standard GPU Node** | Hardware | Physical compute node equipped with a single 24 GB VRAM GPU dedicated to interactive student lab sessions, course assignments, and lightweight model inference/fine-tuning. |
| **High-Capacity GPU Node** | Hardware | Physical compute node equipped with dual 48 GB VRAM GPUs (96 GB aggregate VRAM) and high-speed inter-GPU links dedicated to multi-GPU research training and heavy LLM workloads. |
| **Kubeflow Training Operator** | Kubernetes / ML | Kubernetes extension providing custom resource definitions (CRDs) such as `PyTorchJob` and `TFJob` for orchestrated distributed multi-node model training. |
| **NVIDIA GPU Operator** | Infrastructure | Kubernetes operator automating the deployment and management of all NVIDIA software components (drivers, container toolkit, device plugin, DCGM). |
| **DCGM (Data Center GPU Manager)** | Telemetry | NVIDIA suite of tools providing low-overhead in-band GPU telemetry including active VRAM utilization, power draw, temperature, throttling status, and XID hardware error events. |
| **Token Metering & Bucket** | Governance | Accounting model representing GPU compute capacity as time-decaying tokens assigned to departments and individuals to prevent compute hoarding and encourage off-peak utilization. |
| **Classroom Lockout** | Scheduling | Policy-driven pre-emption window that locks a pre-configured quantity of standard GPU nodes 15 minutes before scheduled course hours, ensuring zero class disruption. |
| **Gang Scheduling** | Scheduling | Scheduling algorithm that ensures all worker pods of a distributed training job are allocated simultaneously, preventing deadlock where some pods hold resources while waiting for others. |
| **Graceful Preemption Drain** | Lifecycle | 15-minute grace window during which low-priority preemptible jobs receive a `SIGTERM` signal to trigger state checkpointing before physical container eviction. |
| **XID Error** | Telemetry | Hardware/driver error code reported by the NVIDIA GPU driver indicating severe physical fault, uncorrectable memory corruption, or bus drop. |
