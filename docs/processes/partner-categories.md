# Partner Categories

## 1. Overview

Every partner onboarded onto the IInovi platform is classified into exactly one **partner category**. The category determines the default configuration surface -- including integration depth, credit scoring strategy, deposit rules, payment terms, and operational workflows -- that the platform applies during tenant provisioning.

This document defines each supported category, explains its characteristics and typical integration pattern, and describes how the category influences downstream platform behavior.

---

## 2. Category Summary

| Category | Code | Typical Examples | Integration Depth | ERP Pattern |
|----------|------|------------------|-------------------|-------------|
| Telco | `telco` | Safaricom, MTN, Airtel, Vodacom | Deep (real-time APIs) | REST Push / Direct DB |
| Reseller | `reseller` | Regional device distributors | Standard (scheduled sync) | REST Pull / SFTP Batch |
| Authorized Dealer | `authorized_dealer` | Franchise operators under OEM/telco brand | Standard to Deep | REST Pull / REST Push |
| Retail Chain | `retail_chain` | Multi-branch electronics retailers | Standard (centralized ERP) | REST Pull / SFTP Batch |

---

## 3. Telco

### 3.1 Characteristics

Telcos are **primary partners** -- mobile network operators with a direct subscriber base. They represent the highest-volume, deepest-integration tier on the platform.

- **Subscriber Data Access**: Telcos can provide real-time subscriber metadata (network tenure, airtime top-up history, data usage patterns, mobile money transaction history) that feeds directly into the credit scoring engine.
- **Mobile Money Integration**: Telcos typically operate or are tightly coupled with the dominant mobile money platform in their market (e.g., M-Pesa for Safaricom, MoMo for MTN). This allows seamless deposit collection and repayment via STK push or USSD prompts.
- **Large Device Portfolios**: Telcos maintain extensive device catalogs sourced from multiple OEMs, often with exclusive launch windows for new models.
- **Nationwide Footprint**: Hundreds or thousands of retail points, including owned shops and agent networks.
- **Co-branded Experience**: The customer-facing BNPL journey is typically co-branded with the telco's identity.

### 3.2 Integration Depth

| Integration Point | Detail |
|--------------------|--------|
| **ERP Sync** | REST Push (event-driven) or Direct DB Connector for real-time stock visibility |
| **Subscriber Scoring** | Real-time API to pull subscriber profile, tenure, ARPU, and MoMo wallet history |
| **Payment Collection** | Direct STK Push integration for deposits and installments |
| **USSD Channel** | USSD shortcode hosted by the telco routes to IInovi backend for pre-order and repayment |
| **Notifications** | In-network SMS delivery at preferential rates |
| **Airtime/Data Bundles** | Optional: bundle incentives (bonus data on timely payment) via telco provisioning API |

### 3.3 Typical ERP Systems

- SAP S/4HANA
- Oracle EBS
- Custom in-house inventory platforms
- Warehouse Management Systems (WMS) with API layers

### 3.4 Default Configuration

| Parameter | Default Value |
|-----------|---------------|
| Scoring Strategy | `telco_score` (primary), `bureau_score` (fallback) |
| Min Deposit | 10% |
| Tenor Options | 90, 180, 270, 360 days |
| Frequency | Daily |
| Grace Period | 3 days |
| Lock Trigger | 7 days overdue |
| Catalog Sync | Real-time push or every 15 minutes |

---

## 4. Reseller

### 4.1 Characteristics

Resellers are **independent distributors** that purchase devices from OEMs or authorized distributors and sell them to end customers. They do not have a direct subscriber relationship with the mobile network.

- **OEM/Distributor Relationship**: Resellers source inventory from one or more OEMs (Samsung, Xiaomi, Tecno, etc.) or regional distributors. They do not manufacture or import directly.
- **Limited Customer Data**: Unlike telcos, resellers do not have access to subscriber-level network data. Credit scoring relies on alternative data sources or credit bureau pulls.
- **Smaller Scale**: Typically fewer retail locations than telcos or retail chains, often operating in a single city or region.
- **Simpler ERP**: Inventory is often managed in lightweight systems (e.g., Odoo, QuickBooks, spreadsheets with CSV export).
- **Price Sensitivity**: Resellers operate on thinner margins, making competitive BNPL pricing and low platform fees critical to adoption.

### 4.2 Integration Depth

| Integration Point | Detail |
|--------------------|--------|
| **ERP Sync** | REST Pull on a scheduled basis (hourly or daily), or SFTP batch upload |
| **Subscriber Scoring** | Not available; relies on `bureau_score` or `alternative_score` |
| **Payment Collection** | Mobile money collection via aggregator API (not direct STK push) |
| **USSD Channel** | Shared USSD shortcode (IInovi-operated) with partner routing |
| **Notifications** | SMS via third-party gateway |

