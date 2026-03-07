# Alternative Credit Data Sources

## 1. Overview

Traditional credit scoring relies on formal financial history -- bank statements, credit cards, and loan records -- that a majority of consumers in emerging markets simply do not have. In Sub-Saharan Africa, Southeast Asia, and other regions where IInovi operates, an estimated 60-70% of the adult population is either unbanked or underbanked, rendering conventional credit assessment ineffective.

The IInovi platform addresses this through **alternative data sources**: non-traditional signals that correlate with creditworthiness and are available for populations excluded from formal credit systems. These data sources are integrated into the credit scoring engine as first-class scoring attributes, evaluated through the same strategy-based rules engine documented in [rules-engine.md](./rules-engine.md).

This document covers each alternative data source, its acquisition model, privacy requirements, and integration architecture.

---

## 2. Data Sources

### 2.1 Call Data Records (CDR)

**What it is**: Anonymized usage data from a customer's mobile network operator, covering voice calls, SMS, and data consumption patterns.

**Why it matters**: CDR data is a strong proxy for economic stability. Consistent usage patterns, regular top-ups, and sustained data consumption indicate a customer who can manage recurring financial commitments.

**Scoring Signals**

| Signal | Description | Credit Relevance |
|---|---|---|
| Top-up frequency | How often the customer adds airtime credit | Regular top-ups indicate steady cash flow |
| Top-up amount consistency | Variance in top-up amounts over time | Low variance suggests predictable income |
| Data usage volume | Monthly mobile data consumption in MB/GB | Higher usage correlates with economic activity |
| Voice call regularity | Consistency of outbound call patterns | Stable patterns indicate routine and stability |
| Network of contacts | Diversity and stability of called numbers | Broader, stable networks correlate with social capital |
| Roaming usage | Frequency of roaming events | Can indicate travel for work (positive) or instability (context-dependent) |
| Active days per month | Number of days with at least one usage event | High active days indicate the phone is essential to daily life |

**Typical Data Window**: 3 to 6 months of historical records.

**Limitations**: CDR data is most readily available when the lending partner is the customer's telco (or has a direct data-sharing agreement with a telco). Aggregator access is possible but adds latency and cost.

---

### 2.2 Mobile Money Transaction History

**What it is**: Transaction records from mobile money platforms, the primary financial infrastructure for hundreds of millions of users across Africa and Asia.

**Platforms**: M-Pesa (Safaricom/Vodacom), Airtel Money, MTN Mobile Money, Orange Money, Tigo Pesa, and regional equivalents.

**Scoring Signals**

| Signal | Description | Credit Relevance |
|---|---|---|
| Transaction volume | Total value of transactions over a period | Higher volume indicates economic activity |
| Transaction frequency | Number of transactions per week/month | Frequent use shows reliance on and access to financial services |
| Income consistency | Regularity of incoming transfers | Predictable inflows suggest stable income |
| Balance maintenance | Average and minimum balance over time | Ability to maintain a balance indicates financial buffer |
| Bill payment regularity | Consistent utility, rent, or service payments | Demonstrates commitment to recurring obligations |
| Savings behavior | Transfers to savings wallets or locked accounts | Active saving is a strong positive signal |
| P2P transfer patterns | Patterns in person-to-person transfers | Stable, reciprocal patterns indicate social financial network |

**Typical Data Window**: 6 to 12 months of transaction history.

**Data Richness**: Mobile money data is often the single most predictive alternative data source for underbanked populations. It provides a near-complete picture of financial behavior for users who transact primarily through mobile money.

---

### 2.3 Platform Repayment History

**What it is**: The customer's own repayment track record on the IInovi platform from prior device financing agreements.

**Why it matters**: This is the most directly predictive data source available. A customer who has successfully repaid one or more device loans is statistically far less likely to default on a subsequent one.

**Scoring Signals**

