# KYC/AML Compliance

## 1. Overview

The IInovi platform operates under strict Know Your Customer (KYC) and Anti-Money Laundering (AML) obligations across all target markets. As a mobile device lending platform handling financial transactions through mobile money channels, the platform must verify customer identities, assess risk, and monitor for suspicious activity in compliance with each jurisdiction's regulatory framework.

This document defines the KYC verification requirements, AML/CFT controls, tiered KYC levels, and per-market regulatory obligations that the platform enforces.

### Guiding Principles

| Principle | Application |
|---|---|
| **Risk-Based Approach** | KYC intensity scales with transaction value and customer risk profile |
| **Regulatory Alignment** | Each market's specific requirements are met or exceeded |
| **Data Minimization** | Only data necessary for verification and compliance is collected |
| **Tenant Configurability** | Tenants configure KYC workflows within platform-enforced regulatory minimums |
| **Auditability** | Every KYC decision, verification attempt, and override is recorded immutably |

---

## 2. KYC Verification Requirements

### 2.1 Accepted Identity Documents

The platform accepts the following primary identity documents for customer verification, varying by market:

| Market | Primary Document | Secondary Documents | Biometric Requirement |
|---|---|---|---|
| **Kenya** | National ID (Huduma Namba / legacy ID) | Passport, Alien Certificate | Not mandated for standard KYC |
| **Nigeria** | National Identification Number (NIN) / NIN slip | International Passport, Voter's Card, Driver's License | BVN-linked biometric verification required |
| **South Africa** | South African ID (Smart ID Card / Green Book) | Passport, Asylum Seeker Permit | Not mandated for standard KYC |
| **Ghana** | Ghana Card (National ID) | Passport, Voter's ID | Ghana Card includes biometric data; verification via NIA API |

### 2.2 Verification Data Points

For each customer, the following data points are collected and verified:

| Data Point | Collection Method | Verification Method |
|---|---|---|
| Full legal name | Customer-provided | Cross-referenced with ID document |
| Date of birth | Customer-provided | Cross-referenced with ID document |
| National ID number | Customer-provided or scanned | Validated against national ID registry (where available) |
| Photograph | Captured at POS or via app | Compared to ID document photo (manual or automated facial match) |
| Residential address | Customer-provided | Utility bill, bank statement, or geolocation confirmation |
| Phone number (MSISDN) | Customer-provided | MSISDN ownership verification via telco adapter |
| Biometric data | Captured where mandated | Matched against national biometric database (Ghana Card via NIA, Nigeria via NIMC) |

### 2.3 Identity Verification Providers

The platform integrates with third-party identity verification providers through a pluggable adapter architecture:

| Provider | Markets | Capabilities |
|---|---|---|
| **Smile Identity** | Kenya, Nigeria, Ghana | Document verification, facial biometric matching, NIN/BVN lookup |
| **Onfido** | All markets | Document OCR, liveness detection, facial comparison |
| **VerifyMe (Nigeria)** | Nigeria | BVN verification, NIN lookup, address verification |
| **NIA API (Ghana)** | Ghana | Ghana Card biometric verification |
| **Home Affairs DHA (SA)** | South Africa | ID number validation (via approved integrators) |

The adapter pattern allows new providers to be added without modifying core platform logic. Provider selection is configurable per tenant and per market.

---

## 3. Customer Onboarding KYC

### 3.1 POS Onboarding (In-Person)

The primary onboarding channel occurs at the point of sale during device issuance. The cashier guides the customer through the KYC process:

1. **Customer presents ID document.** The cashier scans or manually captures the document details.
2. **Document validation.** The system validates the ID number format and checks against the national registry (where integrated).
3. **Photo capture.** The cashier photographs the customer using the POS device camera. For markets with biometric requirements, biometric data is captured.
4. **MSISDN verification.** The customer's primary phone number is verified via OTP or telco MSISDN lookup.
5. **Address capture.** The customer provides their residential address. For simplified KYC, self-declaration is accepted.
6. **Consent collection.** The customer provides explicit consent for KYC data processing, CRB checks, and telco data access (see [Data Privacy and Consent](data-privacy-consent.md)).
7. **KYC decision.** The system performs automated checks (ID validation, sanctions screening, CRB blacklist) and returns a KYC decision.

### 3.2 Remote Onboarding (Mobile App)

Customers may also onboard remotely via the mobile application:

1. **Document upload.** The customer photographs their ID document using the device camera.
2. **OCR extraction.** The system extracts document data via OCR (optical character recognition) through the identity verification provider.
3. **Liveness check.** The customer completes a liveness detection challenge (head turn, blink detection) to prevent spoofing.
4. **Facial comparison.** The selfie is compared against the document photograph.
5. **MSISDN verification.** An OTP is sent to the customer's phone number for ownership confirmation.
6. **Consent collection.** Digital consent is captured with timestamp, consent version, and scope.
7. **KYC decision.** Automated checks and verification are performed; results are stored with the customer record.

