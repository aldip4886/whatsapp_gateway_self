# WhatsApp Gateway

A WhatsApp Communication Automation & Customer Support Platform built around the official Meta WhatsApp Business Platform Cloud API.

## Product scope

- **Campaign Manager:** bulk messaging, audiences, templates, scheduling, campaign analytics.
- **Automation Engine:** triggers, conditions, actions, event-based workflows, reminders.
- **Chatbot & AI:** rule-based auto-responder, AI/RAG knowledge assistant, human handoff.
- **Help Desk:** shared inbox, conversation assignment, ticketing, SLA, agent analytics.
- **Contact Management:** import, segmentation, tags, consent/opt-out.
- **Analytics:** sent, delivered, read, failed, reply rate, response/resolution time.

## Architecture decision

Start with a **modular monolith + background workers**, not microservices. Use Redis + BullMQ for queues, retries, scheduling, and async event processing.

## Technology stack

- Next.js + React + TypeScript
- Tailwind CSS + shadcn/ui
- PostgreSQL + Prisma
- Redis + BullMQ
- Socket.IO (realtime inbox)
- Meta WhatsApp Business Platform Cloud API
- OpenAI API / Gemini (AI features, phased)
- pgvector (RAG, phased)
- Docker Compose, Caddy, GitHub Actions
- Sentry; Prometheus/Grafana as scale needs

## Documentation

- [PRD v1.0](docs/PRD-v1.0.md)
- [Version history](docs/VERSIONING.md)
- [Visual reference overview](docs/REFERENCE-VISUALS.md)
- [Architecture reference diagrams](docs/images/README.md)

## Planned delivery

1. Foundation, authentication, database, provider adapter, webhook and queue
2. Contacts and campaign manager
3. Scheduling, retries, message status and analytics
4. Shared inbox, help desk and agent assignment
5. Rule-based automation
6. AI assistant and RAG knowledge base

## Important operating principles

- Use the official WhatsApp Business Platform API.
- Honor consent and opt-out/suppression lists.
- Never send messages synchronously in a web request for bulk campaigns.
- Make processing idempotent and webhook handling resilient.
- Keep AI answers grounded in approved knowledge and hand off when uncertain.

> This repository currently contains product documentation and visual architecture references. Implementation is planned.
