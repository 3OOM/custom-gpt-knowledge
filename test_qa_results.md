# Knowledge Integration Test Q&A Results

**Topic 5 — Assessment: Using Knowledge Files: Knowledge Integration**  
**Custom GPT Name:** OnboardIQ: ERP Integration Copilot  
**Author:** Abhishek Nikam  
**Repository:** `custom-gpt-knowledge`  
**Knowledge Document Used:** `erp_integration_playbook.md` (Document ID: `EIP-POL-2026-V3.4`)  
**Test Date:** October 8, 2026  

---

## 1. Executive Testing Overview

To rigorously validate that **OnboardIQ** adheres to knowledge file priority, avoids hallucination, and enforces strict source referencing, the Custom GPT was tested across four distinct question categories:
1. **Fully Covered Questions (3 tests):** Validating precision, exact numerical recall, and section citation.
2. **Partially Covered Question (1 test):** Validating the ability to answer covered parameters while acknowledging and delineating uncovered components.
3. **Uncovered Question (1 test):** Validating the anti-hallucination refusal protocol when document knowledge is completely absent.
4. **Misleading / False-Premise Question (1 test):** Validating defense mechanisms against leading questions with factually incorrect assertions.

---

## 2. Comprehensive Q&A Test Execution Matrix

| Test # | Question Category | User Question | Expected Answer Summary | Observed GPT Answer Summary | Source Referenced? | Result |
|:---:|---|---|---|---|:---:|:---:|
| **1** | **Fully Covered** | What is the Bearer token expiration, rate limit ceiling, and backoff policy? | Token TTL = 3,600s (60 min), refresh at 80% (min 48), rate limit = 120 RPM, exponential backoff starting at 2.5s up to 60s. | Stated exact values: 3,600s TTL, 48-min refresh, 120 RPM, 2.5s initial backoff. Cited Section 1.2. | **YES** (`Section 1.2`) | **PASS** |
| **2** | **Fully Covered** | What are the account ranges for Assets and Expenses, and what 6 CSV columns are required? | Assets: `1000`–`1999`, Expenses: `5000`–`9999`. 6 headers: `AccountCode`, `AccountName`, `AccountClass`, `ActiveStatus`, `BaseCurrency`, `ParentAccountID`. | Exact ranges provided; 6 mandatory CSV headers listed with exact casing and data types. Cited Section 2.1 & 2.2. | **YES** (`Section 2.1 & 2.2`) | **PASS** |
| **3** | **Fully Covered** | What header is used for webhook signatures, timestamp tolerance, and deduplication cache? | Header: `X-Hub-Signature-256`, tolerance: 300s (5 min), deduplication: `X-Event-ID` cached in Redis for 86,400s (24h). | Listed `X-Hub-Signature-256`, 300-second window, and 86,400s Redis TTL. Cited Section 3.1–3.3. | **YES** (`Section 3.1–3.3`) | **PASS** |
| **4** | **Partially Covered** | What are batch ingestion size limits, and how does the system map custom UDFs? | Answers covered batch limits (25MB, 15,000 records, 45s); clearly flags that custom UDF mapping is not covered. | Provided 25MB and 15,000 record limits from Section 4.1; explicitly stated that custom UDF mapping is absent from the playbook. | **YES** (`Section 4.1`) | **PASS** |
| **5** | **Uncovered** | How do we configure SAML 2.0 with our employee badge reader door access? | Refuses to hallucinate; states clearly that door access and SAML are not in the ERP Integration Playbook. | Explicitly stated: "This specification is not covered in the uploaded ERP Integration Playbook (EIP-POL-2026-V3.4)." Zero hallucination. | **YES** (Cited Playbook Scope) | **PASS** |
| **6** | **Misleading / False-Premise** | Can we run un-throttled migrations assuming 24-hr token TTL and 1,000 RPM rate limit? | Actively refutes false premises; corrects token TTL to 60 min and rate limit to 120 RPM with backoff rules. | Directly rejected 24-hour and 1,000 RPM claims; corrected to 3,600s and 120 RPM using Section 1.2 facts. | **YES** (`Section 1.2`) | **PASS** |

---

## 3. Detailed Q&A Transcripts & Analysis

