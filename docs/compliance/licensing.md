# Licensing Requirements

## 1. Overview

Operating a mobile device lending platform across African markets requires appropriate licensing in each jurisdiction. The licensing landscape for digital lending is evolving rapidly, with regulators increasingly defining specific requirements for technology platforms, digital credit providers, and microfinance institutions.

This document outlines the licensing requirements per market, the distinction between platform and financer licensing responsibilities, relevant digital lending regulations, regulatory sandbox options, and ongoing compliance monitoring obligations.

### Licensing Principles

| Principle | Application |
|---|---|
| **License Before Launch** | No lending operations commence in any market until the required licenses are obtained or a valid exemption/sandbox arrangement is in place |
| **Platform vs. Financer Separation** | The licensing model clearly distinguishes between the platform operator (IInovi as a technology provider) and the financing partners (tenants) who hold lending licenses |
| **Continuous Compliance** | Licensing is not a one-time event; the platform supports ongoing regulatory reporting, capital adequacy monitoring, and license renewal obligations |
| **Regulatory Engagement** | Proactive engagement with regulators in each market to ensure alignment with evolving requirements |
| **Market-Specific Compliance** | Each market's specific requirements are met independently; compliance in one market does not substitute for another |

---

## 2. Platform vs. Financer Licensing Model

### 2.1 Licensing Model Overview

The IInovi platform operates on a multi-tenant model where the platform provides the technology infrastructure and the financing partners (tenants) are the entities that extend credit to customers. This creates a two-tier licensing consideration:

| Entity | Role | Licensing Responsibility |
|---|---|---|
| **IInovi (Platform Operator)** | Provides the technology platform, manages integrations, processes data, and facilitates lending operations | Technology service provider; may require specific licensing depending on the jurisdiction and the scope of services provided |
| **Financing Partner (Tenant)** | The legal entity that extends credit to customers, bears the credit risk, and holds the customer relationship | Must hold the appropriate lending/microfinance/digital credit provider license in each market where it operates |

### 2.2 Platform Operator Considerations

The platform operator's licensing exposure depends on how regulators in each market classify the services provided:

| Classification | Description | Licensing Implication |
|---|---|---|
| **Pure technology provider** | Platform provides software infrastructure only; financing partner makes all credit decisions | Minimal direct licensing; may require registration as a technology service provider to regulated entities |
| **Credit intermediary** | Platform participates in the credit decision process (e.g., automated credit scoring, approval recommendations) | May require credit intermediary or agent license in some jurisdictions |
| **Payment service provider** | Platform processes or facilitates payment transactions (mobile money integration) | May require payment service provider license or operate under the financing partner's license |
| **Data processor** | Platform processes personal data on behalf of the financing partner | Data protection registration required; see [Data Privacy and Consent](data-privacy-consent.md) |
| **Credit information provider** | Platform submits data to credit bureaus | May require registration as a credit information provider |

### 2.3 Financer Licensing Requirements

Each financing partner operating on the platform must independently hold the required licenses for their lending operations. The platform verifies licensing status during partner onboarding and monitors for license renewals and regulatory actions.

| Verification | Method | Frequency |
|---|---|---|
| **License validity** | Copy of license verified against regulator's public register | At onboarding + annually |
| **License scope** | Verification that the license covers the intended products (micro-lending, device financing, digital credit) | At onboarding + on product change |
| **License conditions** | Review of any special conditions attached to the license | At onboarding + annually |
| **Regulatory standing** | Check for regulatory actions, sanctions, or warnings against the partner | At onboarding + quarterly |

---

## 3. Per-Market Licensing Requirements

### 3.1 Kenya

| Aspect | Details |
|---|---|
| **Primary Regulation** | Central Bank of Kenya (CBK) Act; Digital Credit Providers Regulations, 2022 |
| **Regulatory Authority** | Central Bank of Kenya (CBK) |
| **License Type Required** | **Digital Credit Provider (DCP) License** for entities offering digital credit (loans originated, approved, or disbursed through digital channels) |
| **Applicability** | Any entity that offers credit through a digital platform, including device financing via mobile app or USSD |
| **Key Requirements** | Minimum capital requirements; fit and proper test for directors and officers; audited financial statements; business plan demonstrating viability; consumer protection policies; data protection compliance; CRB integration |
| **Processing Time** | 90 days from complete application submission (per CBK guidelines) |
| **Annual Fees** | As prescribed by CBK (varies by category) |
| **Ongoing Obligations** | Monthly returns to CBK; annual audited financial statements; quarterly risk reports; consumer complaints reports |

**Additional Considerations for Kenya:**

