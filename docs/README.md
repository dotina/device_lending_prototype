# IInovi Mobile Device Lending Platform -- Documentation

> A multi-tenant mobile device lending (BNPL) platform enabling partners to offer device financing through multiple channels, integrated credit scoring, Samsung Knox Guard device management, and mobile money payments.

---

## Target Audience

| Audience | What to Read |
|---|---|
| **Developers** | Architecture overview, API references, service guides, local development setup |
| **Architects** | Architecture decisions, multi-tenancy model, security architecture, integration patterns |
| **Product / Business** | Platform overview, lending workflows, channel descriptions, partner onboarding |
| **DevOps / SRE** | Deployment guides, infrastructure, observability, runbooks |

---

## Platform at a Glance

IInovi is a **multi-tenant, multi-channel mobile device lending platform** that allows financing partners to originate, manage, and collect device loans. The platform supports:

- **Buy Now, Pay Later (BNPL)** device financing with configurable loan products
- **Multi-channel origination** via web POS, mobile app, and USSD
- **Strategy-based credit scoring** with pluggable decision engines
- **Samsung Knox Guard integration** for remote device lock/unlock lifecycle management
- **Mobile money payments** (M-Pesa and other providers) with real-time reconciliation
- **Full loan portfolio management** including disbursement, collections, arrears, and write-offs
- **Multi-tenant isolation** with shared infrastructure and per-tenant configuration

---

## Documentation Map

### Architecture

| Document | Description |
|---|---|
| [System Architecture Overview](architecture/overview.md) | High-level system architecture, service catalog, technology stack, and data flow |
| [Multi-Tenancy Model](architecture/multi-tenancy.md) | Tenant isolation strategy, Row-Level Security, provisioning, and cross-tenant operations |
| [Security Architecture](architecture/security.md) | Authentication, authorization, encryption, audit trails, and compliance |

### Domain Guides

| Document | Description |
|---|---|
| Lending Lifecycle *(planned)* | End-to-end loan origination, approval, disbursement, repayment, and closure |
| Device Management *(planned)* | Knox Guard integration, lock/unlock policies, device provisioning |
| Credit Scoring *(planned)* | Strategy-based scoring engine, rule configuration, CRB integration |
| Payment Processing *(planned)* | Mobile money integration, payment reconciliation, settlement |
| Partner Management *(planned)* | Partner onboarding, configuration, ERP integration |

### Compliance

| Document | Description |
|---|---|
| [KYC/AML Compliance](compliance/kyc-aml.md) | KYC verification requirements, tiered KYC levels, AML/CFT transaction monitoring, sanctions screening, and per-market regulatory obligations |
| [Consumer Protection Compliance](compliance/consumer-protection.md) | Transparent disclosure, total cost of credit, cooling-off periods, fair collections, complaints handling, and responsible lending |
| [Credit Bureau Integration](compliance/credit-bureau-integration.md) | Pre-lending credit checks, post-lending CRB reporting, CRB adapter architecture, data formats, and dispute resolution |
| [Data Privacy and Consent Management](compliance/data-privacy-consent.md) | Consent management, data subject rights, POPIA/DPA/NDPA/GDPR compliance, cross-border transfers, and privacy impact assessments |
| [Licensing Requirements](compliance/licensing.md) | Per-market lending license requirements, platform vs. financer licensing, digital lending regulations, and regulatory sandbox options |

### API Reference

| Document | Description |
|---|---|
| API Gateway *(planned)* | Authentication, rate limiting, versioning, common headers |
| Core Service APIs *(planned)* | Per-service endpoint documentation with request/response schemas |
| Webhook Events *(planned)* | Event catalog, payload schemas, retry policies |

### Operations

| Document | Description |
|---|---|
| Local Development Setup *(planned)* | Prerequisites, environment configuration, running services locally |
| Deployment Guide *(planned)* | Docker Compose, Kubernetes manifests, CI/CD pipelines |
| Observability *(planned)* | Logging, metrics, tracing, alerting, dashboards |
| Runbooks *(planned)* | Incident response procedures, common troubleshooting steps |

---

## Quick Start

### Prerequisites

- Python 3.11+
- Node.js 20+ and pnpm
- PostgreSQL 15+
- RabbitMQ 3.12+
- Redis 7+
- Docker and Docker Compose

### Running Locally

```bash
# Clone the repository
git clone <repository-url>
cd prototype

# Start infrastructure services
docker compose up -d postgres rabbitmq redis

# Set up the backend
cd backend
python -m venv .venv
source .venv/bin/activate   # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
alembic upgrade head
uvicorn app.main:app --reload --port 8000

# Set up the web POS frontend
cd ../frontend
pnpm install
pnpm dev
```

### Environment Configuration

Each service reads configuration from environment variables. Copy the provided `.env.example` to `.env` and adjust values for your local setup. Key variables include:

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `RABBITMQ_URL` | RabbitMQ AMQP connection string |
| `REDIS_URL` | Redis connection string |
| `JWT_SECRET_KEY` | Secret for signing JWT tokens |
| `KNOX_GUARD_API_KEY` | Samsung Knox Guard API credentials |
| `MPESA_CONSUMER_KEY` | M-Pesa API consumer key |

---

## Repository Structure

```
prototype/
  backend/                 # Python FastAPI backend services
    app/
      api/                 # API route handlers
      core/                # Configuration, security, dependencies
      models/              # SQLAlchemy ORM models
      schemas/             # Pydantic request/response schemas
      services/            # Business logic layer
      integrations/        # External system adapters
    migrations/            # Alembic database migrations
    tests/                 # Backend test suite
  frontend/                # React TypeScript web POS application
    src/
      components/          # Reusable UI components
      pages/               # Page-level components
      services/            # API client and state management
  mobile/                  # Kotlin Multiplatform mobile application
    shared/                # Shared KMP business logic
    androidApp/            # Android-specific code
    iosApp/                # iOS-specific code
  ussd/                    # USSD channel gateway
  docs/                    # Project documentation (you are here)
  infrastructure/          # Docker, Kubernetes, Terraform configs
```

---

## Contributing

Refer to `CONTRIBUTING.md` in the repository root for coding standards, branching strategy, pull request guidelines, and code review expectations.

---

## License

Proprietary. All rights reserved.