| Signal | Description | Credit Relevance |
|---|---|---|
| Number of completed loans | Total successfully repaid financing agreements | More completions indicate reliability |
| On-time payment rate | Percentage of installments paid on or before due date | Core measure of repayment discipline |
| Days past due (DPD) history | Maximum and average DPD across all loans | Low DPD indicates consistent payment behavior |
| Default history | Any prior defaults or write-offs | Strong negative signal |
| Loan progression | Whether the customer has graduated to higher-value devices | Successful progression is a strong positive signal |
| Time since last loan | Recency of the most recent repayment activity | Recent activity is more predictive than distant history |

**Availability**: This data is platform-owned and requires no external integration. It is available for any returning customer and is the highest-confidence data source in the system.

**Scoring Impact**: Repeat customers with strong repayment history can receive significantly higher credit limits and streamlined approval. The "Repeat Customer" scoring strategy weights this source at up to 40% of the total score.

---

### 2.4 Credit Bureau (CRB) Data

**What it is**: Traditional credit bureau records from regional and continental credit reference bureaus.

**Providers**

| Bureau | Coverage | Notes |
|---|---|---|
| **TransUnion** | Kenya, South Africa, multiple African markets | Widely used in East and Southern Africa |
| **Experian Africa** | South Africa, expanding pan-African | Strong in formal financial sector data |
| **Metropol CRB** | Kenya | Mandated by Central Bank of Kenya for all credit providers |
| **CreditInfo** | Multiple African markets | Growing pan-African coverage |
| **First Central Credit Bureau** | Nigeria | Required for consumer lending in Nigeria |

**Scoring Signals**

| Signal | Description | Credit Relevance |
|---|---|---|
| Bureau score | Aggregated credit score from the bureau | Direct creditworthiness indicator |
| Active accounts | Number and types of open credit accounts | Context for debt capacity |
| Delinquency records | History of late payments across all creditors | Strong negative signal |
| Inquiry count | Recent credit inquiries by other lenders | High inquiry counts may indicate financial stress |
| Outstanding debt | Total current debt obligations | Input to debt-to-income ratio |

**Coverage Gaps**: Credit bureau coverage in many target markets is limited to the formally banked population. In Kenya, Metropol CRB has relatively broad coverage due to regulatory mandates, but in many other markets, bureau data is available for fewer than 20% of the target population.

**Integration Model**: CRB data is accessed via real-time API calls at the point of credit evaluation. Consent must be obtained from the customer before any bureau inquiry.

---

### 2.5 Telco Tenure

**What it is**: The length of time a customer has maintained their current mobile number with their network operator.

**Why it matters**: Longer tenure on a mobile network is a stability indicator. Customers who maintain the same number for years are more likely to be traceable, reachable, and rooted in their community -- all of which reduce lending risk.

**Scoring Signals**

| Signal | Description | Credit Relevance |
|---|---|---|
| Tenure length | Months/years on current network | Longer tenure correlates with stability |
| SIM registration date | When the SIM was first registered | Establishes minimum tenure |
| Contract vs. prepaid | Type of mobile plan | Contract customers have passed a prior credit check |
| Number portability history | Whether the number was ported from another network | Porting indicates intentional number retention |

**Typical Thresholds**

| Tenure | Interpretation |
|---|---|
| Less than 3 months | Very new; insufficient signal |
| 3 to 12 months | Moderate; acceptable with other supporting data |
| 1 to 3 years | Good stability indicator |
| More than 3 years | Strong positive signal |

**Acquisition**: Telco tenure is typically available through the same CDR/telco API integration. When the lending partner is the telco itself, this data is trivially accessible.

---

### 2.6 Down Payment Ability

**What it is**: The customer's willingness and ability to provide an upfront cash payment toward the device cost.

**Why it matters**: A down payment serves dual purpose: it reduces the financed amount (lowering lender risk) and demonstrates the customer's ability to accumulate and commit capital (a behavioral signal of creditworthiness).

**Scoring Signals**

| Signal | Description | Credit Relevance |
|---|---|---|
| Down payment percentage | Proportion of device value paid upfront | Higher percentage reduces risk and demonstrates savings ability |
| Payment method | Cash, mobile money, bank transfer | Mobile money or bank payment provides additional verification |
| Speed of payment | Time between quote and down payment | Quick payment suggests available funds |

**Typical Tiers**