### 3.3 USSD Onboarding (Simplified)

For USSD-channel customers, only simplified KYC is available due to the limited interface:

1. MSISDN is captured from the telco gateway (trusted source).
2. Customer provides their national ID number via USSD input.
3. The ID number is validated against the national registry.
4. Simplified KYC is granted, limiting the customer to low-value products only.
5. Full KYC must be completed at a POS or via the app to access higher-value products.

---

## 4. KYC Data Storage and Retention

### 4.1 Storage Architecture

All KYC data is stored with the following protections:

| Control | Implementation |
|---|---|
| **Encryption at rest** | KYC data fields (national ID, biometric hashes, document images) are encrypted using AES-256-GCM at the application level |
| **Tenant isolation** | KYC records are scoped by `tenant_id` and enforced via Row-Level Security (RLS) |
| **Access control** | KYC data is accessible only to authorized roles (Partner Admin, designated KYC officers); Cashiers see masked values after onboarding |
| **Audit logging** | Every access to KYC records is audit-logged, including read operations |
| **Document storage** | ID document images and biometric data are stored in encrypted blob storage, separate from the relational database |

### 4.2 Retention Policy

KYC data retention follows the most restrictive applicable regulation per market:

| Market | Minimum Retention Period | Regulatory Basis |
|---|---|---|
| **Kenya** | 7 years after relationship ends | Proceeds of Crime and Anti-Money Laundering Act (POCAMLA) |
| **Nigeria** | 5 years after relationship ends | Money Laundering (Prevention and Prohibition) Act 2022 |
| **South Africa** | 5 years after relationship ends | Financial Intelligence Centre Act (FICA) |
| **Ghana** | 5 years after relationship ends | Anti-Money Laundering Act, 2020 (Act 1044) |

After the retention period expires, KYC data is anonymized. Original identity documents and biometric data are purged; anonymized records are retained for aggregate analytics only.

---

## 5. Tiered KYC Levels

The platform implements a tiered KYC framework that balances regulatory compliance with customer accessibility. Lower-value products require less intensive verification, reducing barriers to entry while maintaining compliance.

### 5.1 Tier Definitions

| KYC Tier | Verification Level | Maximum Device Value | Maximum Outstanding Credit | Required Data |
|---|---|---|---|---|
| **Tier 1 (Simplified)** | Basic identity check | Up to USD 100 equivalent | Single device only | National ID number, MSISDN, name, OTP verification |
| **Tier 2 (Standard)** | Full identity verification | Up to USD 500 equivalent | Up to 2 concurrent devices | National ID + document scan/photo, facial match, address, MSISDN ownership |
| **Tier 3 (Enhanced)** | Enhanced Due Diligence | Above USD 500 | Up to 3 concurrent devices | All Tier 2 + proof of income, utility bill, enhanced CRB check, biometric (where mandated) |

### 5.2 Tier Upgrade Process

Customers can upgrade their KYC tier by providing additional documentation:

1. Customer or agent initiates a tier upgrade request.
2. System identifies the additional data points required for the target tier.
3. Customer provides the required documentation (via POS or app).
4. Verification is performed against the identity provider.
5. Upon successful verification, the customer's KYC tier is upgraded.
6. The customer gains access to higher-value products and credit limits.

### 5.3 Tenant Configuration

Tenants may apply stricter KYC requirements than the platform minimum but cannot relax them below regulatory thresholds. Tenant-configurable parameters include:

- Minimum KYC tier required per loan product
- Maximum device value per tier (within platform limits)
- Additional verification requirements (e.g., requiring employer letter for all tiers)
- Mandatory biometric verification even in markets where it is not legally required

---

## 6. Enhanced Due Diligence (EDD)

Enhanced Due Diligence is triggered for customers or transactions that present elevated risk. EDD supplements standard KYC with deeper investigation.

### 6.1 EDD Triggers

| Trigger | Threshold | Action |
|---|---|---|
| **High-value device** | Device value exceeds Tier 2 maximum | Require Tier 3 KYC; additional documentation |
| **Multiple active loans** | Customer requests a third concurrent device | Manual review by credit team |
| **Adverse CRB record** | Prior defaults or restructures on credit bureau report | Manual review; additional income verification |
| **PEP (Politically Exposed Person)** | Customer matches PEP database | Senior management approval required |
| **Sanctions match** | Name matches sanctions list (partial or full) | Escalate to compliance officer; no onboarding until cleared |
| **Geographic risk** | Customer or transaction originates from high-risk jurisdiction | Additional address verification; source of funds inquiry |
| **Unusual transaction pattern** | Multiple applications in short period, or applications across multiple tenants | Fraud team review; velocity check alert |

