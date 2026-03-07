# Data Retention Policy

## Overview

This document defines the data retention policy for the Mobile Device Lending Solution. It establishes storage tiers, retention durations, migration procedures, and deletion policies for all data categories. The policy is designed to balance operational performance, regulatory compliance, cost efficiency, and data subject rights.

---

## Storage Tiers

### Tier Architecture

```mermaid
flowchart LR
    subgraph Hot Tier
        direction TB
        H_DB[(PostgreSQL)]
        H_DESC[0 - 30 days]
        H_USE[Real-time operations]
    end

    subgraph Warm Tier
        direction TB
        W_DB[(ClickHouse / Parquet)]
        W_DESC[31 - 365 days]
        W_USE[Analytics and reporting]
    end

    subgraph Cold Tier
        direction TB
        C_DB[(S3 Glacier / Archive)]
        C_DESC[1 - 7 years]
        C_USE[Regulatory compliance]
    end

    subgraph Purge
        direction TB
        P_DESC[After retention period]
        P_USE[Secure deletion]
    end

    Hot Tier -->|Automated migration| Warm Tier
    Warm Tier -->|Automated migration| Cold Tier
    Cold Tier -->|Policy-based| Purge

    style H_DB fill:#d32f2f,color:#fff
    style W_DB fill:#f57c00,color:#fff
    style C_DB fill:#1565c0,color:#fff
```

---

## Hot Tier (0--30 Days)

### Purpose

The hot tier stores the most recent data, providing real-time query performance for operational workflows, dashboards, and active loan servicing.

### Technical Specifications

| Attribute | Specification |
|---|---|
| Storage Engine | PostgreSQL (primary relational database) |
| Query Performance | Sub-second response for indexed queries |
| Availability | 99.9% uptime SLA |
| Replication | Synchronous replication to standby |
| Encryption | AES-256 at rest; TLS 1.3 in transit |
| Backup Frequency | Continuous WAL archiving + daily full backup |
| Recovery Point Objective (RPO) | < 1 minute |
| Recovery Time Objective (RTO) | < 15 minutes |

### Data Stored in Hot Tier

| Data Type | Examples |
|---|---|
| Active loan records | Loan status, balance, payment schedule |
| Recent payments | Last 30 days of payment transactions |
| Active device management | Device status, lock state, heartbeat data |
| Active customer profiles | KYC status, contact details, preferences |
| Pending workflows | Maker-checker approvals, in-progress operations |
| Recent audit events | Last 30 days of audit trail |
| Session data | Active user sessions, tokens |
| Cache data | GSMA verification cache, velocity counters |

### Indexing Strategy

| Index Type | Columns | Purpose |
|---|---|---|
| Primary key | `id` | Unique record identification |
| Customer lookup | `national_id`, `msisdn` | Customer deduplication and search |
| Loan lookup | `loan_id`, `customer_id`, `status` | Loan servicing queries |
| Device lookup | `imei`, `knox_device_id` | Device management queries |
| Temporal | `created_at`, `updated_at` | Time-range queries and migration selection |
| Composite | `tenant_id` + `status` + `created_at` | Tenant-scoped operational queries |

---

## Warm Tier (31--365 Days)

### Purpose

The warm tier provides cost-effective analytical storage for data that is no longer needed for real-time operations but is required for reporting, trend analysis, and business intelligence.

### Technical Specifications

| Attribute | Specification |
|---|---|
| Storage Engine | ClickHouse (columnar) or Parquet files on S3 |
| Query Performance | Seconds to minutes for analytical queries |
| Availability | 99.5% uptime SLA |
| Compression | Columnar compression (typically 10:1 ratio) |
| Encryption | AES-256 at rest; TLS 1.3 in transit |
| Backup Frequency | Daily incremental |
| RPO | < 24 hours |
| RTO | < 4 hours |

### Data Stored in Warm Tier

| Data Type | Examples |
|---|---|
| Historical payments | Payment transactions older than 30 days |
| Closed loan records | Fully settled or written-off loans |
| Historical audit events | Audit trail from 31--365 days ago |
| Aggregated device telemetry | Heartbeat history, location data |
| Customer interaction history | Notification delivery logs, support tickets |
| Performance metrics | Agent performance, product performance |
| CRB interaction logs | Credit check history and results |

### Analytical Capabilities