| Down Payment | Scoring Impact |
|---|---|
| 0% (no down payment) | Neutral to slight negative; full device value is at risk |
| Less than 10% | Marginal positive signal |
| 10% to 20% | Meaningful positive signal; demonstrates savings capacity |
| More than 20% | Strong positive signal; significantly reduces financed amount |

**Interaction with Credit Limit**: The credit limit from the scoring engine represents the maximum *financed* device value. A customer whose score qualifies them for a KES 20,000 device limit could finance a KES 25,000 device if they provide a KES 5,000 down payment.

---

## 3. Data Acquisition Models

The platform acquires alternative data through three primary channels, depending on the data source and the partner relationship.

### 3.1 Direct Telco API

When the lending partner is a telco (or has a contractual data-sharing agreement with one), customer data can be accessed directly through the telco's internal or partner APIs.

**Applicable data**: CDR, mobile money (if telco-operated), telco tenure, SIM registration.

**Advantages**:
- Lowest latency (direct API, no intermediary).
- Highest data freshness.
- No per-query cost from third-party aggregators.

**Requirements**:
- Technical integration with the telco's API gateway.
- Data-sharing agreement compliant with local data protection law.
- Customer consent captured at application time.

### 3.2 Aggregator

Third-party data aggregators provide normalized access to telco and financial data across multiple operators and markets through a single API.

**Applicable data**: CDR, mobile money, credit bureau, telco tenure.

**Examples**: Tiaxa, Cignifi, Branch (data layer), Pngme, Smile Identity (KYC + data).

**Advantages**:
- Single integration covers multiple telcos and markets.
- Aggregator handles operator-specific API differences.
- Often includes pre-computed credit features (e.g., "top-up regularity score").

**Disadvantages**:
- Per-query cost.
- Additional latency (aggregator sits between platform and data source).
- Data freshness depends on aggregator's own sync schedule.

### 3.3 Platform-Owned Data

Data generated by the customer's interactions with the IInovi platform itself.

**Applicable data**: Repayment history, loan performance, down payment behavior, device selection patterns.

**Advantages**:
- No external dependency or cost.
- Highest reliability and availability.
- No consent complexity beyond the platform's own terms of service.
- Grows in value as the customer base matures.

**Significance**: Platform-owned data is the strategic endgame. As the platform scales, the accumulated repayment data becomes a proprietary credit signal that no competitor can replicate. Early-stage reliance on external data (CDR, mobile money) transitions over time to a model where platform history is the dominant scoring factor.

---

## 4. Privacy and Consent Requirements

Every alternative data source carries specific privacy and consent obligations. The platform enforces a consent framework that is compliant with applicable regulations across all operating markets.

### 4.1 Regulatory Landscape

| Jurisdiction | Primary Regulation | Key Requirements |
|---|---|---|
| Kenya | Data Protection Act 2019 | Explicit consent, data minimization, right to erasure, mandatory DPA registration |
| Nigeria | Nigeria Data Protection Regulation (NDPR) / Nigeria Data Protection Act 2023 | Consent before collection, purpose limitation, mandatory Data Protection Impact Assessment |
| South Africa | Protection of Personal Information Act (POPIA) | Consent, purpose specification, data minimization, cross-border transfer restrictions |
| Pan-African | AU Convention on Cyber Security (Malabo Convention) | Guiding framework; adoption varies by country |

### 4.2 Consent Requirements by Data Source

| Data Source | Consent Type | Consent Mechanism | Retention Policy |
|---|---|---|---|
| **CDR** | Explicit opt-in | SMS-based consent flow or in-app consent screen. Customer must affirmatively agree to share telco usage data. | Retain derived features only; raw CDR data must be deleted within 30 days of scoring. |
| **Mobile money** | Explicit opt-in | Consent screen with clear description of which transaction data will be accessed. Must specify the mobile money provider by name. | Retain derived features; raw transaction data deleted after scoring. |
| **Platform repayment history** | Terms of service consent | Covered by platform terms of service agreed at account creation. Customer can request deletion under right-to-erasure. | Retained for the duration of the customer relationship plus regulatory retention period. |
| **Credit bureau (CRB)** | Explicit opt-in (regulatory mandate) | Signed or digital consent form, often required by the CRB and/or central bank regulation. Must be obtained before any bureau inquiry. | Bureau data is not stored by the platform; only the resulting score and decision are retained. |
| **Telco tenure** | Bundled with CDR consent | Typically covered by the same consent that authorizes CDR data sharing. | Retained as a static attribute; refreshed on subsequent applications. |
| **Down payment** | Transactional | Implied by the customer's own action of making a payment. No additional consent required. | Retained as part of the loan record. |

