
# IRI One-Time Withdrawal Inquiry API

## Overview
This document defines the **One-Time Withdrawal Inquiry** transaction as part of the broader IRI Digital First transformation effort. The transaction supports inquiry requests related to withdrawal eligibility, available amounts, tax considerations, and supporting data required prior to performing a withdrawal transaction (Partial Withdrawal, Full Surrender, or One-Time RMD).

The initiative modernizes legacy XML/SOAP-based In-Force Transactions (IFT) into RESTful APIs for secure, scalable, and interoperable processing. It leverages industry standards and provides a unified approach for carriers, distributors, and solution providers.

---

## Business Case
### Problem Statement
- Legacy inquiry processes depend on aging XML/SOAP interfaces that lack modern integration patterns.
- There is no standardized industry-wide inquiry format, leading to inconsistent integrations.
- Modern REST APIs enable real-time data access, reduced friction, and faster integration.

### Objectives
- Provide a consistent inquiry mechanism for all one-time withdrawal scenarios.
- Enable pre-transaction validation to reduce downstream failures.
- Improve integration velocity and support modern security standards.
- Deliver predictable JSON response structures aligned with Digital First architecture.

### Key Features
- RESTful API with JSON input/output.
- Unified request schema with correlationId and firm/participant identifiers.
- Standardized response structures: available amounts, charges, tax guidance, supporting documents.
- Alignment with IRI OpenAPI 3.x documentation.
- Conditional validation based on withdrawal type.
- Data Dictionary for field-level definitions and code lists.

---

## Supported Inquiry Types
One-Time Withdrawal Inquiry supports eligibility and informational checks across:

### 1. Partial Withdrawal Eligibility Inquiry
Used to determine:
- Available amounts
- Fund-level withdrawable values
- Pending transactions
- Charges or restrictions

### 2. Full Surrender Inquiry
Provides:
- Total surrender value
- Applicable charges
- Taxable implications
- Required forms or review indicators

### 3. One-Time RMD Inquiry
Determines:
- RMD tax year requirements
- RMD eligible amounts
- Prior year-end contract values
- RMD calculation methods supported by the carrier

---

## Schema Overview
The schema generally includes:

- **Root Attributes:** `correlationId`, `effectiveDate`, `associatedFirmId`, `nsccParticipantId`, `inquiryType`
- **Contract Details:** contractNumber, productType, lineOfBusiness
- **Producer Info:** producerNumber, npn, crdNumber
- **Requested Information:** inquiryCategory (AMOUNT, ELIGIBILITY, TAX, ALL)
- **Parties:** parties[]
- **Fund Details (if applicable):** fundId, fundName, balance, restrictions
- **RMD Inquiry Fields:** rmdTaxYear, rmdPriorYearEndValue, spouseDateOfBirth (optional)
- **Charges & Fees:** chargeTypes, waiverIndicators, explanations

Each inquiry response delivers structured information supporting the corresponding withdrawal workflow.

---

# Error Schema Overview

All API operations—across synchronous and asynchronous processing models—adhere to a unified **Error schema**. This ensures consistent error handling, predictable integration behavior, and standardized troubleshooting across all transaction types.

## Success Response Expectations

### Synchronous APIs
- Return **HTTP 200** for successful, completed processing.

### Asynchronous APIs
- Return **HTTP 202** to acknowledge the request has been accepted for processing but not yet completed.

---

## Standard Error Schema
Every error response—regardless of transaction type—includes:
- An HTTP status code in the **400–599** range
- A structured and validated **error code**
- A **timestamp** of when the error was generated
- A developer‑focused **technical message** (`message`)
- A safe, user‑friendly **userMessage**
- A **correlationId** for cross‑system tracing
- Optional **field‑level** or **rule‑level** error collections

---

## Key Fields

| Field | Description |
|-------|-------------|
| **httpStatus** | Numeric HTTP status code (400–599) representing the type and severity of the failure. |
| **code** | Structured identifier in the enforced format: `domain.category.subcategory`. Enables machine‑readable error handling. |
| **message** | Technical diagnostic detail for developers, logs, or support teams. |
| **userMessage** | End‑user‑friendly explanation, safe to show in portals or consumer‑facing apps. |
| **correlationId** | Carries forward the inbound request’s correlation ID header to enable end‑to‑end traceability. |
| **validationErrors** (optional) | Array describing field‑level validation issues (e.g., missing required fields, incorrect formats). |
| **businessErrors** (optional) | Array describing domain/business rule violations; each entry requires its own code and message. |

---

## Purpose & Benefits
This standardized error structure ensures:
- A **predictable experience** across all APIs (synchronous + asynchronous)
- Clear differentiation between **developer diagnostics** and **user‑safe messages**
- Enhanced **traceability** for carriers, distributors, and integrators
- Support for granular **validation feedback** and complex business rule logic
- Easier **monitoring, logging, and cross‑system troubleshooting**

---

## OpenAPI Specs
Unified Swagger documentation for this inquiry endpoint is available in the `openapi-specs/` folder.

---

## Change Submissions and Reporting Issues
- Use the **Issues** tab of the repository to report bugs or enhancement requests.
- **Security issues** should be reported directly to Katherine Dease at **kdease@irionline.org**.
- Follow the standards governance workflow on the main page for contribution guidelines.

---

## Versioning
- Uses semantic versioning for all updates.
- Changes must be documented through commit messages and changelogs.

---

## Code of Conduct
Refer to the repository’s **Code of Conduct** and **Style Guide** for contribution standards.

---

## How to Contribute
- Fork the repository and submit pull requests.
- Report issues using the **Issues** tab.
- Join working groups: **hpikus@irionline.org**.

---

## Business Owners
- **Carrier Business Owner:** digitalfirst@brighthousefinancial.com
- **Distributor Business Owner:** [contact]
- **Solution Provider Business Owner:** [contact]