| Capability | Description |
|---|---|
| Portfolio analytics | Loan book composition, vintage analysis, cohort performance |
| Payment analytics | Collection rates, cure rates, payment behavior patterns |
| Fraud analytics | Fraud pattern detection, false positive analysis, model performance |
| Operational reporting | Agent productivity, SLA compliance, system performance |
| Tenant reporting | Per-financer portfolio health, revenue, and risk metrics |

---

## Cold Tier (1--7 Years)

### Purpose

The cold tier provides low-cost archival storage for data that must be retained for regulatory compliance but is rarely accessed. Data in this tier is retrieved on-demand for regulatory examinations, legal proceedings, or forensic investigations.

### Technical Specifications

| Attribute | Specification |
|---|---|
| Storage Engine | S3 Glacier, Azure Archive, or equivalent |
| Query Performance | Minutes to hours for retrieval |
| Availability | 99.99% durability |
| Compression | Maximum compression applied |
| Encryption | AES-256 at rest; encryption keys managed via KMS |
| Backup | Cross-region replication for disaster recovery |
| RPO | < 24 hours |
| RTO | < 24 hours (retrieval initiation) |

### Data Stored in Cold Tier

| Data Type | Examples |
|---|---|
| Archived loan records | Complete loan lifecycle records |
| Historical financial transactions | All payments, reversals, write-offs |
| Archived audit trail | Full audit event history |
| Regulatory reports | Generated compliance reports |
| Legal hold data | Data preserved for pending legal matters |
| Archived customer records | Anonymized after relationship end |
| Historical CRB interactions | Credit check and listing history |
| Archived device records | Device lifecycle, GSMA interactions |

---

## Retention Rules by Data Type

### Financial Transaction Data

| Data Element | Hot | Warm | Cold | Total Retention | Justification |
|---|---|---|---|---|---|
| Payment transactions | 30 days | 11 months | 6 years | 7 years | Financial regulatory requirement |
| Loan agreements | Loan lifetime | 1 year post-close | 6 years | 7 years post-close | Contract law |
| Disbursement records | 30 days | 11 months | 6 years | 7 years | Financial audit trail |
| Write-off records | 30 days | 11 months | 6 years | 7 years | Tax and regulatory |
| Restructuring records | 30 days | 11 months | 6 years | 7 years | Financial audit trail |
| Revenue recognition | 30 days | 11 months | 6 years | 7 years | Accounting standards |

### Personally Identifiable Information (PII)

| Data Element | Hot | Warm | Cold | Total Retention | Justification |
|---|---|---|---|---|---|
| Customer name, DOB | Relationship active | 1 year post-close | Anonymized | Relationship + 1 year | Data minimization |
| National ID number | Relationship active | 1 year post-close | Hashed only | Relationship + 1 year (plain), 7 years (hashed) | Identity verification |
| MSISDN | Relationship active | 1 year post-close | Removed | Relationship + 1 year | Contact data |
| Biometric hashes | Relationship active | 6 months post-close | Deleted | Relationship + 6 months | Biometric data regulations |
| Physical address | Relationship active | Deleted on tier migration | -- | Relationship only | Data minimization |
| Email address | Relationship active | 1 year post-close | Removed | Relationship + 1 year | Contact data |

### Audit Log Data

| Data Element | Hot | Warm | Cold | Total Retention | Justification |
|---|---|---|---|---|---|
| Financial action audit events | 30 days | 11 months | 6 years | 7 years | Financial regulations |
| Access/login audit events | 30 days | 11 months | 4 years | 5 years | Security compliance |
| Configuration change events | 30 days | 11 months | 6 years | 7 years | Operational compliance |
| Maker-checker workflow events | 30 days | 11 months | 6 years | 7 years | Authorization records |
| Fraud investigation records | 30 days | 11 months | 6 years | 7 years | Legal and regulatory |

### Device Data

| Data Element | Hot | Warm | Cold | Total Retention | Justification |
|---|---|---|---|---|---|
| IMEI records | Loan lifetime + 1 year | 2 years | 4 years | 7 years | Device tracking |
| Knox Guard enrollment data | Loan lifetime | 1 year post-disenrollment | 4 years | 6 years | Device management |
| Device heartbeat data | 30 days | 6 months | Deleted | 7 months | Operational monitoring |
| SIM event records | 30 days | 11 months | 6 years | 7 years | Fraud evidence |
| GSMA check results | Loan lifetime | 1 year post-close | 4 years | 6 years | Verification records |
| Location data (device) | 7 days | Deleted | -- | 7 days | Privacy sensitivity |

---

## Automated Tier Migration

### Migration Schedule