- The Digital Credit Providers Regulations, 2022 specifically target entities offering digital credit, including BNPL arrangements.
- The platform operator may fall under the DCP definition if it participates in credit origination or decisioning.
- A financing partner already licensed as a bank, microfinance bank, or SACCO may not require a separate DCP license for device financing, but must notify CBK of the new product line.

### 3.2 Nigeria

| Aspect | Details |
|---|---|
| **Primary Regulation** | CBN Revised Microfinance Policy, 2024; FCCPC Limited Interim Regulatory/Registration Framework for Digital Lending, 2022 |
| **Regulatory Authorities** | Central Bank of Nigeria (CBN); Federal Competition and Consumer Protection Commission (FCCPC) |
| **License Type Required** | **Microfinance Bank License** (state or national), **Finance Company License**, or **FCCPC Digital Lending Registration** depending on scale and structure |
| **Applicability** | Entities providing micro-loans, consumer credit, or digital lending services |
| **Key Requirements (FCCPC Registration)** | Company registration with CAC; disclosure of lending products; APR disclosure; complaints resolution mechanism; data protection compliance; prohibition of abusive collection practices |
| **Key Requirements (CBN Microfinance License)** | Minimum capital of NGN 200 million (state) or NGN 5 billion (national); fit and proper assessment; business plan; AML/CFT compliance framework |
| **Processing Time** | FCCPC registration: 30-60 days; CBN license: 6-12 months |
| **Ongoing Obligations** | Periodic returns to CBN/FCCPC; annual audited financial statements; consumer protection compliance reporting |

**Additional Considerations for Nigeria:**

- The FCCPC has been actively regulating digital lenders since 2022, requiring registration and compliance with consumer protection standards.
- Unlicensed digital lending operations have faced enforcement actions, including app store removal and penalties.
- The CBN is developing a comprehensive Digital Lending Framework that may impose additional requirements.
- The platform operator should register with FCCPC as a technology provider to digital lending companies.

### 3.3 South Africa

| Aspect | Details |
|---|---|
| **Primary Regulation** | National Credit Act (NCA), 2005 (as amended); National Credit Regulations |
| **Regulatory Authority** | National Credit Regulator (NCR) |
| **License Type Required** | **Credit Provider Registration** under the NCA |
| **Applicability** | Any person or entity that enters into a credit agreement with a consumer, including instalment sale agreements and credit transactions |
| **Key Requirements** | Registration with NCR; compliance with affordability assessment requirements; prescribed maximum interest rates (per NCA Regulations, Table A); mandatory pre-agreement disclosure; cooling-off period compliance; reckless lending provisions; complaints resolution mechanism |
| **Registration Fee** | As prescribed by NCR (based on loan book size) |
| **Processing Time** | 60-90 business days from complete application |
| **Ongoing Obligations** | Annual registration renewal; quarterly statistical returns (Form 39); annual compliance report; annual financial statements; consumer complaints reports |

**Additional Considerations for South Africa:**

- Device financing through instalment agreements falls squarely within the NCA's definition of "credit agreement."
- The NCA's affordability assessment requirements are particularly stringent and must be fully implemented.
- The platform operator may need to register as a credit bureau user (for CRB access) and as a payment system participant.
- South Africa's regulatory environment is the most mature among the target markets, with well-established enforcement mechanisms.
- The NCR conducts regular compliance inspections; the platform must support on-site examination readiness.

### 3.4 Ghana

| Aspect | Details |
|---|---|
| **Primary Regulation** | Banks and Specialised Deposit-Taking Institutions Act, 2016 (Act 930); Borrowers and Lenders Act, 2020 (Act 1052); Non-Bank Financial Institutions Act, 2008 (Act 774) |
| **Regulatory Authority** | Bank of Ghana (BoG) |
| **License Type Required** | **Money Lending License** under the Non-Bank Financial Institutions framework, or **Microfinance License** depending on structure and scale |
| **Applicability** | Entities providing lending services, including device financing and BNPL arrangements |
| **Key Requirements** | Minimum capital requirements (as prescribed by BoG); fit and proper assessment of directors; business plan; AML/CFT compliance framework; KYC (Ghana Card mandatory); consumer protection policies |
| **Processing Time** | 3-6 months from complete application |
| **Ongoing Obligations** | Monthly returns to BoG; annual audited financial statements; prudential ratio compliance; KYC/AML compliance reporting |

**Additional Considerations for Ghana:**

- The Bank of Ghana has been tightening regulation of non-bank financial institutions, including microfinance and money lending entities.
- The mandatory use of the Ghana Card for customer verification is a unique requirement that the platform must fully support.
- BoG periodically issues directives that may affect lending operations; the platform must monitor and adapt to these directives.