### 6.2 EDD Process

1. Standard KYC is completed first.
2. System flags the account or transaction for EDD based on trigger rules.
3. The compliance team receives an EDD case for review.
4. Additional documentation is requested from the customer (proof of income, bank statements, employer letter).
5. The compliance officer reviews all available data and makes a decision (approve, decline, or request further information).
6. The decision is recorded with full justification and supporting evidence.
7. EDD cases are subject to periodic review (at least annually for ongoing relationships).

---

## 7. AML/CFT Controls

### 7.1 Transaction Monitoring

The platform monitors all financial transactions for patterns indicative of money laundering or terrorist financing. The Fraud Detection Service performs real-time and batch analysis.

| Monitoring Type | Method | Example Patterns |
|---|---|---|
| **Real-time** | Rule-based screening on every transaction | Single transaction exceeding reporting threshold; rapid successive transactions |
| **Batch analysis** | Periodic pattern analysis across transaction history | Structuring (splitting transactions to avoid thresholds); round-amount patterns; unusual geographic clusters |
| **Velocity checks** | Rate-based monitoring | Multiple loan applications from the same identity within a short window; excessive payment reversals |
| **Behavioral analysis** | Deviation from established customer profile | Sudden change in payment channel; payment from a third-party MSISDN not previously associated with the customer |

### 7.2 Suspicious Transaction Reporting (STR)

When a transaction or pattern is flagged as suspicious, the following process applies:

1. The Fraud Detection Service generates an alert with a risk score and supporting data.
2. The compliance officer reviews the alert and determines whether it constitutes a reportable suspicious transaction.
3. If reportable, a Suspicious Transaction Report (STR) is filed with the relevant Financial Intelligence Centre:

| Market | Reporting Authority | Reporting Obligation |
|---|---|---|
| **Kenya** | Financial Reporting Centre (FRC) | Within 7 working days of suspicion |
| **Nigeria** | Nigerian Financial Intelligence Unit (NFIU) | Immediately upon suspicion (within 24 hours for terrorism financing) |
| **South Africa** | Financial Intelligence Centre (FIC) | Within 15 business days of suspicion (within 5 days for terrorism financing) |
| **Ghana** | Financial Intelligence Centre (FIC Ghana) | Within 3 working days of suspicion |

4. The STR is filed electronically using the reporting authority's prescribed format (goAML platform where applicable).
5. All STR filings are recorded internally with restricted access (compliance officers only).
6. No tipping-off: the customer is not informed that an STR has been filed.

### 7.3 Sanctions Screening

All customers are screened against sanctions lists at onboarding and periodically thereafter:

| Sanctions List | Source | Screening Frequency |
|---|---|---|
| **UN Security Council** | UN Consolidated List | At onboarding + daily batch |
| **OFAC (US)** | SDN List, Consolidated Non-SDN | At onboarding + daily batch |
| **EU Sanctions** | EU Consolidated Financial Sanctions List | At onboarding + daily batch |
| **Local sanctions** | National lists per market | At onboarding + daily batch |
| **PEP databases** | Commercial PEP screening providers | At onboarding + quarterly batch |

Screening is performed using fuzzy matching algorithms to account for name transliterations, spelling variations, and aliases. Match results are categorized as exact match, strong match, or weak match, with escalation thresholds configured per risk appetite.

### 7.4 Currency Transaction Reporting (CTR)

Transactions exceeding prescribed thresholds must be reported regardless of suspicion:

| Market | CTR Threshold | Reporting Timeframe |
|---|---|---|
| **Kenya** | KES 1,000,000 (or equivalent in other currency) | Within 7 working days |
| **Nigeria** | NGN 5,000,000 (individual) / NGN 10,000,000 (corporate) | Within 7 working days |
| **South Africa** | ZAR 24,999.99 (cash) | Within 2 business days |
| **Ghana** | GHS 20,000 (or equivalent) | Within 3 working days |

While most IInovi transactions are below these thresholds (device prices typically range from USD 50 to USD 1,500), the monitoring framework is in place for completeness and for cases involving high-value devices or aggregate transaction patterns.

---

## 8. Per-Market Regulatory Requirements

### 8.1 Kenya

| Requirement | Details |
|---|---|
| **Primary Legislation** | Proceeds of Crime and Anti-Money Laundering Act (POCAMLA), 2009 |
| **Regulatory Authority** | Central Bank of Kenya (CBK); Financial Reporting Centre (FRC) |
| **KYC Requirements** | National ID or passport; MSISDN verification; face photograph; physical or digital address |
| **Simplified KYC** | Available for low-value transactions per CBK guidelines on digital credit |
| **CRB Obligation** | Mandatory credit check before lending; mandatory monthly reporting (CBK Prudential Guidelines) |
| **STR Filing** | goAML platform; 7 working days |
| **Record Retention** | 7 years |
| **Biometric** | Not mandated by regulation; optional enhancement |

