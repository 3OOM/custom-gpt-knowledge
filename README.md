# Topic 5 Assessment — Using Knowledge Files: Knowledge Integration

**Course Topic:** Topic 5 — Using Knowledge Files: Knowledge Integration  
**Student Name:** Abhishek Nikam  
**Custom GPT Name:** OnboardIQ: ERP Integration Copilot  
**Repository:** `custom-gpt-knowledge`  
**GitHub Link:** https://github.com/3OOM/custom-gpt-knowledge  

---

## 📌 Project Overview

This repository contains the complete assessment deliverables for **Topic 5 — Using Knowledge Files: Knowledge Integration**. Building directly upon the foundation of Topics 1–4 (Use Case Definition, Instruction Drafting, Persona Engineering, and Conversation Flow Design), this project integrates an authoritative enterprise knowledge base into **OnboardIQ: ERP Integration Copilot**.

The project establishes **strict knowledge-file priority**, enforces **source-referencing protocols**, codifies **anti-hallucination guardrails**, and validates model accuracy across **fully covered**, **partially covered**, **uncovered**, and **misleading / false-premise** test queries.

---

## 📁 Repository Structure & Deliverables

```
custom-gpt-knowledge/
├── README.md                            # Complete project documentation, LMS submission text, video script
├── erp_integration_playbook.md          # Core Deliverable: Authoritative Enterprise Technical Knowledge Base (Doc ID: EIP-POL-2026-V3.4)
├── knowledge_enhanced_instructions.md  # Core Deliverable: Instructions with knowledge priority, citation, and anti-hallucination rules
└── test_qa_results.md                  # Core Deliverable: Detailed Q&A test execution logs across all 4 question categories
```

---

## 📄 Key Deliverables Manifest

| Deliverable File | Type | Description |
|---|---|---|
| [`erp_integration_playbook.md`](erp_integration_playbook.md) | **Mandatory** | The official enterprise knowledge file: a structured technical policy document defining OAuth 2.0 grant types, 3,600s token TTLs, 120 RPM rate limits, 4-tier Chart of Accounts schemas, 6 mandatory CSV headers, HMAC-SHA256 webhooks, and batch SLAs. |
| [`knowledge_enhanced_instructions.md`](knowledge_enhanced_instructions.md) | **Mandatory** | Production system instructions enforcing strict knowledge priority, mandatory section referencing (`[Reference: EIP-POL-2026-V3.4, Section X.Y]`), and anti-hallucination refusal protocols. |
| [`test_qa_results.md`](test_qa_results.md) | **Mandatory** | In-depth testing dossier logging 6 varied test queries across Fully Covered, Partially Covered, Uncovered, and Misleading scenarios, verifying 100% factual accuracy and source citation. |
| [`README.md`](README.md) | **Guide** | Project blueprint, LMS copy-paste submission description, 3-minute video presentation script, and practical assessment checklist. |

---

## 📋 Copy-Paste LMS Submission Details

When submitting in the LMS, copy and paste the following text into the respective submission fields:

### 1. Description Field Submission Text

```markdown
### Topic 5 Assessment: Using Knowledge Files — Knowledge Integration

**GPT Name:** OnboardIQ: ERP Integration Copilot (Persona: Principal Solutions Architect Marcus Vance)
**Repository URL:** https://github.com/3OOM/custom-gpt-knowledge
**Deliverables:** erp_integration_playbook.md, knowledge_enhanced_instructions.md, test_qa_results.md (PDFs attached)

#### 1. Knowledge Base Architecture & Priority Rules
- Uploaded Knowledge File: "Enterprise ERP Integration Technical Playbook & Policy Guide" (Document ID: EIP-POL-2026-V3.4). A clean, well-structured enterprise policy covering OAuth 2.0 grant types, token lifetimes (3,600s), rate limits (120 RPM), 6 mandatory Chart of Accounts CSV headers, HMAC-SHA256 webhook signatures, and batch ingestion SLAs.
- Priority Mandate: Codified the rule: "Always prioritize uploaded knowledge files over general knowledge. The parameters, thresholds, and schemas defined in EIP-POL-2026-V3.4 represent binding enterprise standards."
- Source Citation Protocol: Codified mandatory section referencing: "Whenever providing factual specifications, explicitly reference the relevant document title and section number (e.g., [Reference: EIP-POL-2026-V3.4, Section 1.2])."
- Anti-Hallucination & Uncovered Policy: If a query asks about specifications absent from the document, the model must explicitly state: "This specification is not covered in the uploaded ERP Integration Playbook (EIP-POL-2026-V3.4)" and refuse speculative extrapolation.

#### 2. Challenges Faced with Retrieval Accuracy & Resolutions
- Challenge 1 (Generic Model Knowledge Interference): Language models often substitute generic REST defaults (e.g., assuming 24-hour token expiration or 600 RPM rate limits) instead of adhering to tenant-specific standards.
  - Resolution: Added explicit instruction directives instructing the model to treat the uploaded document as the single source of truth and to cite section numbers on every factual claim.
- Challenge 2 (Handling Partially Covered Inquiries): When a user asked about batch file size limits (covered) alongside custom UDF field mapping (not covered), earlier models either hallucinated custom field rules or refused the entire question.
  - Resolution: Structured the instructions to bifurcate responses into covered facts (cited with section numbers) and explicitly identified uncovered components, prompting the user for supplementary tenant-specific documentation.
- Challenge 3 (Defending Against Misleading Questions): Users asking questions containing false premises (e.g., "Since the playbook allows 1,000 RPM, can we run un-throttled?").
  - Resolution: Programmed an active defense protocol that detects false technical premises, refutes them explicitly with cited document facts (120 RPM limit, 2.5s backoff), and prevents catastrophic operational errors.

#### 3. Key Assumptions Made
- The uploaded playbook represents authoritative architectural law for all client onboarding projects.
- Users require exact numerical precision (e.g., 300-second timestamp drift, 86,400-second Redis TTL) and concrete section references to present to Solution Architects and security compliance officers.

#### 4. Q&A Testing Performance Summary
Tested across 6 queries covering all 4 mandated categories: Fully Covered (3), Partially Covered (1), Uncovered (1), and Misleading (1). All tests achieved 100% factual accuracy, zero hallucinations, and consistent section referencing.
```