---

## 4. Digital Lending Regulations

### 4.1 Kenya: Digital Credit Providers Regulations, 2022

| Provision | Requirement | Platform Impact |
|---|---|---|
| **Pricing transparency** | Must disclose APR, total cost of credit, all fees before disbursement | Pre-contractual disclosure screen required |
| **Unsolicited credit prohibition** | Cannot disburse credit without customer request | System prevents auto-disbursement |
| **Data access** | Cannot access customer contact lists, photos, or other private data without explicit consent | Telco data access restricted to consented categories |
| **Collection practices** | Prohibition on aggressive recovery; no contact of third parties; restricted hours | Dunning rules enforced by platform (see [Consumer Protection](consumer-protection.md)) |
| **CRB reporting** | Mandatory CRB query and reporting | CRB integration required (see [Credit Bureau Integration](credit-bureau-integration.md)) |
| **Complaints mechanism** | Must provide accessible complaints channel | In-app and USSD complaints flow |

### 4.2 Nigeria: FCCPC Guidelines for Digital Lending

| Provision | Requirement | Platform Impact |
|---|---|---|
| **Registration** | All digital lenders must register with FCCPC | Financing partners must provide registration evidence |
| **APR disclosure** | APR must be disclosed prominently before loan acceptance | APR calculated and displayed on all origination channels |
| **Data privacy** | Prohibition on accessing phone contacts, media, or other private data; compliance with NDPA | App permissions limited to necessary functions |
| **Recovery practices** | Prohibition on public shaming, harassment, or contact of third parties for debt recovery | Collection scripts reviewed for compliance |
| **Pricing limits** | FCCPC monitors for exploitative pricing; no statutory cap but regulatory scrutiny | APR benchmarking and monitoring |

### 4.3 South Africa: National Credit Act

| Provision | Requirement | Platform Impact |
|---|---|---|
| **Credit provider registration** | Must be registered with NCR | Financing partner must provide NCR registration number |
| **Affordability assessment** | Must assess customer's ability to repay (income, expenses, existing debt) | Affordability assessment module in credit scoring |
| **Maximum interest rates** | Prescribed rates per credit type (NCA Regulations, Table A) | Product configuration enforces maximum rate per agreement type |
| **Pre-agreement statement** | Must provide prescribed pre-agreement statement | Generated and presented before contract |
| **Quotation** | Must provide binding quotation valid for 5 days | System generates and stores quotation with expiry |
| **Cooling-off period** | 5 business days to cancel without penalty | Cooling-off workflow implemented (see [Consumer Protection](consumer-protection.md)) |
| **Reckless lending** | Prohibition on lending if customer cannot afford or is over-indebted | Enforced through affordability gate and CRB check |
| **Debt counselling** | Must cooperate with debt counsellors | Process for receiving and acting on debt counselling notifications |

### 4.4 Ghana: Borrowers and Lenders Act, 2020 (Act 1052)

| Provision | Requirement | Platform Impact |
|---|---|---|
| **Registration** | Lenders must be registered under the Act | Financing partners must provide registration evidence |
| **Disclosure** | Must disclose all terms, including total cost of credit | Pre-contractual disclosure implemented |
| **Fair lending** | Prohibition on unfair credit practices | Lending terms validated against regulatory requirements |
| **Borrower rights** | Right to information, early settlement, complaints resolution | Supported through platform features |
| **Ghana Card KYC** | Mandatory Ghana Card for customer verification | KYC flow includes Ghana Card capture and NIA verification |

---

## 5. Regulatory Sandbox Options

### 5.1 Available Sandboxes

Several target markets offer regulatory sandbox programs that allow fintech companies to test innovative products under a controlled regulatory environment:

| Market | Sandbox Program | Regulator | Duration | Scope |
|---|---|---|---|---|
| **Kenya** | CBK Regulatory Sandbox | Central Bank of Kenya | Up to 24 months | Payment systems, digital credit, insurtech |
| **Nigeria** | CBN Regulatory Sandbox | Central Bank of Nigeria | Up to 12 months (extendable) | Payment systems, lending, banking services |
| **South Africa** | IFWG Regulatory Sandbox | Intergovernmental Fintech Working Group (SARB, FSCA, FIC, NCR) | Variable (case-by-case) | Broad fintech innovation |
| **Ghana** | BoG Regulatory Sandbox | Bank of Ghana | Up to 24 months | Payment systems, digital financial services |

### 5.2 Sandbox Considerations for IInovi

