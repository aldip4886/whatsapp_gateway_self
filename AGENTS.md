# WhatsApp Gateway — Agent Guide

WhatsApp Gateway is a consent-based communication automation and customer-support platform built around the official Meta WhatsApp Business Platform Cloud API.

## Product direction

- Prioritize reliable, measurable, controllable messaging over raw throughput.
- Build the MVP as a modular monolith with independently scalable background workers; do not introduce microservices without a demonstrated boundary or operational need.
- MVP scope is reliable core messaging: authentication/RBAC, contacts, consent, templates, provider integration, webhook processing, campaigns, queues/retries, and basic analytics. Treat inbox, ticketing, visual automation, and AI/RAG as later phases.
- Start with one organization, one WABA, and one active sender. Keep boundaries extensible for future multi-tenancy and multi-sender support.
- Keep product boundaries visible: contacts and consent, campaigns, templates and messaging, automation, inbox/help desk, analytics, integrations, and administration.
- Treat the PRD at `docs/PRD-v1.0.md` as the product baseline. Read it before making architectural or domain-level changes.

## Engineering rules

- Use the official WhatsApp API and isolate provider payloads behind an adapter. Domain and application logic use normalized provider-neutral models.
- Bulk sends, webhook processing, automation, media work, and AI work are asynchronous. Use durable jobs, bounded concurrency, retries with backoff, dead-letter handling, and restart recovery.
- Make sends and webhook/event handling idempotent. Prevent duplicate sends during retries and deduplicate provider events.
- Use explicit campaign state transitions and recipient snapshots. Campaigns above 1,000 recipients require configurable approval by default.
- Enforce consent and suppression checks at dispatch time, not only when an audience is built. Preserve opt-out history.
- Validate templates, variables, audience eligibility, provider limits, and sender configuration before launch. Campaigns must support preview, pacing, pause, resume, cancel, and recipient-level outcomes.
- Apply RBAC, least privilege, audit logging, secure secret handling, input/file validation, webhook signature checks, rate limits, and tenant boundaries when multi-tenancy is introduced.
- Keep AI answers grounded in approved knowledge, respect confidence thresholds, protect restricted data, and hand off to a human for uncertainty, sensitive topics, complaints, or explicit requests.
- Treat reliability, consent, auditability, observability, and recovery as release gates; defer features that do not meet them.
- Prefer small module-focused changes with tests and migration safety. Preserve observability through structured logs, health checks, metrics, error tracking, and useful failure states.

## Working agreements

- Before changing a domain term or workflow, read the relevant `GLOSSARY.md` and ADRs when they exist. Record durable architectural decisions in `docs/adr/`.
- Use TypeScript types and runtime validation at boundaries. Keep database transactions and retry semantics explicit.
- Never commit secrets, provider credentials, personal data fixtures, or generated production data.
- For risky campaign, contact, permission, or migration changes, add focused tests for authorization, consent, idempotency, failure classification, and recovery.
- Update relevant documentation when behavior, API contracts, operational runbooks, or environment configuration changes.

## Agent skills

### Issue tracker

Issues for this repository live in GitHub Issues and are managed with `gh`. See `docs/agents/issue-tracker.md`.

### Triage labels

Use the repository's five canonical GitHub triage labels. See `docs/agents/triage-labels.md`.

### Domain docs

This is a single-context repository. Read the root glossary and relevant ADRs when present. See `docs/agents/domain.md`.

## Source of truth

- Product requirements: `docs/PRD-v1.0.md`
- Project overview: `README.md`
- Versioning: `docs/VERSIONING.md`
- Visual references: `docs/REFERENCE-VISUALS.md` and `docs/images/README.md`
