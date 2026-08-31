---
id: SPEC-ai-compute-platform
slug: ai-compute-platform
version: 1.0.0
created: 2026-08-31
status: canonical
companions:
  - "ARCHITECTURE-SPINE.md"
  - "companion-files/glossary.md"
  - "companion-files/persona-archetypes.md"
  - "diagrams/system-context.mmd"
  - "diagrams/state-transitions.mmd"
sources:
  - "design-artifacts/A-Product-Brief/project-brief.md"
---

> **Canonical contract.** This SPEC is the complete, preservation-validated machine contract for what to build, test, and validate for the University AI Compute Management Platform.

# University AI Compute Management Platform Specification

## Why

University AI education and cutting-edge research share a constrained, heterogeneous on-premise GPU fleet (24 GB standard nodes and 2x48 GB high-capacity nodes), currently compromised by manual, ad-hoc access mechanisms (unmonitored SSH keys and spreadsheet sign-ups). This causes severe dual-sided friction: scheduled undergraduate and graduate classes suffer unpredictable outages when rogue background processes monopolize GPU memory, while multi-node research training jobs experience starvation, fragmentation, and abrupt manual terminations. Centralized, automated Kubernetes and Kubeflow governance is required immediately to enforce deterministic classroom reservations, guarantee fair multi-tenant quota distribution, and maximize sustained fleet-wide hardware utilization without expanding administrative overhead.

## Capabilities

- **CAP-1** (Multi-Tier Quota & Token Metering)
  - **intent:** System meters GPU compute-hours and enforces per-user and per-department time-decaying token quotas across standard (24 GB) and high-capacity (2x48 GB) GPU tiers.
  - **success:** Any workload launch request submitted by a user or department exceeding their allocated GPU-hour quota is rejected at admission time with an actionable quota denial response (HTTP 429 / admission webhook rejection within 500ms), while requests within quota boundaries are admitted and queued within 500ms.

- **CAP-2** (Classroom Reservation & GPU Lockout)
  - **intent:** Instructors can lock dedicated slices of standard GPU nodes for scheduled academic calendar windows with automated preemption and evacuation of non-instructional workloads.
  - **success:** 100% of reserved standard GPU nodes are completely evacuated of preemptible non-classroom workloads at least 15 minutes prior to the scheduled class start time, guaranteeing a 0.0% class session launch failure rate.

- **CAP-3** (Heterogeneous Hardware Placement)
  - **intent:** System inspects pod resource specifications and routes workloads automatically to the optimal node tier based on VRAM capacity (24 GB Standard vs 2x48 GB High-Capacity).
  - **success:** Single-GPU jobs requiring $\le 24$ GB VRAM are never scheduled onto 2x48 GB high-capacity nodes when standard nodes have available capacity, and jobs requiring $> 24$ GB VRAM or multi-GPU gang scheduling are routed exclusively to 2x48 GB nodes with zero placement mismatches.

- **CAP-4** (Distributed Training Workload Lifecycle)
  - **intent:** Researchers can launch, monitor, checkpoint, and resume multi-GPU and multi-node Kubeflow training jobs with elastic preemption tolerance.
  - **success:** Distributed training jobs (PyTorchJob / TFJob) initiate gang-scheduling across high-capacity nodes, execute state checkpointing upon receipt of a SIGTERM preemption signal within a 15-minute grace window, and resume from the latest valid checkpoint within 120 seconds of compute resource availability.

- **CAP-5** (Cluster Telemetry & Node Eviction)
  - **intent:** System continuously ingests real-time hardware telemetry (VRAM saturation, compute utilization, thermal, power) and automatically cordons and drains nodes violating health thresholds.
  - **success:** Any node experiencing an unrecoverable GPU driver fault (XID error) or exceeding a sustained 88°C thermal threshold for $> 60$ seconds is automatically cordoned, marked unschedulable, and has its active workloads evacuated within 30 seconds.

- **CAP-6** (Role-Based Access Governance)
  - **intent:** Platform enforces role-based permissions (Academic Administrator, Course Instructor, Student, Lab Operator, Privileged AI Researcher) linked to institutional identity providers for resource discovery and operation execution.
  - **success:** Unauthenticated or unauthorized API and UI requests across tier namespaces, reservation creation, and node management endpoints are denied with HTTP 403 Forbidden with 100% policy enforcement consistency across all endpoints.

## Constraints

- **Kubernetes Device Plugin Limitation:** The upstream Kubernetes integer GPU allocation model does not support native fine-grained dynamic VRAM slicing without vendor-specific vGPU / MPS daemons, constraining node assignment to discrete physical GPUs per container pod.
- **VRAM Threshold Boundaries:** Strict binary demarcation: Standard tier nodes enforce a hard maximum of 24 GB VRAM per node; High-Capacity tier nodes provide dual 48 GB VRAM (96 GB aggregate). Workloads exceeding node VRAM limits trigger immediate Out-Of-Memory (OOM) killer eviction.
- **On-Premise Network Isolation:** Cluster operates within university on-premise network topology; core control planes and compute runners maintain zero runtime dependencies on external public cloud services.
- **Thermal & Electrical Limits:** Total aggregate fleet electrical draw must not exceed the facility rack limit of 15 kW.

## Non-Goals

- **Public Cloud Billing Gateways:** No integration with commercial payment gateways, credit card processing, AWS/GCP/Azure billing APIs, or cloud burst-billing.
- **Custom GPU Driver / Hardware Manufacturing:** No kernel-level GPU driver modification, custom silicon design, or firmware reprogramming.
- **General-Purpose University IT Management:** The platform does not manage general university enterprise systems, student tuition billing, HR, or non-compute academic records.
- **Arbitrary Web Application Hosting:** The platform will not host non-AI web applications or general IT virtual machines.

## Success Signal

- **Success signal:** The platform sustains $\ge 80\%$ aggregate GPU fleet utilization over a rolling 30-day academic window while maintaining exactly 0.0% class session disruption during scheduled reservation windows and $< 45$ seconds interactive session readiness.

## Assumptions

- **[Safe]** University identity infrastructure provides a stable LDAP, SAML, or OAuth2 single sign-on service for user authentication.
- **[Safe]** Physical GPU nodes run certified Linux kernel distributions and NVIDIA drivers compatible with the NVIDIA GPU Operator for Kubernetes.
- **[Safe]** High-capacity 2x48 GB nodes feature high-bandwidth inter-GPU PCIe Gen4/5 or NVLink interconnects sufficient for distributed PyTorch/TF training.
- **[Safe]** Central shared network storage (NFS / Ceph / NVMe-oF) provides sufficient I/O bandwidth to prevent GPU starvation during multi-node data staging.
- **[Risky]** Course instructors will register semester lab reservations at least 48 hours in advance rather than making ad-hoc instant classroom lockout requests during active research peaks.
- **[Risky]** Research user training scripts implement standardized checkpointing hooks (e.g., PyTorch checkpointing / Kubeflow integration) to survive elastic preemption on shared high-capacity nodes.