| Consideration | Assessment |
|---|---|
| **When to use** | Sandbox is appropriate for initial market entry when the licensing pathway is unclear, or when testing a novel product structure that does not fit neatly into existing license categories |
| **Limitations** | Sandbox typically limits customer numbers, transaction volumes, and geographic scope; not suitable for full-scale operations |
| **Exit strategy** | A clear path from sandbox to full licensing must be defined before entry; sandbox participation does not guarantee license issuance |
| **Reporting** | Sandbox participants must provide regular progress reports, risk assessments, and customer impact analyses to the regulator |
| **Customer protection** | Sandbox participants must comply with consumer protection requirements from day one; sandbox status does not exempt from fair treatment obligations |

### 5.3 Sandbox Application Process

| Step | Description | Timeline |
|---|---|---|
| **1. Expression of Interest** | Submit initial application describing the innovation, target market, and regulatory questions | 1-2 weeks |
| **2. Assessment** | Regulator evaluates eligibility, innovation merit, and consumer impact | 4-8 weeks |
| **3. Sandbox Agreement** | If accepted, negotiate sandbox terms (scope, duration, reporting, exit criteria) | 2-4 weeks |
| **4. Testing Phase** | Operate within sandbox parameters; report to regulator as agreed | 6-24 months |
| **5. Evaluation** | Regulator evaluates outcomes against sandbox objectives | 4-8 weeks |
| **6. Exit** | Transition to full license, modify product to fit existing framework, or cease operations | Varies |

---

## 6. Compliance Monitoring and Reporting

### 6.1 Ongoing Regulatory Reporting

The platform supports the generation and submission of regulatory reports required by each market's supervisory authority:

| Report | Markets | Frequency | Content |
|---|---|---|---|
| **Portfolio statistical return** | All markets | Monthly / Quarterly (varies) | Loan volumes, outstanding balances, disbursements, collections, arrears aging, write-offs |
| **Prudential ratios** | SA, Kenya, Ghana | Monthly / Quarterly | Capital adequacy, liquidity ratios, provisioning levels |
| **Consumer complaints report** | All markets | Quarterly / Annually | Complaints volume, categories, resolution times, outcomes |
| **AML/CFT returns** | All markets | Quarterly / Annually | STR filings, CTR filings, sanctions screening results, KYC completion rates |
| **CRB compliance report** | All markets | Monthly | CRB submission completeness, dispute volumes, correction rates |
| **Annual financial statements** | All markets | Annually | Audited financial statements per local GAAP or IFRS |
| **NCA Form 39 (SA specific)** | South Africa | Quarterly | Prescribed statistical return for registered credit providers |

### 6.2 Regulatory Change Monitoring

The platform compliance team monitors regulatory developments across all target markets:

| Activity | Method | Frequency |
|---|---|---|
| **Regulatory gazette monitoring** | Subscription to official gazettes and regulatory announcements | Weekly |
| **Industry association membership** | Membership in relevant industry bodies (e.g., Digital Lenders Association of Kenya, Fintech Association of Nigeria) | Ongoing |
| **Regulatory engagement** | Direct engagement with regulators through industry consultations and stakeholder forums | As scheduled |
| **Legal advisory** | Retainer with local legal counsel in each market for regulatory interpretation | Ongoing |
| **Platform impact assessment** | When a regulatory change is identified, assess impact on platform features and operations | Within 5 business days of identification |

### 6.3 License Renewal and Maintenance

| Activity | Responsibility | Timeline |
|---|---|---|
| **License renewal application** | Financing partner (with platform support for data and reports) | 60-90 days before expiry |
| **Annual compliance self-assessment** | Platform compliance team + financing partner | Annually |
| **Regulatory examination preparation** | Platform compliance team + financing partner | Ongoing readiness; intensified when examination announced |
| **Capital adequacy monitoring** | Financing partner; platform provides portfolio data | Monthly |
| **Condition compliance monitoring** | Platform compliance team | Continuous |

---

## 7. License Application Process

### 7.1 General Application Steps

The following general steps apply across all markets, with market-specific variations:

| Step | Description | Typical Duration |
|---|---|---|
| **1. Legal entity establishment** | Incorporate a local entity (or register a foreign entity) in the target market | 2-8 weeks |
| **2. Legal and regulatory assessment** | Engage local counsel to determine the specific license required and assess eligibility | 2-4 weeks |
| **3. Application preparation** | Prepare all required documentation (business plan, policies, procedures, financial projections, fit-and-proper assessments) | 4-8 weeks |
| **4. Capital deployment** | Deposit the required minimum capital in a local bank account | 1-2 weeks |
| **5. Application submission** | Submit the complete application to the regulatory authority | 1 week |
| **6. Regulatory review** | Regulator reviews the application, may request additional information or conduct interviews | 3-12 months (varies significantly) |
| **7. Conditional approval** | Regulator may issue conditional approval with specific requirements to be met before final license | 2-4 weeks to meet conditions |
| **8. License issuance** | Final license issued; operations may commence | Upon condition fulfillment |
| **9. Post-license obligations** | Commence regulatory reporting; undergo initial examination (if required) | Ongoing from license date |

