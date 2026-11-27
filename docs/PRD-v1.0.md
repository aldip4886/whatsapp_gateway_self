# WhatsApp Gateway — Product Requirements Document

**Document:** PRD  
**Version:** 1.0.0  
**Status:** Baseline / Proposed  
**Date:** 2026-11-27  
**Repository:** [whatsapp_gateway_self](https://github.com/aldip4886/whatsapp_gateway_self)

## 1. Executive Summary

WhatsApp Gateway is a centralized platform for WhatsApp communications, campaign delivery, workflow automation, automated replies, AI-assisted knowledge support, and help desk operations. It is designed to reliably deliver outbound messages to hundreds or thousands of recipients while managing inbound conversations in a shared workspace.

### Product vision

> A single platform to manage WhatsApp communication that is centralized, automated, measurable, reliable, and extensible.

### Objectives

- Send bulk messages to hundreds/thousands of recipients using audience selection and campaigns.
- Manage contacts, tags, groups, segments, and consent.
- Create, approve, schedule, pause, resume, and cancel campaigns.
- Automate messaging based on events, schedules, conditions, and incoming messages.
- Provide rule-based auto-responses and an AI/RAG assistant.
- Support shared inbox, agent assignment, tickets, and service analytics.
- Track delivery, read, failure, response, and service metrics.
- Integrate with external systems through API and webhooks.

## 2. Scope & Product Modules

1. Dashboard & analytics
2. Contact management and segmentation
3. Campaign manager / bulk messaging
4. Message template management
5. Automation engine
6. Rule-based chatbot and AI assistant
7. Shared inbox and help desk
8. API, webhook and integration management
9. Administration, security, audit, and settings

## 3. Personas & Roles

| Role | Responsibilities / permissions |
|---|---|
| Super Admin | Full system control, account and configuration |
| Admin | Users, operational settings, campaigns, integrations |
| Campaign Manager | Contacts, segments, templates, campaigns |
| Automation Manager | Workflow and chatbot rules |
| Help Desk Supervisor | Inbox oversight, assignment, tickets, SLA |
| Help Desk Agent | Assigned conversations, replies, tickets |
| Analyst | Read-only analytics and exports |
| API User | Restricted machine-to-machine access |

Implement role-based access control (RBAC), least privilege, and audit trails.

## 4. Functional Requirements

### 4.1 Dashboard

Display aggregate and period-filtered metrics:
- Contacts, eligible contacts, opted-out/suppressed contacts
- Campaigns active/scheduled/completed
- Messages sent, delivered, read, failed
- Delivery rate, read rate, reply rate
- Inbound conversations, unassigned/open tickets
- First response time, resolution time, agent workload

### 4.2 Contact Management

Contact fields: ID, name, WhatsApp number (normalized to E.164), email, organization, job/title, tags, groups, custom fields, consent status and source, consent timestamp, opt-out timestamp, source, created/updated, last interaction.

Requirements:
- CSV/XLSX import with mapping, preview, validation, and error report.
- Normalize and validate phone numbers.
- Duplicate detection and merge guidance.
- Add/edit/delete (soft-delete for appropriate records).
- Tags, groups, custom fields and dynamic segments.
- Search, filters, pagination, export.
- Consent records and suppression list.
- Exclude opted-out or otherwise ineligible contacts from campaign recipients.

### 4.3 Campaign & Bulk Messaging

Campaign lifecycle: DRAFT → SCHEDULED → QUEUED → RUNNING → COMPLETED; additional PAUSED, CANCELLED, FAILED statuses.

Requirements:
- Create campaign with name, purpose, message type, sender/number, audience and schedule.
- Select contacts, groups, saved segments, or filter-based audiences.
- Compose template messages with approved variables, text, supported media and CTA elements.
- Preview personalized messages and validate missing variables.
- Validate consent, template status, audience size, provider limits, and sending configuration before launch.
- Send immediately or schedule for later.
- Pause, resume, cancel; show progress and status.
- Track recipient-level status, errors, provider message IDs.
- Rate control, pacing, retries, and duplicate prevention.
- Campaign approval/confirmation option for high-volume sends.
- Export campaign report.

### 4.4 Messaging & Templates

Support message types allowed by provider: text, template, media (image/document/video as supported), and supported interactive buttons/list/CTA.

Template attributes: name, category, language, header/body/footer, variables, media, buttons, provider template ID, approval status, timestamps.

Template requirements:
- Local catalog synchronized/managed with provider templates.
- Variable mapping and preview.
- Validate template approval and category/policy before sending.
- Record version and change history.

### 4.5 Automation Engine

Triggers:
- Incoming message / keyword
- Contact created or field changed
- Tag added/removed
- Schedule or date event
- Campaign lifecycle event
- Ticket/conversation lifecycle event
- Webhook/API event
- Inactivity timeout (subject to policy and messaging-window rules)

Conditions:
- Message contains/intent
- Contact tag, group, or custom field
- Time/date
- Campaign membership/status
- Ticket/conversation state
- Agent availability

Actions:
- Send approved WhatsApp message/template
- Add/remove tag or update contact
- Create/assign ticket
- Notify staff
- Wait/delay
- Call approved external API
- Trigger another workflow (with loop protection)

Requirements: visual workflow builder, draft/publish/enable/disable, versioning, test/simulation, execution history, logs, retry/failure policy, loop/depth protection, per-workflow limits.

### 4.6 Chatbot & Auto-Responder

Rule-based v1:
- Keyword/phrase rules, welcome message, fallback response, FAQ, menu options, intent routing, configurable priority.
- Handle repeated messages and prevent reply loops.
- Handoff to agent when requested or rule cannot resolve.

AI/RAG phase:
- Knowledge ingestion from PDF, DOCX, XLSX, approved websites, FAQ, SOP, policy and other approved sources.
- Document processing, chunking, embeddings, vector retrieval and grounded answers with source links.
- Intent classification, summarization, draft replies, ticket categorization, optional sentiment signals.
- Confidence/grounding threshold and safe fallback.
- Admin-configurable model, prompts, knowledge scope and usage controls.
- Human handoff for low confidence, sensitive topics, complaints, explicit human request, or unresolved questions.
- AI must not disclose restricted data or invent policy/process answers.

### 4.7 Shared Inbox & Help Desk

Shared inbox views: All, Unassigned, Assigned to me, Waiting, Resolved; search and filters by contact, agent, tags, category and status.

Conversation features:
- Realtime incoming message display and status
- Reply with text and supported media/template
- Contact profile and conversation history
- Assignment and reassignment
- Internal notes (not sent to contact)
- Tags and categories
- Close/reopen conversation
- Search and attachment preview
- Agent availability and workload

Ticket fields: ID, contact, subject, category, priority, assigned agent/team, status, created/updated, first-response and resolution timestamps, linked conversation, history/comments.

Ticket statuses: OPEN, IN_PROGRESS, WAITING, RESOLVED, CLOSED. Priorities: LOW, MEDIUM, HIGH, URGENT.

Support assignment modes: manual, round-robin, category/team routing, availability-aware routing (phased).

### 4.8 Analytics & Reporting

Campaign: recipient count, sent, delivered, read, failed, replies, delivery/read/reply rates, per-recipient outcome and time-series.

Help desk: inbound volume, open/resolved/overdue, first response, resolution time, category volume, agent workload and service quality.

Automation: workflow runs, successes, failures, triggers/actions, handoff/fallback rates.

Exports: CSV/XLSX initially; PDF report later.

### 4.9 API & Integrations

Versioned REST API at `/api/v1`, JSON payloads, API key/token auth for machine clients, rate limits and scopes.

Integration features:
- WhatsApp provider adapter
- Outbound webhooks for selected events
- Inbound webhook endpoint
- External-system API actions in workflows
- Webhook signing, retry, event IDs, replay protection, logs

## 5. User Journeys

### Bulk campaign

Admin → Import/maintain contacts → Create campaign → Select segment → Compose template → Preview/validate consent and variables → Approve/confirm → Schedule/send → Monitor queue and results → Export.

### Incoming support

WhatsApp user sends message → webhook verifies and persists event → async processing → rule/AI response or ticket/routing → agent receives inbox conversation → replies → resolves ticket → analytics updated.

### Automated reminder

Event/schedule trigger → audience/condition evaluation → eligible contact and consent check → queue message → send via provider → webhook status update → workflow log.

## 6. Technical Architecture

### Architecture decision

Use a **modular monolith with independently scalable background workers** for MVP. Avoid microservices until scale, team boundaries, or operational requirements justify splitting services.

Logical layers:
1. Client: responsive admin dashboard and agent inbox.
2. Application: auth, contacts, campaigns, messaging, inbox, tickets, automation, AI.
3. Messaging/event: API gateway, webhook receiver, event handling, queue, scheduler, workers.
4. Data: PostgreSQL, Redis, object storage.
5. Integrations: Meta WhatsApp Cloud API, LLM provider, external APIs.

### Outbound message flow

Campaign → validation/audience resolver → consent/template checks → recipient jobs → Redis/BullMQ → send worker → WhatsApp Cloud API → status webhook → event processor → database/analytics.

### Inbound message flow

WhatsApp → Meta webhook → signature verification/deduplication → persist quickly → enqueue event → conversation processor → rule/AI/ticket/handoff → outbound queue or agent inbox.

### Provider abstraction

Create a provider interface for sending text, templates and media, and handling normalized message statuses. Business logic must not be coupled to raw provider payloads. Provider adapter may support alternate BSPs later.

## 7. Recommended Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | Next.js, React | Dashboard and inbox |
| Language | TypeScript | Full-stack type safety |
| Styling | Tailwind CSS | Styling |
| UI components | shadcn/ui | Accessible UI components |
| Forms | React Hook Form | Forms |
| Validation | Zod | Runtime validation |
| Backend | Next.js API initially; optional Fastify/NestJS later | API/business logic |
| ORM | Prisma | Data access/migrations |
| Database | PostgreSQL | Persistent data |
| Queue/cache | Redis + BullMQ | Queue, retries, delayed jobs, locks |
| Realtime | Socket.IO or native managed realtime | Inbox updates |
| Auth | Auth.js or secure managed auth | Sessions/login |
| Storage | S3-compatible / Cloudflare R2 | Media and files |
| WhatsApp | Meta WhatsApp Business Platform Cloud API | Official messaging |
| AI | OpenAI API / Gemini | AI response and assist |
| Vector | pgvector | RAG retrieval |
| Containers | Docker Compose | MVP deployment |
| Reverse proxy | Caddy | TLS/routing |
| CI/CD | GitHub Actions | Build/test/deploy |
| Error tracking | Sentry | Application errors |
| Logs | Pino structured logs | Diagnostics |
| Metrics | Prometheus/Grafana or managed metrics | Observability |
| Testing | Vitest, Playwright, k6 | Unit, E2E, load testing |

### Backend recommendation

MVP: Next.js application + separate Node.js worker process(es) in the same codebase/workspace, Redis + BullMQ. NestJS/Fastify can be introduced if backend complexity/team size warrants it.

## 8. Infrastructure

### Initial VPS baseline

- 4 vCPU
- 8 GB RAM
- 100 GB SSD
- Ubuntu LTS
- Docker Engine + Docker Compose
- Public DNS domain and HTTPS

Baseline containers/services:
- web/API
- messaging worker
- automation worker
- AI worker (can remain disabled until phase 3)
- PostgreSQL
- Redis
- Caddy reverse proxy
- optional monitoring/logging stack

This is a planning baseline only; capacity must be validated with workload, concurrency, message throughput, database growth and provider limits.

### Scaling path

MVP: single VPS, Docker Compose, persistent volumes, off-server backup.

Growth: separate application/worker host, managed PostgreSQL, managed Redis, object storage, independently scaled workers.

Enterprise: high availability database, multiple app replicas, queue resilience, dedicated observability, orchestration only when justified.

## 9. Queue & Reliability

BullMQ queues:
- `campaign-dispatch`
- `whatsapp-send`
- `webhook-processing`
- `automation-execution`
- `ai-processing`
- `media-processing`
- `notifications`

Requirements:
- Persist jobs and outcomes.
- Concurrency controlled by provider limits and account health.
- Exponential backoff for transient errors.
- Permanent errors are not retried.
- Dead-letter/failure review.
- Idempotency key per logical send.
- Deduplicate webhook and provider events.
- Campaign pause/cancel controls.
- Job timeout and observability.
- Recovery after worker restart.
- Avoid duplicate sends during retry or reconciliation.

Example retry backoff (configurable): initial attempt; retry after 5 seconds, 30 seconds, 2 minutes, then 10 minutes. Provider Retry-After and error semantics take precedence.

## 10. Database Domain Model

Core tables/entities:

- Identity: users, roles, permissions, user_roles, teams
- Contacts: contacts, tags, contact_tags, groups, group_members, custom_fields, consents
- Campaigns: campaigns, campaign_runs, campaign_audiences, campaign_recipients
- Messaging: messages, message_templates, message_status_events, message_media
- Inbox: conversations, conversation_messages, conversation_assignments
- Help desk: tickets, ticket_comments, ticket_categories, ticket_events
- Automation: workflows, workflow_triggers, workflow_nodes, workflow_edges, workflow_runs, workflow_run_logs
- AI: knowledge_sources, knowledge_documents, knowledge_chunks, ai_sessions, ai_messages
- Platform: audit_logs, webhook_events, api_keys, system_settings

Database requirements: UUIDs, timestamps, indexes on phone, provider message ID, status, campaign and conversation lookup fields; soft-delete where appropriate; migrations; transaction boundaries; connection pooling; backups.

## 11. REST API Outline

Base path: `/api/v1`.

Auth:
- POST /auth/login
- POST /auth/logout
- GET /auth/me

Contacts:
- GET/POST /contacts
- GET/PATCH/DELETE /contacts/:id
- POST /contacts/import
- POST /contacts/export

Campaigns:
- GET/POST /campaigns
- GET/PATCH /campaigns/:id
- POST /campaigns/:id/send
- POST /campaigns/:id/schedule
- POST /campaigns/:id/pause
- POST /campaigns/:id/resume
- POST /campaigns/:id/cancel

Messages:
- POST /messages/send
- POST /messages/send-bulk
- GET /messages
- GET /messages/:id

Automation:
- GET/POST /workflows
- PATCH /workflows/:id
- POST /workflows/:id/activate
- POST /workflows/:id/deactivate
- POST /workflows/:id/test
- GET /workflows/:id/runs

Inbox:
- GET /conversations
- GET /conversations/:id
- POST /conversations/:id/reply
- POST /conversations/:id/assign
- POST /conversations/:id/resolve

Tickets:
- GET/POST /tickets
- PATCH /tickets/:id
- POST /tickets/:id/assign
- POST /tickets/:id/resolve

Webhook:
- GET /webhooks/whatsapp (provider verification as required)
- POST /webhooks/whatsapp

## 12. Security, Privacy & Compliance

- TLS for all external traffic.
- Secure secrets management; no secrets committed.
- RBAC and least privilege.
- 2FA for privileged accounts (recommended).
- Webhook signature verification.
- API authentication, scopes, rate limits.
- Audit trail for admin, campaign, contact and configuration changes.
- Encrypt sensitive data at rest where supported.
- Secure cookies/session expiration.
- Validate uploaded files, type/size, and malware scanning strategy.
- Contact consent and opt-out history.
- Suppression list enforced at dispatch time.
- Data retention, access, export and deletion policy.
- Tenant boundaries if multi-tenant support is introduced.
- Follow WhatsApp Business policies, template approval, messaging windows, rate limits and quality restrictions.
- Design for Indonesia's personal data protection requirements (including UU PDP); obtain appropriate legal/compliance review.

Never design this system as an unsolicited spam tool. Campaigns should be consent-based, relevant, and respect opt-out.

## 13. Non-Functional Requirements

| Area | Initial target |
|---|---|
| Dashboard initial load | ≤3 seconds under normal load |
| Internal API latency | P95 ≤500ms for lightweight non-aggregated endpoints |
| Webhook response | Acknowledge quickly after validation/persistence/enqueue; no AI work inline |
| Import | Batch import of 10,000 records with progress/error report |
| Queue | 10,000+ campaign jobs with controlled throughput |
| Realtime inbox | Near-real-time updates |
| Availability | MVP goal 99%; later 99.9% with suitable architecture |
| Horizontal scaling | Workers can scale independently |
| Observability | Structured logs, error tracking, health checks, metrics |
| Recovery | Persistent queues, database backup, tested restoration |

Targets exclude external provider latency and are subject to load testing.

## 14. UX / UI

Responsive SaaS admin, light mode by default, compact sidebar navigation:
- Dashboard
- Contacts
- Campaigns
- Inbox / Help Desk
- Automation
- Chatbot & AI
- Templates
- Analytics
- Settings

Design direction: clean/minimal; navy and blue primary, WhatsApp green accent, gold highlight; data tables, filters, status chips, drawer contact details, responsive layouts, skeleton/empty states, toast notifications, charts and clear destructive-action confirmation.

## 15. MVP & Phasing

### MVP — Reliable core messaging
- Authentication/RBAC
- Dashboard foundation
- Contact CRUD, CSV/XLSX import, tags, segments
- WhatsApp provider integration
- Webhook receiver and status processing
- Template catalog/validation
- Campaign creation, preview, send/schedule
- Queue, pacing, retries, pause/resume/cancel
- Consent/suppression
- Basic campaign analytics
- Logs, audit, backups, Docker deployment

### Phase 2 — Automation & support
- Shared inbox
- Conversation history
- Assignment and teams
- Internal notes
- Ticketing and categories
- Basic keyword/FAQ auto-responder
- Workflow triggers/actions
- Realtime agent inbox
- Service analytics and SLA basics

### Phase 3 — AI & advanced integrations
- AI chatbot
- RAG knowledge base
- AI intent and ticket classification
- AI summaries and agent copilot
- Sentiment signals (optional)
- External API actions
- Advanced analytics and reports

## 16. Delivery Roadmap (Indicative 12 weeks)

| Sprint | Focus | Deliverable |
|---|---|---|
| 1 | Repo, CI, Docker, architecture, secrets | Foundation |
| 2 | Auth, RBAC, DB, navigation | Admin shell |
| 3 | Contacts, import, tags | Contacts |
| 4 | Provider adapter, webhook, send | Messaging |
| 5 | Campaigns, templates, audience | Campaign manager |
| 6 | Queue, scheduler, retries, status | Reliable sending |
| 7 | Inbox and conversation history | Shared inbox |
| 8 | Auto-reply and workflow foundations | Automation |
| 9 | Tickets, assignment, SLA | Help desk |
| 10 | Analytics and exports | Reporting |
| 11 | Security, load and resilience testing | Release candidate |
| 12 | UAT, docs, deployment | MVP production release |

Timeline is indicative and assumes scope discipline, available provider credentials, and a small focused team. AI/RAG may move beyond the initial 12-week release.

## 17. Testing Strategy

- Unit: phone normalization, template variables, audience resolver, consent rules, permissions, automation conditions (Vitest).
- Integration: PostgreSQL, Redis, provider mocks, webhook dedup, retry and campaign state.
- E2E: login, import, create campaign, schedule/send, status reporting, incoming message, agent reply, resolve (Playwright).
- Load: queue throughput, webhook burst, 1,000-recipient campaign, worker recovery, DB/Redis pressure (k6).
- Security: auth/authorization, input validation, webhook signature, upload validation, rate limits, secret scanning.

## 18. Budget Framework

Recurring cost categories:
- VPS/compute
- PostgreSQL (self-hosted or managed)
- Redis (self-hosted or managed)
- Domain/TLS
- Object storage and egress
- Backups
- Monitoring/logging
- WhatsApp usage charges
- AI token usage
- Optional email/SMS provider

Formula:
`Total = Infrastructure + WhatsApp + AI + Storage/Egress + Backup + Monitoring`

WhatsApp costs vary by applicable Meta pricing, message category, destination and current commercial model. AI cost varies by model and token volume. Confirm pricing before launch.

## 19. Risks & Mitigation

| Risk | Mitigation |
|---|---|
| Account restriction/quality degradation | Official API, opt-in, policy adherence, quality monitoring |
| Provider rate limits | Queue, throttling, backoff, account-specific controls |
| Duplicate messages | Idempotency and reconciliation |
| Webhook retries/duplicates | Signature, event IDs, dedup |
| DB/Redis outage | Monitoring, backups, recovery/runbooks |
| AI hallucination | Retrieval grounding, sources, thresholds, handoff |
| Data leakage | RBAC, minimal access, audit, retention |
| Wrong audience/campaign | Preview, eligibility validation, approval controls |
| Worker crash | Durable queue, restart and recovery |
| Cost spikes | Usage limits, budgets, rate limits, AI quotas |

## 20. MVP Acceptance Criteria

- Admin can authenticate and manage permitted users.
- Contacts can be imported and validated from CSV/XLSX.
- Contacts can be segmented and tagged.
- Campaign can target a defined audience.
- Templates and dynamic variables can be previewed and validated.
- Campaigns can be scheduled and controlled.
- Bulk delivery uses persistent background jobs, not a synchronous request loop.
- Provider statuses are reflected when webhook events are received.
- Incoming messages are accepted, validated, deduplicated and persisted.
- Basic auto-response can be configured.
- Shared inbox basic operation is available (if included in MVP release scope; otherwise Phase 2).
- Failed sends are classified and retried when appropriate.
- Opt-out contacts are suppressed.
- Campaign outcomes can be exported.
- Audit logging and backup are configured.
- Services can run with Docker Compose.
- Runbooks and environment configuration are documented.

## 21. Repository Structure (Target)

```
whatsapp-gateway/
├── apps/
│   ├── web/                 # Dashboard
│   ├── api/                 # API (may be same app initially)
│   └── worker/              # Background workers
├── packages/
│   ├── database/
│   ├── whatsapp/
│   ├── messaging/
│   ├── contacts/
│   ├── campaigns/
│   ├── automation/
│   ├── chatbot/
│   ├── helpdesk/
│   ├── auth/
│   └── shared/
├── infrastructure/
│   ├── docker/
│   ├── reverse-proxy/
│   ├── monitoring/
│   └── scripts/
├── docs/
│   ├── PRD-v1.0.md
│   ├── VERSIONING.md
│   └── images/
└── docker-compose.yml
```

MVP may begin as a simpler workspace; folders should reflect module boundaries without forcing premature package extraction.

## 22. Product Principles

1. Reliability before throughput.
2. Official WhatsApp integration.
3. Consent and user preferences are first-class data.
4. Async processing for bulk and webhook workloads.
5. Idempotency for message sending and event handling.
6. Human support remains available.
7. AI is grounded, bounded and auditable.
8. Modular design; avoid premature microservices.
9. Observability and operations are part of product, not an afterthought.
10. Campaigns must be measurable and controllable.

## 23. Decisions & Open Questions

- Public or private repository: repository currently public; confirm before production code or secrets are added.
- WhatsApp business ownership, number, WABA, app and production access.
- Single business/organization vs multi-tenant architecture.
- Campaign approval workflow and sender permissions.
- Expected monthly message volume and peak rate.
- Help desk team size, hours, SLA and assignment rules.
- AI provider, knowledge sources, budget, data residency and retention.
- Hosting provider, domain, backup retention and disaster recovery.
- Data retention and legal/compliance review.
- Required integrations and API consumers.

## 24. Version History

| Version | Date | Notes |
|---|---|---|
| 1.0.0 | 2026-11-27 | Initial product baseline covering campaign messaging, automation, chatbot/AI, help desk, architecture, stack, infrastructure, API outline, security, roadmap and acceptance criteria. |

---

## References (official documentation to validate during implementation)

- Meta WhatsApp Business Platform / Cloud API: https://developers.facebook.com/docs/whatsapp/cloud-api/
- Meta Webhooks: https://developers.facebook.com/docs/whatsapp/cloud-api/webhooks/
- WhatsApp Business Platform policy: https://business.whatsapp.com/policy
- BullMQ: https://docs.bullmq.io/
- Prisma: https://www.prisma.io/docs
- PostgreSQL: https://www.postgresql.org/docs/
- Next.js: https://nextjs.org/docs
