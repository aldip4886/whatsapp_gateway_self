# WhatsApp Gateway — Claude Instructions

Read and follow `AGENTS.md` as the primary repository guidance. The product baseline and confirmed design direction are in `docs/PRD-v1.0.md`.

When implementing changes, preserve these project invariants:

- Use the official Meta WhatsApp Business Platform API behind a provider adapter.
- Keep bulk messaging, webhooks, workflows, and AI processing asynchronous, durable, observable, and idempotent.
- Enforce consent and opt-out suppression at dispatch time.
- Apply RBAC, least privilege, audit trails, secure secret handling, and webhook verification.
- Keep AI grounded in approved knowledge with confidence thresholds and human handoff.
- Prefer modular-monolith boundaries and avoid premature microservices.
- Keep the MVP focused on reliable core messaging; inbox, ticketing, visual automation, and AI/RAG are later phases.
- The initial deployment has one organization, one WABA, and one active sender. Campaigns above 1,000 recipients require configurable approval by default.
- Treat reliability, consent, auditability, observability, and recovery as release gates.

Engineering configuration and issue-tracker conventions are documented under `docs/agents/`.