### 4.3 Consent Flow

1. **Pre-application disclosure**: Before starting a credit application, the customer is presented with a clear, plain-language summary of which data sources will be consulted.
2. **Granular consent**: The customer can consent to or decline individual data sources (e.g., consent to mobile money data but decline CDR). Declining a data source may reduce the credit limit but does not necessarily prevent approval.
3. **Consent record**: The platform records the consent event -- what was consented to, when, and through which channel -- as an immutable audit record.
4. **Withdrawal**: Customers can withdraw consent for future data access at any time. Withdrawal does not retroactively affect scores already computed.

---

## 5. Integration Architecture -- MSISDN Adapter Pattern

All alternative data sources that are keyed on a mobile phone number (MSISDN) are accessed through a unified **MSISDN Adapter** pattern. This is an extension of the general adapter pattern used for ERP integrations, specialized for customer-centric data retrieval.

### 5.1 Why MSISDN as the Key

In markets where formal identity documents are inconsistent and national ID systems are incomplete, the mobile phone number (MSISDN) is the most reliable and universal customer identifier. It serves as the lookup key across CDR systems, mobile money platforms, telco subscriber databases, and in many markets, credit bureaus.

### 5.2 Architecture

```
Credit Scoring Engine
       |
       | requests customer attributes by MSISDN
       v
  DataSourceRegistry
       |
       | resolves configured data sources for the tenant
       v
  +---------------------+
  | MSISDN Adapter       |  (abstract interface)
  | Interface            |
  +---------------------+
       |         |         |          |            |
       v         v         v          v            v
   CDR       MobileMoney  CRB     TelcoTenure  Platform
   Adapter   Adapter      Adapter  Adapter      History
   (Telco    (M-Pesa,     (Trans-  (Telco       Adapter
    API /     Airtel       Union,   API)         (internal
    Aggreg.)  Money)       Metropol)              DB)
```

### 5.3 MSISDN Adapter Interface

```python
from abc import ABC, abstractmethod
from typing import Optional
from dataclasses import dataclass


@dataclass
class DataSourceResult:
    source_name: str
    msisdn: str
    attributes: dict        # key-value pairs of scoring attributes
    confidence: float       # 0.0 to 1.0, data quality confidence
    fetched_at: datetime
    consent_reference: str  # reference to the consent record


class MSISDNDataAdapter(ABC):
    """Abstract interface for all MSISDN-keyed data source integrations."""

    @abstractmethod
    def fetch_attributes(
        self, msisdn: str, consent_token: str, lookback_months: int = 6
    ) -> DataSourceResult:
        """
        Retrieve scoring attributes for a customer identified by MSISDN.

        Args:
            msisdn: Customer's phone number in E.164 format.
            consent_token: Reference to the customer's consent record.
            lookback_months: How many months of historical data to consider.

        Returns:
            DataSourceResult with extracted attributes.

        Raises:
            ConsentNotFoundError: If the consent token is invalid or expired.
            DataSourceUnavailableError: If the upstream source is unreachable.
        """
        ...

    @abstractmethod
    def health_check(self) -> bool:
        """Verify that the data source is reachable and authenticated."""
        ...

    @abstractmethod
    def supported_attributes(self) -> List[str]:
        """Return the list of attribute keys this adapter can provide."""
        ...
```

### 5.4 Data Source Registry

The `DataSourceRegistry` manages which data sources are configured and enabled for each tenant.

