# IRI One-Time Withdrawal Quote API

> **NOTE:** This repository is for the working group to use while drafting documentation and specifications before they are formally ratified.  Users outside of the working group should not begin implementing solutions based on the content in this repository as it is likely to change without notice.

This document defines the **One-Time Withdrawal Quote** transaction as part of the broader IRI Digital First transformation effort. The transaction supports quote requests related to withdrawal eligibility, available amounts, tax considerations, and supporting data required prior to performing a withdrawal transaction (Partial Withdrawal, Full Surrender, or One-Time RMD).

The initiative modernizes legacy XML/SOAP-based In-Force Transactions (IFT) into RESTful APIs for secure, scalable, and interoperable processing. It leverages industry standards and provides a unified approach for carriers, distributors, and solution providers.

## Draft API Specifications

The working group's draft OpenAPI specification is in the [draft-api-specs](./draft-api-specs) directory.  See [README.md](draft-api-specs/README.md) for more details.

## Business Case

The working group's draft business case documentation is available in this repository. Once the documentation is finalized, it will be formally published on the [IRI DFA Library of Standards](https://www.irionline.org/member-programs/operations-technology/digital-first-library-standards/).

### Problem Statement
- Legacy quote processes depend on aging XML/SOAP interfaces that lack modern integration patterns.
- There is no standardized industry-wide quote format, leading to inconsistent integrations.
- Modern REST APIs enable real-time data access, reduced friction, and faster integration.

### Objectives
- Provide a consistent quote mechanism for all one-time withdrawal scenarios.
- Enable pre-transaction validation to reduce downstream failures.
- Improve integration velocity and support modern security standards.
- Deliver predictable JSON response structures aligned with Digital First architecture.

### Key Features
- RESTful API with JSON input/output.
- Unified request schema with correlationId and firm/participant identifiers.
- Standardized response structures: available amounts, charges, tax guidance, supporting documents.
- Alignment with IRI OpenAPI 3.1.x documentation.
- Conditional validation based on withdrawal type.
- Data Dictionary for field-level definitions and code lists.

---

## Supported Quotes Types
One-Time Withdrawal Quote supports eligibility and informational checks across:

### 1. Partial Withdrawal Eligibility quote
Used to determine:
- Available amounts
- Fund-level withdrawable values
- Pending transactions
- Charges or restrictions

### 2. Full Surrender Quote
Provides:
- Total surrender value
- Applicable charges
- Taxable implications
- Required forms or review indicators

### 3. One-Time RMD Quote
Determines:
- RMD tax year requirements
- RMD eligible amounts
- Prior year-end contract values
- RMD calculation methods supported by the carrier

---

## Schema Overview
The schema generally includes:

- **Root Attributes:** status, errors, policyNumber, effectiveDate, cusip, freeWithdrawalAmount, tenPercentOfPurchasePayments, freeAmountGain, freeAmountPremium
- **transactionAmounts:** appliedAmount, taxableAmount, totalChargeAmount, totalTaxWithheldAmount, netPaymentAmount, disbursementType
- **Parties:** individual/entity identity, relationships, paymentForm, allocationPercentage, bank, address
- **RMD Info Fields:** rmdTaxYear, rmdPriorYearEndValue, rmdSpouseDoB, rmdRequestedAmount, rmdAgeAtDistribution, rmdCalculationMethod
- **Charges & Fees:** chargeType, chargeCategory, isChargeWaived, chargeWaiverReason, chargeAmount, loanDeduction
- **Tax Qualification fields:** isTefraApplicable, isTemraApplicable, tefraValue, temraValue, isirc72qPenaltyApplied, taxablePortionRule (LIFO/FIFO)
- **PayeeOrBeneficiary:** taxId, paymentForm, allocationPercentage, disbursementAmount

Each quote response delivers structured information supporting the corresponding withdrawal workflow.

---

# Response Schema Overview

All API operations—across synchronous and asynchronous processing models—adhere to a unified **Error schema**. This ensures consistent error handling, predictable integration behavior, and standardized troubleshooting across all transaction types.

## Success Response Expectations

### Synchronous APIs
- Return **HTTP 200** for successful, completed processing.
---

## Standard Error Schema
Every error response—regardless of transaction type—includes:
- An HTTP status code in the **400–599** range
- A structured and validated **error code**
- A **timestamp** of when the error was generated
- A safe, user‑friendly **Message**
- A **field‑level** or **rule‑level** error collections

---

## Key Fields

| Field | Description |
|-------|-------------|
| **correlationId** | Carries forward the inbound request’s correlation ID header to enable end‑to‑end traceability. |
| **httpStatus** | Numeric HTTP status code (400–599) representing the type and severity of the failure. |
| **code** | Structured identifier in the enforced format: `domain.category.subcategory`. Enables machine‑readable error handling. |
| **message** | End‑user‑friendly explanation, safe to show in portals or consumer‑facing apps. |
| **validationErrors** | Array describing domain/business rule violations; each entry requires its own code and message. |

---

## Purpose & Benefits
This standardized error structure ensures:
- A **predictable experience** across all APIs (synchronous + asynchronous)
- Clear differentiation between **developer diagnostics** and **user‑safe messages**
- Enhanced **traceability** for carriers, distributors, and integrators
- Support for granular **validation feedback** and complex business rule logic
- Easier **monitoring, logging, and cross‑system troubleshooting**

---

## Example Payloads

Sample request/response payloads for each quote type, and the data dictionary, are in the [draft-api-specs](./draft-api-specs) directory. See [draft-api-specs/README.md](draft-api-specs/README.md) for the current draft versions:



---

## Business Owners
- **Carrier Business Owner:** digitalfirst@brighthousefinancial.com
- **Distributor Business Owner:** [contact]
- **Solution Provider Business Owner:** [contact]

## How to engage, contribute, and give feedback
- Please contact the business owners or IRI (hpikus@irionline.org) to get added to the working group discussions.

## Change submissions and reporting issues and bugs

Security issues and bugs should be reported directly to Katherine Dease kdease@irionline.org. Issues and bugs can be reported directly within the issues tab of a repository. Change requests should follow the standards governance workflow outlined on the [main page](https://github.com/Insured-Retirement-Institute).

## Code of conduct

See the [Digital-First-Specifications](https://github.com/Insured-Retirement-Institute/Digital-First-Specifications) repository