### 8.2 Nigeria

| Requirement | Details |
|---|---|
| **Primary Legislation** | Money Laundering (Prevention and Prohibition) Act, 2022; CBN AML/CFT Regulations |
| **Regulatory Authority** | Central Bank of Nigeria (CBN); Nigerian Financial Intelligence Unit (NFIU) |
| **KYC Requirements** | BVN (Bank Verification Number) mandatory; NIN (National Identification Number); passport photograph |
| **Simplified KYC (Tier 1)** | Available per CBN tiered KYC framework for low-value accounts |
| **CRB Obligation** | Credit check recommended; reporting obligations per CBN guidelines |
| **STR Filing** | NFIU portal; immediately / within 24 hours for terrorism financing |
| **Record Retention** | 5 years |
| **Biometric** | BVN includes biometric data; verification via NIBSS BVN validation |

### 8.3 South Africa

| Requirement | Details |
|---|---|
| **Primary Legislation** | Financial Intelligence Centre Act (FICA), 2001 (as amended); Prevention of Organised Crime Act (POCA) |
| **Regulatory Authority** | Financial Intelligence Centre (FIC); National Credit Regulator (NCR) |
| **KYC Requirements** | South African ID number; proof of address (not older than 3 months); FICA verification |
| **Simplified KYC** | Available under FICA exemptions for low-value transactions |
| **CRB Obligation** | Mandatory credit check per National Credit Act; mandatory reporting to registered credit bureaus |
| **STR Filing** | FIC online reporting; 15 business days (5 days for terrorism financing) |
| **Record Retention** | 5 years |
| **Biometric** | Not mandated; Smart ID Card includes biometric features |

### 8.4 Ghana

| Requirement | Details |
|---|---|
| **Primary Legislation** | Anti-Money Laundering Act, 2020 (Act 1044); Anti-Terrorism Act, 2008 (Act 762) |
| **Regulatory Authority** | Bank of Ghana (BoG); Financial Intelligence Centre (FIC Ghana) |
| **KYC Requirements** | Ghana Card (mandatory primary ID per BoG directive); biometric verification via NIA |
| **Simplified KYC** | Available per BoG e-money guidelines for low-value accounts |
| **CRB Obligation** | Credit check via licensed credit bureaus (XDS Data Ghana, Hudson Price Ghana) |
| **STR Filing** | FIC Ghana portal; 3 working days |
| **Record Retention** | 5 years |
| **Biometric** | Mandated; Ghana Card biometric verification required for all account openings |

---

## 9. Ongoing Monitoring and Review

### 9.1 Periodic KYC Refresh

Customer KYC data must be refreshed periodically to ensure continued accuracy:

| Customer Risk Level | Refresh Frequency | Trigger |
|---|---|---|
| **Low risk** | Every 36 months | Automated prompt; data re-verification |
| **Medium risk** | Every 24 months | Automated prompt; document re-submission |
| **High risk / EDD** | Every 12 months | Manual review by compliance team |

### 9.2 Event-Driven Re-verification

Certain events trigger immediate KYC re-verification regardless of the scheduled refresh:

- Customer changes their registered phone number (MSISDN)
- Customer requests a significant credit limit increase
- Sanctions screening returns a new match
- Customer reports identity theft or document compromise
- Regulatory authority issues a directive for re-verification

### 9.3 Compliance Reporting

The platform generates the following compliance reports:

| Report | Frequency | Audience |
|---|---|---|
| **KYC Completion Rate** | Weekly | Compliance team, management |
| **Pending KYC Reviews** | Daily | Compliance team |
| **STR Filing Summary** | Monthly | Compliance officer, board |
| **Sanctions Screening Results** | Daily | Compliance officer |
| **KYC Refresh Backlog** | Weekly | Compliance team |
| **AML Risk Assessment** | Quarterly | Board, regulators (on request) |

---

## Related Documents

- [Data Privacy and Consent Management](data-privacy-consent.md) -- consent frameworks and data subject rights
- [Credit Bureau Integration](credit-bureau-integration.md) -- CRB check and reporting requirements
- [Consumer Protection Compliance](consumer-protection.md) -- transparent disclosure and fair treatment obligations
- [Licensing Requirements](licensing.md) -- per-market licensing obligations
- [Security Architecture](../architecture/security.md) -- encryption, access control, and audit trail
- [Documentation Index](../README.md) -- full documentation map
