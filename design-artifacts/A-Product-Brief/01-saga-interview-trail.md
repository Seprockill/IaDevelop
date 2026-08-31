# Saga Discovery Interview Trail: University AI Compute Management Platform

> Strategic Alignment & Requirements Extraction Log  
**Agent:** Saga (📚 WDS Strategic Analyst)  
**Participant:** Seprockill (Lead Architect & Product Owner)  
**Date:** 2026-08-31  
**Phase:** Phase 1 — Product Brief  

---

## 1. Discovery Session Overview

This document records the strategic inquiry conducted by **Saga** to extract and crystallize the core objectives, architectural requirements, user personas, and success metrics for the **University AI Compute Management Platform**.

---

## 2. Interview & Discovery Dialogue

### 📚 Saga (Strategic Analyst):
*"Welcome! I am Saga, goddess of stories and wisdom, and your strategic partner in WDS. We are here to establish the foundational North Star for the **University AI Compute Management Platform**.*

*Let us begin with the core vision and problem landscape. In your own words, what friction does the current infrastructure face, and what is our primary mission?"*

### 👤 Seprockill (Product Owner):
*"Our university possesses a heterogeneous local GPU fleet: standard nodes equipped with 24 GB VRAM GPUs and high-capacity nodes equipped with dual 48 GB VRAM GPUs (2x48 GB VRAM). Currently, GPU access is managed ad-hoc via spreadsheets, direct SSH keys, and unmonitored processes.*

*This causes two critical failures: scheduled computer science and AI classes get disrupted because rogue research jobs consume all GPU memory, and researchers running multi-node Kubeflow training jobs suffer from starvation or unpredictable manual termination. Our mission is centralized, automated governance that replaces manual access with intelligent Kubernetes scheduling, automated quota enforcement, and guaranteed zero class disruptions."*

---

### 📚 Saga (Strategic Analyst):
*"Fascinating insight into the friction between interactive classroom teaching and heavy asynchronous research workloads.*

*Let us map out the stakeholder ecosystem. Who are the distinct personas interacting with this platform, what are their daily jobs, and where are their points of vulnerability?"*

### 👤 Seprockill (Product Owner):
*"We have 5 core stakeholder personas:*
1. **Academic Administrators:** Need high-level ROI visibility, departmental compute accounting, audit trails, and reporting on grant-funded resource utilization.
2. **Course Instructors:** Require time-bounded reservation of GPU pools for scheduled lectures/labs, one-click deployment of pre-warmed container environments, and a strict guarantee of zero class disruptions.
3. **Students:** Need frictionless browser-based access (JupyterLab/VS Code) without managing CUDA/Linux drivers, safe quotas, and automatic session saving.
4. **Lab Operators / Infrastructure Engineers:** Maintain node health, monitor thermals/power, manage Kubernetes scheduling, drivers, and preemption policies, and handle multi-tenant isolation.
5. **Privileged AI Researchers:** Require priority access to the 2x48 GB VRAM nodes, distributed multi-node/multi-GPU training pipelines via Kubeflow (PyTorchJob/TFJob), long batch executions, and checkpointing."*

---

### 📚 Saga (Strategic Analyst):
*"That provides crystal-clear boundaries between educational concurrency and high-throughput batch research. Now, how will we objectively measure success? What KPIs will prove the platform has succeeded?"*

### 👤 Seprockill (Product Owner):
*"Three primary metrics define our success:*
1. **GPU Fleet Utilization Rate:** Increase overall sustained utilization from ~35% to ≥ 80% through elastic queueing and off-peak backfilling.
2. **Zero Class Disruptions:** 100% reservation compliance during scheduled instructional hours with automated preemption locks.
3. **Fair Quota Distribution:** Enforce automated fair-share quotas, eliminating monopolization while reducing manual access ticket processing to instant self-service (< 1 minute)."*

---

## 3. Strategic Synthesis & Approval

* **Vision:** Approved & synthesized into `A-Product-Brief/project-brief.md`.
* **Hardware Matrix:** Formatted across Standard (24 GB) and High-Capacity (2x48 GB) tiers.
* **Next Phase:** Proceeding to **Phase 2: Trigger Mapping** to map psychological forces, anxieties, and motivations for each persona.