```python
class DataSourceRegistry:
    def __init__(self):
        self._adapters: dict[str, MSISDNDataAdapter] = {}
        self.sources_used: List[str] = []

    def register(self, source_name: str, adapter: MSISDNDataAdapter):
        self._adapters[source_name] = adapter

    def fetch_all(
        self, msisdn: str, consent_token: str, tenant_config: TenantDataConfig
    ) -> dict:
        """
        Fetch attributes from all enabled data sources for this tenant.
        Sources are queried in parallel. Failures in individual sources
        are logged but do not block other sources.
        """
        combined_attributes = {}
        self.sources_used = []

        for source_name in tenant_config.enabled_sources:
            adapter = self._adapters.get(source_name)
            if adapter is None:
                continue

            try:
                result = adapter.fetch_attributes(
                    msisdn, consent_token, tenant_config.lookback_months
                )
                combined_attributes.update(result.attributes)
                self.sources_used.append(source_name)
            except DataSourceUnavailableError:
                log.warning(f"Data source {source_name} unavailable for {msisdn}")
            except ConsentNotFoundError:
                log.warning(f"No consent for {source_name} for {msisdn}")

        return combined_attributes
```

### 5.5 Parallel Fetching

Data sources are fetched concurrently to minimize latency. The scoring engine sets a per-source timeout (typically 5 to 10 seconds) and proceeds with whatever data is available when the timeout expires. Missing data sources reduce the scoring resolution but do not block the evaluation.

```
Timeline:    0s -------- 2s -------- 4s -------- 6s -------- 8s
CDR:         |========|  (2.1s)
MobileMoney: |============|  (3.4s)
CRB:         |================|  (4.8s)
TelcoTenure: |====|  (1.2s)
Platform:    |==|  (0.5s)
                                    ^
                                    All sources returned by 4.8s
                                    Scoring proceeds immediately
```

If a source fails or times out, the engine evaluates the strategy's rules with the attributes that are available. Rules referencing missing attributes are skipped, and the `data_sources_used` field in the scoring result reflects which sources actually contributed.

---

## 6. Data Source Configuration Per Tenant

Each tenant configures which alternative data sources are enabled, reflecting their data-sharing agreements, market, and available integrations.

```json
{
  "tenant_id": "tn_safaricom_ke",
  "data_sources": {
    "enabled_sources": ["cdr", "mobile_money", "crb_metropol", "telco_tenure", "platform_history"],
    "lookback_months": 6,
    "source_configs": {
      "cdr": {
        "provider": "direct_telco",
        "api_endpoint": "https://api.internal.safaricom.co.ke/cdr/v2",
        "auth_type": "oauth2",
        "timeout_seconds": 5
      },
      "mobile_money": {
        "provider": "direct_telco",
        "platform": "mpesa",
        "api_endpoint": "https://api.internal.safaricom.co.ke/mpesa/history/v1",
        "auth_type": "oauth2",
        "timeout_seconds": 8
      },
      "crb_metropol": {
        "provider": "metropol_crb",
        "api_endpoint": "https://api.metropol.co.ke/v2",
        "auth_type": "api_key",
        "timeout_seconds": 10
      },
      "telco_tenure": {
        "provider": "direct_telco",
        "api_endpoint": "https://api.internal.safaricom.co.ke/subscriber/v1",
        "auth_type": "oauth2",
        "timeout_seconds": 3
      },
      "platform_history": {
        "provider": "internal",
        "timeout_seconds": 2
      }
    }
  }
}
```

---

## 7. Summary -- Data Source Comparison

| Data Source | Availability | Predictive Power | Cost | Consent Complexity | Best For |
|---|---|---|---|---|---|
| CDR | High (via telco partner) | Medium-High | Low (direct) / Medium (aggregator) | Explicit opt-in | First-time customers on partner network |
| Mobile money | High (Africa, SE Asia) | High | Low (direct) / Medium (aggregator) | Explicit opt-in | Underbanked with active mobile money usage |
| Platform history | Platform customers only | Very High | None | Terms of service | Repeat customers |
| Credit bureau | Low-Medium (market dependent) | High (where available) | Per-query fee | Regulatory mandate | Formally banked customers |
| Telco tenure | High (via telco partner) | Low-Medium | Negligible | Bundled with CDR | Supplementary signal |
| Down payment | Universal | Medium | None | Transactional | All customers; risk reduction mechanism |