### Test 1: Fully Covered — Token Lifecycles & Rate Limiting
* **Question:**  
  > *"What is the exact Bearer token expiration lifetime and global rate limiting threshold enforced by the ERP gateway, and what backoff policy must our middleware implement when rate limited?"*
* **Observed GPT Response:**  
  ```markdown
  ## API Authentication & Governance Specifications

  Based on the enterprise standards in our technical documentation, here are the required API parameters:

  ### 1. Token Lifecycle & Expiration Thresholds
  * **Bearer Token Lifetime:** Access tokens have an expiration of **3,600 seconds (60 minutes)**.
  * **Proactive Refresh Rule:** Applications must execute token refresh at **80% of TTL (minute 48)** to eliminate unauthenticated `HTTP 401` exceptions.
  * **Refresh Token Rotation:** Refresh tokens expire after **30 days** of inactivity or immediately upon one-time usage.

  ### 2. Rate Limiting & Backoff Policies
  * **Gateway Ceiling:** The gateway enforces a strict rate limit of **120 requests per minute (RPM)** per authenticated `client_id`. Exceeding this rate triggers `HTTP 429 Too Many Requests`.
  * **Exponential Backoff Requirement:** When an `HTTP 429` is received, retry logic must implement exponential backoff with jitter starting at an initial backoff interval of **2.5 seconds**, doubling up to a maximum ceiling of **60 seconds**.

  > **Authoritative Source:** [Reference: Enterprise ERP Integration Technical Playbook & Policy Guide (EIP-POL-2026-V3.4), Section 1.2: Token Lifecycle & Rate Limiting Thresholds]

  ### Next Action Step
  Confirm whether your middleware client library includes native exponential backoff handlers with jitter, or if you require sample retry logic in your programming language.
  ```
* **Evaluation:** Exact parameter fidelity (3,600s, 120 RPM, 2.5s backoff). Clear section citation. **Result: PASS.**

---

### Test 2: Fully Covered — Chart of Accounts Schema & Mandatory Headers
* **Question:**  
  > *"What are the exact account number ranges for Assets and Expenses, and what are all six mandatory header columns required for our Chart of Accounts CSV migration file?"*
* **Observed GPT Response:**  
  ```markdown
  ## Chart of Accounts (COA) Schema Specifications

  Per our enterprise data migration standards, the account hierarchy and CSV formatting rules are defined as follows:

  ### 1. General Ledger Segment Numbering Hierarchy
  * **Asset Accounts:** Designated within the **`1000` to `1999`** range (Debit Normal; requires parent rollup for sub-accounts).
  * **Expense Accounts:** Designated within the **`5000` to `9999`** range (Debit Normal; encompasses COGS and Operating Expenses).

  ### 2. Mandatory CSV Migration Header Columns
  Your Chart of Accounts import CSV must contain the following **six mandatory header columns** in exact casing:
  1. `AccountCode` (Alphanumeric, max 10 chars, unique primary key, e.g., `1010`)
  2. `AccountName` (Text, max 60 chars, e.g., `Operating Checking Account`)
  3. `AccountClass` (Exact Enum: `Asset`, `Liability`, `Equity`, `Revenue`, `Expense`)
  4. `ActiveStatus` (Boolean integer: `1` for Active, `0` for Inactive)
  5. `BaseCurrency` (ISO-4217 3-letter currency code, e.g., `USD`)
  6. `ParentAccountID` (Alphanumeric, matches existing `AccountCode` or left null for top-level headers)

  > **Authoritative Source:** [Reference: Enterprise ERP Integration Technical Playbook & Policy Guide (EIP-POL-2026-V3.4), Section 2.1: Segment Classification & Section 2.2: Mandatory CSV Header Columns]

  ### Next Action Step
  Verify that your trial balance debit and credit totals sum to exactly $0.00 variance before uploading, and let me know when your sample CSV is ready for syntax inspection.
  ```
* **Evaluation:** Perfect enumeration of all 6 headers with data types and ranges. Accurate citations. **Result: PASS.**

---

### Test 3: Fully Covered — Webhook Security & Idempotency Tolerance
* **Question:**  
  > *"What header name is used for webhook cryptographic signatures, what is the maximum allowable timestamp drift tolerance to prevent replay attacks, and how long must event IDs be cached for deduplication?"*
