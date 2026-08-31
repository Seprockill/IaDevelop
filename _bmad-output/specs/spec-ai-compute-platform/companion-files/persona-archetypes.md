# Stakeholder Persona Archetypes

> Companion to SPEC-ai-compute-platform and ARCHITECTURE-SPINE

### 1. Academic Administrator (Strategic Steward)
* **Archetype Name:** Dean Danielle / Chair Carlos
* **Operational Scope:** Department-wide resource budgets, grant compliance, ROI oversight.
* **Key Interactions:** Views aggregate telemetry dashboards, configures semester token budgets, reviews audit trails.
* **Primary Interface:** Web Executive Dashboard (aggregated charts, CSV export, quota allocation tables).

### 2. Course Instructor (Curriculum Conductor)
* **Archetype Name:** Professor Priya
* **Operational Scope:** Scheduled lab sessions, exam windows, standardized course environments.
* **Key Interactions:** Creates recurring classroom reservations, publishes pre-warmed container images for courses, monitors in-class student pod status.
* **Primary Interface:** Web Reservation Portal & Course Environment Builder.

### 3. Student (Eager Learner)
* **Archetype Name:** Sam the Student
* **Operational Scope:** Hands-on lab exercises, homework assignments, coursework model training.
* **Key Interactions:** One-click launch of browser-based JupyterLab or VS Code Web instances within standard 24 GB quota limits; views queue wait times.
* **Primary Interface:** Student Launchpad Web UI.

### 4. Lab Operator / Infrastructure Engineer (Fleet Guardian)
* **Archetype Name:** Omar the Operator
* **Operational Scope:** Kubernetes cluster health, GPU thermals/power, node patching, driver upgrades, hardware failovers.
* **Key Interactions:** Configures preemption rules, monitors real-time Prometheus/Grafana metrics, triggers automated node cordons and drains.
* **Primary Interface:** Admin Operations Portal, Grafana Dashboards, `unictl` CLI / kubectl.

### 5. Privileged AI Researcher (Deep Explorer)
* **Archetype Name:** Rachel the Researcher
* **Operational Scope:** Multi-GPU / multi-node distributed deep learning, LLM fine-tuning, Kubeflow pipeline automation.
* **Key Interactions:** Submits `PyTorchJob` manifests to high-capacity 2x48 GB node queues, monitors loss curves via TensorBoard, relies on automated checkpointing on preemption.
* **Primary Interface:** Kubeflow Dashboard, `unictl` CLI, Python SDK / REST API.