### 7.2 Application Documentation Checklist

| Document | Description |
|---|---|
| **Certificate of incorporation** | Proof of legal entity in the target jurisdiction |
| **Memorandum and Articles of Association** | Company constitution reflecting lending activities |
| **Business plan** | 3-5 year business plan including market analysis, product description, growth projections, risk management framework |
| **Financial projections** | Detailed financial projections demonstrating viability and capital adequacy |
| **Fit and proper declarations** | Directors and senior officers' CVs, police clearance certificates, and fit-and-proper questionnaires |
| **Shareholding structure** | Details of all shareholders (direct and indirect); source of capital documentation |
| **AML/CFT policy** | Anti-money laundering and counter-terrorism financing policy (see [KYC/AML](kyc-aml.md)) |
| **Consumer protection policy** | Policy covering disclosure, complaints handling, fair collections (see [Consumer Protection](consumer-protection.md)) |
| **Data protection policy** | Data protection framework and DPIA (see [Data Privacy and Consent](data-privacy-consent.md)) |
| **IT and cybersecurity policy** | Technology infrastructure, security architecture, and business continuity plan (see [Security Architecture](../architecture/security.md)) |
| **Risk management framework** | Credit risk, operational risk, and market risk management policies |
| **Compliance manual** | Internal compliance procedures, reporting obligations, regulatory examination readiness |
| **Product terms and conditions** | Draft loan agreements and product terms sheets |
| **Audited financial statements** | If the entity has been operating in any capacity; or audited statements of the parent company |
| **Capital proof** | Bank statement or auditor's confirmation of minimum capital deposit |

### 7.3 Estimated Timelines by Market

| Market | Minimum Timeline (from application to license) | Typical Timeline | Key Dependencies |
|---|---|---|---|
| **Kenya** | 3 months | 6-9 months | CBK processing capacity; completeness of application |
| **Nigeria** | 1 month (FCCPC) / 6 months (CBN) | 2 months (FCCPC) / 9-12 months (CBN) | License type; regulatory engagement |
| **South Africa** | 2 months | 3-6 months | NCR processing; completeness of application |
| **Ghana** | 3 months | 6-9 months | BoG processing capacity; capital requirements |

---

## 8. Platform Compliance Infrastructure

### 8.1 Regulatory Compliance Dashboard

The platform provides a compliance dashboard for monitoring licensing and regulatory obligations:

| Dashboard Component | Purpose |
|---|---|
| **License tracker** | Displays license status, expiry dates, and renewal deadlines for each financing partner per market |
| **Regulatory reporting calendar** | Shows upcoming reporting deadlines with submission status |
| **Regulatory change feed** | Aggregates regulatory announcements and assessed impacts |
| **Compliance task manager** | Tracks compliance action items, owners, and deadlines |
| **Examination readiness score** | Self-assessment score based on documentation completeness, report timeliness, and policy currency |

### 8.2 Regulatory Reporting Engine

The platform includes a reporting engine that generates regulatory returns in the formats required by each jurisdiction:

| Capability | Description |
|---|---|
| **Template management** | Regulatory report templates are maintained and updated per regulator specifications |
| **Automated data aggregation** | Report data is aggregated from the portfolio, payment, and customer services automatically |
| **Validation rules** | Built-in validation ensures data consistency and completeness before submission |
| **Audit trail** | Every generated report is versioned and stored with a complete audit trail |
| **Multi-format export** | Reports can be exported in PDF, Excel, CSV, or XML as required by the regulator |
| **Submission tracking** | Records submission date, confirmation receipt, and any regulator feedback |

---

## Related Documents

- [KYC/AML Compliance](kyc-aml.md) -- identity verification and anti-money laundering controls
- [Consumer Protection Compliance](consumer-protection.md) -- transparent disclosure and fair treatment obligations
- [Credit Bureau Integration](credit-bureau-integration.md) -- CRB access and reporting requirements
- [Data Privacy and Consent Management](data-privacy-consent.md) -- data protection and consent frameworks
- [Security Architecture](../architecture/security.md) -- IT security infrastructure
- [Documentation Index](../README.md) -- full documentation map