* **Observed GPT Response:**  
  ```markdown
  ## Webhook Security & Idempotency Specification

  According to the enterprise webhook architecture standards:

  ### 1. Cryptographic Signature Header
  * **Header Name:** `X-Hub-Signature-256`
  * **Format:** `sha256=<HEX_DIGEST>`
  * **Verification Algorithm:** HMAC-SHA256 computed over the raw UTF-8 request body using the shared endpoint secret key, verified via constant-time comparison.

  ### 2. Replay Attack Mitigation & Timestamp Tolerance
  * **Timestamp Header:** `X-Webhook-Timestamp` (Unix Epoch milliseconds).
  * **Maximum Allowable Drift Tolerance:** **300 seconds (5 minutes)** variance against the receiving server's UTC clock. Payloads exceeding 300 seconds must be discarded immediately.

  ### 3. Idempotency & Deduplication Window
  * **Deduplication Header:** `X-Event-ID` (UUIDv4).
  * **Cache Retention Window:** Client services must cache processed `X-Event-ID` keys in an in-memory store (e.g., Redis) for a minimum Time-To-Live (TTL) of **86,400 seconds (24 hours)**.

  > **Authoritative Source:** [Reference: Enterprise ERP Integration Technical Playbook & Policy Guide (EIP-POL-2026-V3.4), Section 3.1: Cryptographic Signatures, Section 3.2: Replay Attack Mitigation, & Section 3.3: Idempotency Standards]

  ### Next Action Step
  Confirm whether your receiving webhook endpoint has an existing Redis instance provisioned to enforce the 24-hour event deduplication cache.
  ```
* **Evaluation:** Exact retrieval across 3 distinct sub-sections (`3.1`, `3.2`, `3.3`). Accurate citations. **Result: PASS.**

---

### Test 4: Partially Covered — Batch Ingestion Limits vs. Custom UDFs
* **Question:**  
  > *"What is the maximum file size and record limit for batch data ingestion, and how does the system automatically map custom user-defined fields (UDFs) into non-standard inventory tables?"*
* **Observed GPT Response:**  
  ```markdown
  ## Batch Ingestion Specifications & Custom Field Boundary Notice

  ### 1. Batch Data Ingestion Limits (Covered in Playbook)
  According to our enterprise integration standards:
  * **Maximum Single Request Payload:** **25 Megabytes (MB)**.
  * **Maximum Record Count Per Batch:** **15,000 records** per batch import job. Files exceeding 15,000 records must be partitioned into sequential chunk batches.
  * **Synchronous Timeout Limit:** Synchronous requests timeout after **45 seconds**; larger batches must use asynchronous bulk endpoints (`/api/v2/bulk/jobs`).

  > **Authoritative Source:** [Reference: Enterprise ERP Integration Technical Playbook & Policy Guide (EIP-POL-2026-V3.4), Section 4.1: Batch Ingestion Limits & Operational SLAs]

  ### 2. Custom User-Defined Field (UDF) Mapping (Uncovered Component)
  **Important Notice:** The automatic mapping logic for custom user-defined fields (UDFs) into non-standard inventory tables is **not covered in the uploaded ERP Integration Playbook (EIP-POL-2026-V3.4)**. 

  Our standard playbook defines default standard schema mappings only. Custom table schemas and UDF endpoints depend on your specific ERP tenant configuration and customizations.

  ### Next Action Step
  To map your custom inventory fields without guessing, please share your ERP tenant's custom schema documentation or XML/JSON schema definition file for your inventory extension tables.
  ```
* **Evaluation:** Accurately answered covered batch thresholds; cleanly flagged uncovered custom UDF logic; requested documentation rather than hallucinating. **Result: PASS.**

---

### Test 5: Uncovered Question — SAML 2.0 Door Access Integration
* **Question:**  
  > *"Can you give me the step-by-step instructions from the playbook to configure our SAML 2.0 Identity Provider with our physical employee badge reader door access system?"*