### 4.3 Typical ERP Systems

- Odoo
- Sage
- QuickBooks with inventory module
- Custom spreadsheets exported to CSV/JSON

### 4.4 Default Configuration

| Parameter | Default Value |
|-----------|---------------|
| Scoring Strategy | `bureau_score` or `alternative_score` |
| Min Deposit | 20% |
| Tenor Options | 90, 180 days |
| Frequency | Weekly or Monthly |
| Grace Period | 5 days |
| Lock Trigger | 10 days overdue |
| Catalog Sync | Hourly or daily batch |

---

## 5. Authorized Dealer

### 5.1 Characteristics

Authorized dealers operate under a **franchise or brand authorization** from an OEM (e.g., Samsung Authorized Store) or a telco (e.g., Safaricom dealer). They carry a curated subset of the authorizing brand's catalog and must comply with the brand's retail standards.

- **Franchise Model**: Dealers sign a franchise or authorization agreement with the brand principal. This may impose pricing floors, merchandising requirements, and reporting obligations.
- **Curated Catalog**: The device catalog is typically limited to the authorizing brand's products, though some dealers carry accessories and complementary devices from other brands.
- **Brand Compliance**: The IInovi BNPL experience must align with the brand principal's visual identity and customer experience guidelines.
- **Training Requirements**: Staff must complete brand-specific training; IInovi cashier onboarding includes brand compliance modules where applicable.
- **Mid-Range Integration**: Integration depth sits between a reseller and a telco. Some authorized dealers have access to the brand principal's centralized inventory system.

### 5.2 Integration Depth

| Integration Point | Detail |
|--------------------|--------|
| **ERP Sync** | REST Pull from brand principal's dealer portal API, or local ERP |
| **Subscriber Scoring** | Bureau score; may have access to brand loyalty program data |
| **Payment Collection** | Mobile money via aggregator; some dealers use the brand principal's payment infrastructure |
| **Notifications** | SMS via third-party gateway or brand principal's messaging platform |

### 5.3 Typical ERP Systems

- Brand principal's dealer portal (Samsung Partner Portal, etc.)
- SAP Business One
- Odoo or Sage (local installation)

### 5.4 Default Configuration

| Parameter | Default Value |
|-----------|---------------|
| Scoring Strategy | `bureau_score`, optionally `hybrid_score` if brand loyalty data available |
| Min Deposit | 15% |
| Tenor Options | 90, 180, 270 days |
| Frequency | Weekly or Monthly |
| Grace Period | 3 days |
| Lock Trigger | 7 days overdue |
| Catalog Sync | Every 30 minutes or hourly |

---

## 6. Retail Chain

### 6.1 Characteristics

Retail chains are **multi-location electronics or general retailers** that sell devices alongside other consumer electronics. They operate under a single legal entity with centralized procurement and ERP.

- **Multi-Location Operations**: Chains operate tens to hundreds of outlets across a country or region. Each location must be registered as a separate shop in the platform, but they share a single tenant and catalog.
- **Centralized ERP**: A single ERP instance manages inventory across all branches. Stock levels are tracked per warehouse/location.
- **Diverse Catalog**: Retail chains carry devices from multiple OEMs and across all price segments (entry-level to flagship).
- **Existing POS Infrastructure**: Chains typically have established POS systems. IInovi integrates as a payment method within their existing checkout flow, or provides a companion app used alongside the existing POS.
- **Higher Throughput**: Transaction volumes can be significant, requiring the platform to handle concurrent POS operations across many locations.

### 6.2 Integration Depth

| Integration Point | Detail |
|--------------------|--------|
| **ERP Sync** | REST Pull from centralized ERP; stock levels per branch |
| **Subscriber Scoring** | Bureau score or alternative score |
| **Payment Collection** | Mobile money via aggregator; POS-integrated card payments where applicable |
| **POS Integration** | API or SDK for embedding BNPL as a payment method in existing POS |
| **Notifications** | SMS via third-party gateway; in-store receipt printing |

### 6.3 Typical ERP Systems

- SAP Retail
- Oracle Retail
- Microsoft Dynamics 365
- Custom retail management systems

### 6.4 Default Configuration

| Parameter | Default Value |
|-----------|---------------|
| Scoring Strategy | `bureau_score` or `alternative_score` |
| Min Deposit | 15% |
| Tenor Options | 90, 180, 270, 360 days |
| Frequency | Monthly |
| Grace Period | 5 days |
| Lock Trigger | 10 days overdue |
| Catalog Sync | Every 15 minutes or real-time via webhook |

---

## 7. Category-Specific Configuration Impact

### 7.1 Credit Scoring

The partner category determines the **primary scoring strategy** and the data sources available to the scoring engine.

