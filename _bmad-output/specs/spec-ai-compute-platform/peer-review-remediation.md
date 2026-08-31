# Peer-Review Remediation Matrix (Deliverable E)

> Systematic Traceability & Remediation Mapping: Spec 1 Gaps to Spec 2 & Architecture Invariants  
**Project:** University AI Compute Management Platform  
**Date:** 2026-08-31  
**Status:** Complete  

---

## 1. Overview

This document provides a comprehensive remediation matrix addressing the feedback received during the Workshop 2 Peer-Review session. Each issue identified in **Spec 1** has been analyzed for root cause and systematically resolved in **Spec 2 (`SPEC.md`)** and the **Architecture Spine (`ARCHITECTURE-SPINE.md`)** across all 8 evaluation dimensions.

---

## 2. Workshop 2 Remediation Matrix

| Dimension | Peer-Review Issue Identified (Spec 1) | Root Cause in Spec 1 | Specific Action Taken in Spec 2 & Architecture Spine | Reference Artifacts & IDs |
| :--- | :--- | :--- | :--- | :--- |
| **1. Clarity** | Ambiguous descriptions of GPU resources; vague references to "GPU instances" without specifying VRAM sizes or node capabilities. | Lack of explicit hardware categorization between undergraduate coursework and research compute tiers. | Explicitly codified the fleet topology into Standard (single 24 GB VRAM) and High-Capacity (dual 48 GB VRAM / 96 GB aggregate). Defined exact tier intents. | **CAP-3**, **AD-3**, `glossary.md` |
| **2. Scope Control** | Scope creep regarding public cloud failover, external billing integrations, and custom kernel driver development. | Absence of an explicit Non-Goals section and boundary definitions in Spec 1. | Added explicit Non-Goals section in `SPEC.md` (excluding public cloud billing, custom silicon/driver modifications, general university IT). Bound control plane to on-premise k8s. | **SPEC.md (Non-Goals)**, **AD-1**, **AD-6** |
| **3. Verifiability** | Subjective and unmeasurable acceptance criteria (e.g., "fast response", "fair quota allocation", "stable classroom access"). | Success criteria lacked concrete, binary/threshold-based metrics suitable for automated test suites. | Refactored all capability success criteria into deterministic binary/threshold signals: $\ge 80\%$ fleet utilization, exactly 0.0% class disruption, $<500\text{ms}$ quota admission, $<45\text{s}$ session launch. | **CAP-1 to CAP-6 (success fields)**, **SPEC.md (Success Signal)** |
| **4. Atomicity** | Monolithic capabilities mixing authentication, quota reservation, pod scheduling, and telemetry into broad single requirements. | Conflation of multiple independent system lifecycles into a single "Compute Management" umbrella. | Decomposed platform into 6 isolated, single-responsibility capabilities (**CAP-1** to **CAP-6**), mapping each to decoupled ports and adapters in the Hexagonal Architecture. | **CAP-1 through CAP-6**, **ARCHITECTURE-SPINE.md (Design Paradigm)** |
| **5. Consistency** | Conflicting terminology across documents (e.g., "credits" vs "tokens" vs "slots"; contradictory preemption timing). | Absence of an authoritative technical glossary and standardized architectural conventions. | Authored authoritative `glossary.md`, standardized on "Token Metering" and "24 GB / 2x48 GB tiers", and locked preemption to a strict 15-minute drain window ($T-15\text{m}$). | `glossary.md`, **CAP-2**, **CAP-4**, **AD-4**, `state-transitions.mmd` |
| **6. Implementation Independence** | Spec 1 dictated internal database schemas and specific code-level function signatures inside capability requirements. | Violation of Spec Law Rule 2 (Capabilities must specify WHAT the system does, not HOW it is coded). | Extracted all implementation details from `SPEC.md` capability intents; relocated structural invariants, component boundaries, and pinned stack versions to `ARCHITECTURE-SPINE.md`. | **SPEC.md (Capabilities)**, **ARCHITECTURE-SPINE.md (AD-1 to AD-6, Stack)** |
| **7. Traceability** | Disconnect between high-level stakeholder personas (Instructors, Students, Researchers) and low-level scheduling rules. | Requirements lacked end-to-end traceability mapping user pain points to technical invariants. | Created `persona-archetypes.md`, mapped personas to capabilities in `project-brief.md`, and added the *Capability $\rightarrow$ Architecture Map* linking CAP-IDs to AD-IDs and code packages. | `project-brief.md`, `persona-archetypes.md`, **Capability $\rightarrow$ Architecture Map** |
| **8. Decomposition Readiness** | Inability to transition directly from specification to development stories due to missing architectural boundaries and schemas. | Missing structural seed, runtime lifecycle models, and component dependency graphs. | Delivered full structural source tree scaffold (`cmd/`, `internal/`, `deploy/`), renderable Mermaid state machines (`state-transitions.mmd`), and pinned stack dependencies. | **ARCHITECTURE-SPINE.md (Structural Seed, Stack)**, `system-context.mmd`, `state-transitions.mmd` |

---

## 3. Summary of Impact

* **Specification Quality:** Fully compliant with **Spec Law** (zero placeholder findings via deterministic linter).
* **Architectural Robustness:** Decoupled Hexagonal Control Plane with 6 formally documented Architecture Decision Records (ADRs).
* **Delivery Readiness:** 100% ready for automated story breakdown and development loop execution via `bmad-create-epics-and-stories`.
