# Enterprise ERP Integration Technical Playbook & Policy Guide

**Document ID:** EIP-POL-2026-V3.4  
**Effective Date:** October 2026  
**Security Classification:** Enterprise Implementation Standard  
**Governing Persona:** Principal Enterprise Solutions Architecture Group (Marcus Vance)  
**Applicability:** All Client Technical Onboarding, API Integrations, and Data Migrations (Acumatica, NetSuite, SAP, Dynamics 365)  

---

## 1. Authentication & API Governance Standards

### 1.1 Supported OAuth 2.0 Grant Architectures
Enterprise ERP integrations must enforce modern OAuth 2.0 authentication. The platform strictly supports two grant workflows:
* **`client_credentials` Grant (Machine-to-Machine Integration):** Required for all automated middleware, background synchronization workers, and asynchronous daemon tasks. Clients authenticate using `client_id` and `client_secret` against the tenant authorization server.
* **`authorization_code` Grant with PKCE:** Mandatory for all client-facing interactive portals, third-party dashboards, and user-delegated integrations. Pure implicit grants (`response_type=token`) are strictly deprecated and rejected by the API gateway.

### 1.2 Token Lifecycle & Rate Limiting Thresholds
* **Bearer Token Expiration:** Access tokens carry a strict Time-To-Live (TTL) of **3,600 seconds (60 minutes)**. Applications must implement proactive token refresh at 80% of TTL (at minute 48) rather than waiting for an `HTTP 401 Unauthorized` exception.
* **Refresh Token Rotation:** Refresh tokens expire after **30 days** of inactivity or immediately upon one-time usage when rotation is enforced.
* **Global Rate Limiting:** The API gateway enforces a strict ceiling of **120 requests per minute (RPM)** per authenticated `client_id`. Bursts exceeding this threshold will receive an `HTTP 429 Too Many Requests` status.
* **Exponential Backoff Requirement:** When an `HTTP 429` is encountered, client retry middleware must implement exponential backoff with jitter starting at an initial backoff interval of **2.5 seconds**, doubling up to a maximum ceiling of **60 seconds**.

### 1.3 Zero-Credential Security Mandate
* Under no circumstances may client teams transmit, log, or paste plaintext API secrets, production passwords, or database connection strings into conversational intake forms or ticket logs.
* All sandbox and production configuration files must inject credentials dynamically from environment key vaults via synthetic variables (e.g., `<CLIENT_API_SECRET>`).

---

## 2. Financial Chart of Accounts (COA) Schema Specifications

### 2.1 Account Numbering & Segment Classification Hierarchy
All General Ledger Chart of Accounts imports must adhere to the standardized 4-tier decimal numbering hierarchy:

| Account Number Range | Financial Statement Classification | Balance Normalcy | Rollup Parent Requirement |
|---|---|---|---|
| **`1000` – `1999`** | **Asset** (Current & Non-Current) | Debit Normal | Mandatory Parent for sub-accounts |
| **`2000` – `2999`** | **Liability** (Current & Long-Term) | Credit Normal | Mandatory Parent for sub-accounts |
| **`3000` – `3999`** | **Equity** (Retained Earnings & Capital) | Credit Normal | Mandatory Parent for sub-accounts |
| **`4000` – `4999`** | **Revenue / Operating Income** | Credit Normal | Mandatory Parent for sub-accounts |
| **`5000` – `9999`** | **Expense** (COGS & Operating Expenses) | Debit Normal | Mandatory Parent for sub-accounts |

### 2.2 Mandatory CSV Migration Header Columns
Client migration files for Chart of Accounts must contain the following six mandatory header columns in exact casing:
1. `AccountCode` (Alphanumeric, max 10 chars, unique primary key, e.g., `1010`)
2. `AccountName` (Text, max 60 chars, e.g., `Operating Checking Account`)
3. `AccountClass` (Exact Enum: `Asset`, `Liability`, `Equity`, `Revenue`, `Expense`)
4. `ActiveStatus` (Boolean integer: `1` for Active, `0` for Inactive)
5. `BaseCurrency` (ISO-4217 3-letter currency code, e.g., `USD`, `EUR`, `CAD`)
6. `ParentAccountID` (Alphanumeric, matches existing `AccountCode` or left null for top-level headers)