| Category | Primary Strategy | Supplementary Data | Score Range |
|----------|-----------------|-------------------|-------------|
| Telco | Telco Score | Subscriber ARPU, MoMo history, network tenure | 0 -- 1000 |
| Reseller | Bureau Score or Alternative Score | Credit bureau record, app telemetry | 300 -- 850 (bureau) or 0 -- 100 (alt) |
| Authorized Dealer | Bureau Score | Credit bureau, brand loyalty program | 300 -- 850 |
| Retail Chain | Bureau Score or Alternative Score | Credit bureau, app telemetry | 300 -- 850 (bureau) or 0 -- 100 (alt) |

The scoring engine normalizes scores from different strategies into a unified **IInovi Risk Band** (A through E) that drives approval decisions, deposit requirements, and tenor availability.

### 7.2 Device Catalog

| Category | Catalog Source | Breadth | IMEI Tracking |
|----------|---------------|---------|---------------|
| Telco | Telco's central inventory | Wide (all OEMs) | Per-IMEI, real-time |
| Reseller | Reseller's local inventory | Narrow (selected OEMs) | Per-IMEI, batch |
| Authorized Dealer | Brand principal's catalog | Brand-limited | Per-IMEI, near-real-time |
| Retail Chain | Chain's centralized inventory | Wide (all OEMs) | Per-IMEI, per-branch |

### 7.3 Payment Options

| Category | Deposit Floor | Frequencies | Max Tenor |
|----------|---------------|-------------|-----------|
| Telco | 10% | Daily, Weekly, Monthly | 360 days |
| Reseller | 20% | Weekly, Monthly | 180 days |
| Authorized Dealer | 15% | Weekly, Monthly | 270 days |
| Retail Chain | 15% | Monthly | 360 days |

These are **defaults** applied at onboarding. Partners may negotiate adjusted terms subject to IInovi Risk approval.

---

## 8. Category Selection Rules

A partner's category is determined during the application review stage based on the following criteria:

1. **Is the applicant a licensed mobile network operator?** If yes, classify as **Telco**.
2. **Does the applicant operate under a franchise or brand authorization agreement?** If yes, classify as **Authorized Dealer**.
3. **Does the applicant operate three or more retail locations under a single legal entity?** If yes, classify as **Retail Chain**.
4. **Otherwise**, classify as **Reseller**.

Edge cases (e.g., a telco subsidiary that operates as a retail chain) are resolved by IInovi Ops during KYB review, with the guiding principle being: choose the category whose default configuration most closely matches the partner's operational reality.

---

## 9. Future Category Extensibility

The category system is designed to accommodate new partner types as the platform expands into adjacent markets. Planned and anticipated future categories include:

### 9.1 Planned Categories

| Category | Code | Use Case |
|----------|------|----------|
| **MFI (Microfinance Institution)** | `mfi` | Microfinance lenders offering device financing as part of a broader credit product |
| **SACCO / Cooperative** | `sacco` | Savings and credit cooperatives bundling device financing for members |
| **Employer** | `employer` | Employers offering payroll-deducted device financing as a staff benefit |

### 9.2 Extensibility Mechanism

Adding a new category involves the following steps:

1. **Define the category enum value** in the `PartnerCategory` type.
2. **Create a default configuration template** specifying scoring strategy, deposit rules, payment terms, and integration pattern.
3. **Implement any category-specific integration adapters** (e.g., a payroll deduction adapter for the Employer category).
4. **Update the onboarding flow** to include category-specific document requirements and validation rules.
5. **Configure feature flags** to gate category availability by market or rollout phase.

The platform's configuration-driven architecture ensures that new categories do not require changes to core business logic. Category-specific behavior is resolved at runtime through strategy patterns and configuration lookup, not conditional branching.

---

## 10. Category Data Model

The category is stored as an enum on the `Partner` entity and referenced throughout the platform to resolve default configuration.

```
PartnerCategory:
  telco
  reseller
  authorized_dealer
  retail_chain

PartnerCategoryConfig:
  category              : PartnerCategory
  default_scoring_ids   : UUID[]
  default_min_deposit   : decimal(5,2)
  default_max_deposit   : decimal(5,2)
  default_tenors        : int[]
  default_frequency     : enum(daily, weekly, monthly)
  default_grace_period  : int
  default_late_fee_type : enum(flat, percentage)
  default_late_fee_val  : decimal(10,2)
  default_lock_trigger  : int
  erp_sync_patterns     : enum[]
  required_documents    : string[]
  feature_flags         : jsonb
  created_at            : timestamp
  updated_at            : timestamp
```

When a new partner is onboarded, the platform copies the `PartnerCategoryConfig` defaults into the partner's tenant-level configuration, where they can then be customized by the partner admin or IInovi Ops without affecting the category-wide template.