| Migration | Frequency | Window | Target Data |
|---|---|---|---|
| Hot to Warm | Daily | 02:00--04:00 UTC (off-peak) | Records older than 30 days |
| Warm to Cold | Weekly | Saturday 00:00--06:00 UTC | Records older than 365 days |
| Cold to Purge | Monthly | First Sunday 00:00--06:00 UTC | Records past retention period |

### Migration Process

| Step | Description | Validation |
|---|---|---|
| 1 | Identify records eligible for migration based on age and type | Record count verification |
| 2 | Transform data for target tier (e.g., denormalize for columnar storage, anonymize PII for cold) | Schema validation |
| 3 | Write data to target tier | Write confirmation |
| 4 | Verify data integrity in target tier (checksum comparison) | Checksum match |
| 5 | Mark source records as migrated | Status update |
| 6 | Delete source records after verification hold period (7 days) | Final verification before delete |
| 7 | Log migration event in audit trail | Audit record |

### Migration Safeguards

| Safeguard | Description |
|---|---|
| Verification hold | Source data retained for 7 days after migration, allowing rollback |
| Checksum validation | SHA-256 checksums computed before and after migration must match |
| Record count reconciliation | Source count must equal target count |
| Dry run mode | Migrations can be run in dry-run mode to preview affected records |
| Rollback capability | Failed migrations are automatically rolled back; source data preserved |
| Monitoring | Migration job progress, duration, and error rates tracked via dashboard |
| Alerting | Failed migration triggers immediate operations alert |

---

## Data Purge Policy

### Purge Criteria

Data is eligible for permanent deletion when all of the following conditions are met:

| Condition | Requirement |
|---|---|
| Retention period expired | Data has exceeded its maximum retention duration |
| No legal hold | Data is not subject to any pending legal matter |
| No active regulatory request | No outstanding regulatory examination request |
| No active fraud investigation | Data is not evidence in an open fraud case |
| Purge approval | Automated purge jobs run with pre-approved policies; exceptions require manual approval |

### Purge Process

1. **Identification**: Automated job identifies records past retention period.
2. **Legal Hold Check**: Cross-reference against active legal holds.
3. **Regulatory Request Check**: Cross-reference against pending regulatory requests.
4. **Fraud Investigation Check**: Cross-reference against open fraud cases.
5. **Purge Execution**: Records meeting all criteria are permanently deleted.
6. **Verification**: Verify records are no longer accessible.
7. **Audit**: Log purge event with record counts and categories.
8. **Certificate**: Generate purge certificate for compliance records.

### Secure Deletion Methods

| Tier | Deletion Method | Verification |
|---|---|---|
| Hot (PostgreSQL) | `DELETE` with `VACUUM` | Query returns zero results |
| Warm (Columnar) | Partition drop or file deletion | File system verification |
| Cold (Archive) | Object deletion via cloud API | Object listing verification |
| All tiers | Encryption key rotation for additional assurance | Key destruction certificate |

---

## Right to Erasure Compliance

### Scope

Under applicable data protection regulations, customers may request erasure of their personal data. The platform handles these requests within the constraints of financial regulatory obligations.

### Erasable vs. Non-Erasable Data

| Data Category | Erasable | Reason |
|---|---|---|
| Marketing preferences | Yes | No regulatory retention requirement |
| Communication history (marketing) | Yes | No regulatory retention requirement |
| Physical address | Yes | Not required for financial records |
| Email address | Yes (after relationship end) | Contact data not part of financial record |
| Biometric data | Yes (after retention period) | Biometric data regulations |
| Financial transaction records | No (during retention period) | Financial regulatory requirement |
| Audit trail entries | No (during retention period) | Regulatory compliance |
| Loan agreement data | No (during retention period) | Contract law |
| CRB interaction records | No (during retention period) | Credit reporting regulations |
| Fraud investigation data | No (during retention period or while case is open) | Legal and regulatory |

### Erasure Process

| Step | Action | SLA |
|---|---|---|
| 1 | Customer submits erasure request (via tenant or direct) | -- |
| 2 | Verify customer identity | 24 hours |
| 3 | Identify all data linked to the customer | 48 hours |
| 4 | Classify data as erasable vs. non-erasable | 48 hours |
| 5 | Execute erasure on all erasable data | 72 hours |
| 6 | Anonymize remaining non-erasable data where possible | 72 hours |
| 7 | Confirm completion to customer | 30 days (from request) |
| 8 | Log erasure in audit trail (without the erased PII) | Immediate |

### Anonymization Techniques

