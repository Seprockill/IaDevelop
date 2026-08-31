# Agent Experience: Saga — Phase 1 Product Brief
Date: 2026-08-31
Key Takeaways:
- Heterogeneous GPU fleet requires clear scheduling tier separation (24 GB vs 2x48 GB).
- Zero class disruption is a hard requirement achieved through calendar-linked reservation locks.
- Research workloads must utilize elastic backfill with checkpointing hooks on Kubeflow.