### 2. Video Demonstration Link
* **Loom / YouTube Link:** `https://www.loom.com/share/custom-gpt-knowledge-walkthrough` *(Replace with your recorded video URL)*

---

## 🎥 Video Demonstration Script (3-Minute Loom / YouTube Guide)

Follow this structured script when recording your assessment walkthrough video:

| Timestamp | Section | Talking Points & Visual Demonstration |
|---|---|---|
| **0:00 – 0:40** | **Introduction & Knowledge Integration** | • Open [custom-gpt-knowledge](https://github.com/3OOM/custom-gpt-knowledge).<br>• Introduce yourself: *"Hello, I am Abhishek Nikam, presenting Topic 5: Knowledge File Integration for OnboardIQ."*<br>• Explain the purpose: Equipping OnboardIQ with an authoritative ERP Integration Playbook (EIP-POL-2026-V3.4) and enforcing strict document priority. |
| **0:41 – 1:30** | **Knowledge File & Instructions Walkthrough** | • Open [erp_integration_playbook.md](file:///C:/Users/abhis/.gemini/antigravity-ide/scratch/custom-gpt-knowledge/erp_integration_playbook.md).<br>• Highlight Section 1 (3,600s token TTL, 120 RPM rate limit), Section 2 (6 mandatory CSV headers), and Section 3 (HMAC webhooks).<br>• Open [knowledge_enhanced_instructions.md](file:///C:/Users/abhis/.gemini/antigravity-ide/scratch/custom-gpt-knowledge/knowledge_enhanced_instructions.md) to showcase the **Knowledge Priority Rule**, **Mandatory Citation Rule**, and **Anti-Hallucination Guardrail**. |
| **1:31 – 2:25** | **Testing Across 4 Question Modalities** | • Open [test_qa_results.md](file:///C:/Users/abhis/.gemini/antigravity-ide/scratch/custom-gpt-knowledge/test_qa_results.md).<br>• **Fully Covered (Tests 1–3):** Show exact retrieval of token limits, CSV headers, and webhook headers with Section citations.<br>• **Partially Covered (Test 4):** Show how OnboardIQ answered batch limits but cleanly flagged uncovered custom UDF mapping.<br>• **Uncovered (Test 5):** Show zero-hallucination refusal on physical door badge access.<br>• **Misleading (Test 6):** Show how the model actively refuted false 24-hr token and 1,000 RPM premises. |
| **2:26 – 3:00** | **PDF Dossiers & LMS Readiness** | • Open [`OnboardIQ_Knowledge_Integration_Complete_Assessment.pdf`](file:///C:/Users/abhis/.gemini/antigravity-ide/scratch/custom-gpt-knowledge/OnboardIQ_Knowledge_Integration_Complete_Assessment.pdf).<br>• Showcase the compiled PDF dossier prepared for LMS upload.<br>• Conclude with LMS submission readiness and GitHub repository verification. |

---

## ✅ Practical Assessment Checklist Verification

| Requirement / Task Description | Status | Verification Reference |
|---|:---:|---|
| At least 1 knowledge file prepared and uploaded | **PASS** | `erp_integration_playbook.md` (Document ID: `EIP-POL-2026-V3.4`) |
| Instructions updated to prioritize knowledge file content | **PASS** | `knowledge_enhanced_instructions.md` (Section 2) |
| Anti-hallucination rule added | **PASS** | `knowledge_enhanced_instructions.md` (Section 3.1 & 3.3) |
| Source-citation rule added | **PASS** | `knowledge_enhanced_instructions.md` (Section 3.2) |
| Fully covered question tested | **PASS** | `test_qa_results.md` (Tests 1, 2, and 3) |
| Partially covered question tested | **PASS** | `test_qa_results.md` (Test 4) |
| Uncovered question tested | **PASS** | `test_qa_results.md` (Test 5) |
| Misleading question tested | **PASS** | `test_qa_results.md` (Test 6) |
| Q&A results saved as `test_qa_results.md` | **PASS** | Located in root directory |
| Necessary docs pushed to GitHub | **PASS** | Clean markdown files committed and pushed |
| High-quality PDF files generated for LMS upload | **PASS** | Generated via Edge headless rendering engine |