| Technique | Application | Example |
|---|---|---|
| Pseudonymization | Replace identifiers with tokens | Name becomes `CUST-ANON-001234` |
| Generalization | Reduce precision | Date of birth becomes year of birth |
| Suppression | Remove field entirely | Phone number removed |
| Hashing | One-way transformation | National ID becomes SHA-256 hash |
| Aggregation | Replace individual records with aggregate | Individual payments become monthly totals |

---

## Backup and Disaster Recovery

### Backup Strategy

| Tier | Backup Method | Frequency | Retention | Location |
|---|---|---|---|---|
| Hot | Continuous WAL archiving + daily full snapshot | Continuous + daily | 30 days of backups | Secondary region |
| Warm | Daily incremental + weekly full | Daily/weekly | 90 days of backups | Secondary region |
| Cold | Cross-region replication | Continuous | Duration of retention | Secondary region |

### Disaster Recovery Plan

| Scenario | RPO | RTO | Procedure |
|---|---|---|---|
| Single database failure | < 1 minute | < 15 minutes | Automatic failover to synchronous standby |
| Availability zone failure | < 1 minute | < 30 minutes | Failover to secondary AZ replica |
| Region failure | < 1 hour | < 4 hours | Restore from cross-region backup |
| Data corruption (logical) | Point-in-time | < 1 hour | Point-in-time recovery using WAL archive |
| Ransomware / malicious deletion | < 24 hours | < 4 hours | Restore from immutable backup (no overwrite) |

### Backup Security

| Measure | Implementation |
|---|---|
| Encryption | All backups encrypted at rest with AES-256 |
| Access control | Backup access restricted to disaster recovery role |
| Immutability | Backup retention lock prevents premature deletion |
| Testing | Monthly backup restoration test |
| Monitoring | Backup job success/failure alerts |
| Key management | Encryption keys stored in separate KMS with independent access controls |

### Recovery Testing

| Test Type | Frequency | Scope | Success Criteria |
|---|---|---|---|
| Hot tier restore | Monthly | Single table restore from backup | Data matches, referential integrity preserved |
| Warm tier restore | Quarterly | Full partition restore | Query results match pre-backup data |
| Cold tier retrieval | Quarterly | Random sample retrieval | Data accessible and readable within RTO |
| Full DR failover | Annually | Complete platform failover to secondary region | All services operational within RTO |
| Point-in-time recovery | Semi-annually | Restore to specific timestamp | Transaction consistency verified |

---

## Compliance Matrix

| Regulation / Standard | Requirement | Platform Implementation |
|---|---|---|
| Central Bank regulations | 7-year retention for financial records | Cold tier retention of 7 years for all financial data |
| Data Protection Act | Data minimization, right to erasure | PII anonymization, erasure process, purpose limitation |
| PCI DSS | Cardholder data security (if applicable) | Not applicable (mobile money, not card-based) |
| Credit Bureau regulations | CRB interaction record retention | 7-year retention of CRB check and listing records |
| Anti-Money Laundering (AML) | Transaction monitoring records | 7-year retention of all financial transactions |
| Tax regulations | Financial record availability | 7-year retention with retrieval capability |
| Consumer protection | Loan agreement availability | Loan records retained for loan lifetime + 7 years |

---

## Monitoring and Reporting

### Operational Metrics

| Metric | Target | Alert Threshold |
|---|---|---|
| Hot tier storage utilization | < 70% capacity | > 80% capacity |
| Migration job success rate | 100% | Any failure |
| Migration job duration | Within maintenance window | Exceeds window by 30% |
| Warm tier query performance | < 30 seconds for standard reports | > 60 seconds |
| Cold tier retrieval time | < 4 hours | > 8 hours |
| Purge job completion | Monthly on schedule | Missed schedule |
| Backup success rate | 100% | Any failure |

### Compliance Reporting

| Report | Frequency | Audience |
|---|---|---|
| Data retention compliance summary | Monthly | Data Protection Officer |
| Erasure request status | Monthly | Data Protection Officer |
| Backup and recovery test results | Quarterly | IT Management, Auditors |
| Storage utilization and cost | Monthly | Operations, Finance |
| Legal hold inventory | Monthly | Legal, Compliance |
| Purge certificate summary | Monthly | Compliance, Auditors |

---

## Related Documentation

- [Audit Trail](audit-trail.md)
- [Fraud Risk Framework](../fraud-prevention/fraud-framework.md)
- [Customer Registry and Deduplication](../customer-registry/deduplication.md)
- [Notification Service](../notifications/notification-service.md)