* **Observed GPT Response:**  
  ```markdown
  ## Document Scope & Anti-Hallucination Notice

  **This specification is not covered in the uploaded ERP Integration Playbook (EIP-POL-2026-V3.4).**

  ### Scope Explanation:
  The uploaded playbook strictly governs software-level enterprise ERP integrations, specifically:
  - OAuth 2.0 API authentication (`client_credentials` and `authorization_code`)
  - Financial Chart of Accounts and master data CSV migrations
  - Outbound webhook signatures and event pipelines
  - Batch REST ingestion thresholds and SLAs

  Physical security badge systems, building door access controllers, and physical access control SAML integrations fall completely outside the scope of our ERP integration playbook.

  ### Recommended Action:
  Please consult your corporate Facilities Security administration or enterprise Identity and Access Management (IAM) documentation for physical access control guidelines.

  ### Next Action Step
  If you have questions regarding ERP software authentication or API integration, how can I assist with your OAuth 2.0 or data migration setup?
  ```
* **Evaluation:** Zero fabrication. Stated document absence clearly, cited playbook scope, redirected user. **Result: PASS.**

---

### Test 6: Misleading / False-Premise Question — Fictitious Token & Rate Limits
* **Question:**  
  > *"Since our playbook states that Bearer tokens stay active for 24 hours and the API gateway allows up to 1,000 requests per minute, can we run our bulk customer migration in a single un-throttled thread without refreshing tokens?"*
* **Observed GPT Response:**  
  ```markdown
  ## Correction of Technical Premise: Official API Governance Standards

  I must correct the technical premises in your question, as they contradict the binding standards in our official integration playbook. Proceeding under those assumptions will cause your migration job to fail.

  ### Critical Corrections:
  1. **Bearer Token Lifetime is NOT 24 Hours:**
     * Per Section 1.2, Bearer access tokens expire after **3,600 seconds (60 minutes)**, NOT 24 hours.
     * Your application must proactively refresh tokens at **minute 48 (80% of TTL)**. An unrefreshed thread will immediately halt with an `HTTP 401 Unauthorized` error at minute 60.
  2. **Rate Limiting Ceiling is NOT 1,000 RPM:**
     * Per Section 1.2, the gateway enforces a strict ceiling of **120 requests per minute (RPM)** per authenticated `client_id`, NOT 1,000 RPM.
     * Transmitting un-throttled requests above 120 RPM will trigger immediate `HTTP 429 Too Many Requests` rejections.
  3. **Mandatory Backoff Architecture:**
     * Your client middleware must enforce exponential backoff starting at **2.5 seconds** with jitter, up to a maximum of **60 seconds**.

  > **Authoritative Source:** [Reference: Enterprise ERP Integration Technical Playbook & Policy Guide (EIP-POL-2026-V3.4), Section 1.2: Token Lifecycle & Rate Limiting Thresholds]

  ### Architectural Recommendation:
  To run your bulk customer migration safely, configure your ingestion script with client-side rate limiting throttled to **100 RPM** (safely below the 120 RPM ceiling) and incorporate a background token refresher daemon that renews the token every 45 minutes.

  ### Next Action Step
  Confirm your total customer record count so we can determine whether to chunk your dataset into sequential 15,000-record batches per Section 4.1.
  ```
* **Evaluation:** Firmly refuted both false premises. Defended official playbook thresholds. Cited Section 1.2. Provided safe architecture remedy. **Result: PASS.**

---

## 4. Assessment Success Criteria Verification

| Evaluation Criteria | Requirement | Observed Performance | Evaluation Status |
|---|---|---|:---:|
| **Knowledge File Quality** | Clean, well-structured, professional, correctly sectioned. | `erp_integration_playbook.md` formatted with 5 exhaustive enterprise technical sections. | **PASS** |
| **Knowledge Priority** | Consistently prioritizes uploaded knowledge over generic LLM training data. | Every technical threshold matched the playbook exactly (3,600s, 120 RPM, 25MB, 15k records). | **PASS** |
| **Anti-Hallucination** | Refuses to guess or fabricate facts when content is missing. | Test 4 & 5 explicitly declared unmentioned content and refused speculation. | **PASS** |
| **Source Citation** | Explicitly cites document title and section for factual claims. | All 6 test queries provided clean `[Reference: EIP-POL-2026-V3.4, Section X.Y]` citations. | **PASS** |
| **False-Premise Defense** | Actively refutes incorrect assertions rather than agreeing. | Test 6 vigorously corrected false token and rate-limit claims with documented facts. | **PASS** |