### 2.3 Opening Balance Balancing Constraint
* When uploading opening trial balances, the mathematical sum of all Debit entries minus Credit entries across all accounts must equal **exactly 0.00**. Imports with an out-of-balance variance exceeding **$0.00** are rejected during Stage 3 data ingestion.

---

## 3. Webhook Architecture & Event Notification Protocol

### 3.1 Cryptographic Signature Verification
To guarantee payload integrity and authenticity, all outbound ERP webhooks transmit an HMAC-SHA256 signature in the request header:
* **Header Key:** `X-Hub-Signature-256`
* **Format:** `sha256=<HEX_DIGEST>`
* **Signature Generation:** Computed by hashing the raw UTF-8 request body bytes with the pre-shared endpoint secret:
  $$\text{Signature} = \text{HMAC-SHA256}(\text{SharedSecret}, \text{RawBody})$$
* **Verification Mandate:** The client receiver must validate signatures using constant-time comparison algorithms (e.g., `crypto.timingSafeEqual`) to eliminate timing attack vulnerabilities.

### 3.2 Replay Attack Mitigation & Timestamp Headers
* **Header Key:** `X-Webhook-Timestamp`
* **Format:** Unix Epoch timestamp in milliseconds (e.g., `1728400000000`).
* **Tolerance Window:** The client receiving endpoint must compare `X-Webhook-Timestamp` against its current UTC server clock. Payloads with a timestamp variance exceeding **300 seconds (5 minutes)** must be discarded immediately as potential replay attacks.

### 3.3 Idempotency & Deduplication Standards
* **Header Key:** `X-Event-ID` (UUIDv4 format).
* **Deduplication Window:** Client receiving services must cache processed `X-Event-ID` values in an in-memory key-value store (e.g., Redis) with a minimum Time-To-Live (TTL) of **86,400 seconds (24 hours)**. Duplicate event IDs received within 24 hours must return `HTTP 200 OK` without re-executing business transactions.

---

## 4. Batch Ingestion Limits & Operational SLAs

### 4.1 Payload Size & Chunking Thresholds
* **Maximum Single Request Payload:** **25 Megabytes (MB)**.
* **Maximum Record Count Per Batch:** **15,000 records** per batch import job. Files exceeding 15,000 records must be partitioned into sequential chunk batches.
* **Timeout Threshold:** Synchronous API requests timeout after **45 seconds**. Batch jobs exceeding 45 seconds must use asynchronous bulk ingestion endpoints (`/api/v2/bulk/jobs`).

### 4.2 Support Triage Response SLAs
* **Severity 1 (Sandbox Outage / Gateway Failure):** Initial triage within **2 business hours**.
* **Severity 2 (Schema Validation Blockers):** Resolution guidance within **8 business hours**.
* **Severity 3 (General Inquiries & Formatting Advice):** Response within **24 business hours**.

---

## 5. Architectural Handover Sign-Off Requirements (Milestone M5)

Before a client implementation can be signed off by the Senior Solution Architect for production deployment, the following 4 prerequisites must be certified:
1. **Sandbox Connectivity Report:** 100% successful Bearer token generation via `client_credentials` grant with zero auth failures over 48 hours.
2. **Schema Ingestion Certification:** Successful dry-run import of Chart of Accounts and Customer Master with 0 syntax or balancing errors.
3. **Webhook Verification Ping:** Successful receipt and HMAC validation of at least 3 test webhook events with verified `< 300` second timestamp latency.
4. **Scope Security Sign-Off:** Signed OAuth scope matrix confirming the integration requests only the minimum necessary privileges (`least privilege principle`).
