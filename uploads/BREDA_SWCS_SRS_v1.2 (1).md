                                                           ![][image1]

**SOFTWARE REQUIREMENTS SPECIFICATION**

**BREDA Single Window Clearance System (SWCS)**

Bihar Renewable Energy Development Agency  
Submitted By

 ![][image2]

AKIKO SHERMAN INFOTECH

Version 1.2

# **Table of Contents**

[**Table of Contents	2**](#heading=)

[**1\. Introduction	4**](#heading=)

[1.1 Purpose	4](#heading=)

[1.2 Scope	4](#heading=)

[1.3 Definitions, Acronyms & Abbreviations	4](#heading=)

[1.4 References	4](#heading=)

[**2\. Overall Description	5**](#heading=)

[2.1 Background	5](#heading=)

[2.2 Roles & Access Overview	5](#heading=)

[2.3 Module Summary	5](#heading=)

[2.4 Operating Environment	6](#heading=)

[2.5 Assumptions and Constraints	6](#heading=)

[**3\. Functional Requirements	7**](#heading=)

[3.1 Layer 1 — Applicant-Facing	7](#heading=)

[3.1.1 M01 — Public Portal & Landing Page	7](#heading=)

[3.1.2 M02 — User Registration & Authentication	7](#heading=)

[3.1.3 M03 — Project Registration (Multi-Part Application Form)	7](#heading=)

[3.1.4 M04 — Document Management (Section D)	8](#heading=)

[3.1.5 M05 — Fee Payment & Acknowledgement (Section F)	9](#heading=)

[3.1.6 M06 — Applicant Dashboard	10](#heading=)

[3.1.7 M07 — Application Status Tracker	10](#heading=)

[3.2 Layer 2 — Workflow & Processing	11](#heading=)

[3.2.1 M08 — Application Processing & BREDA Review	11](#heading=)

[3.2.2 M09 — Inter-Departmental Clearance Workflow	12](#heading=)

[3.2.3 M10 — Commissioning & Compliance	12](#heading=)

[3.2.4 M11 — Query Management	13](#heading=)

[3.2.5 M12 — SLA Management	13](#heading=)

[3.3 Layer 3 — Platform & Admin	14](#heading=)

[3.3.1 M13 — Notifications & Communication	14](#heading=)

[3.3.2 M14 — Admin Console & Configuration	14](#heading=)

[3.3.3 M15 — MIS & Reporting	15](#heading=)

[3.3.4 M16 — Helpdesk, Grievance & Appeal Management	15](#heading=)

[3.3.5 M17 — Security, Audit & Compliance	15](#heading=)

[**4\. Role and Form Based Workflows	16**](#heading=)

[4.1 Registration & Login Flow	16](#heading=)

[4.2 Project Application Submission Flow (Step 0 → Section F, payment)	18](#heading=)

[4.3 Offline Payment & Finance Verification Flow	20](#heading=)

[4.4 BREDA Query — Resubmission Loop	21](#heading=)

[4.5 Apply for Clearances Flow (per department/service)	23](#heading=)

[4.6 Department Query — Resubmission Loop	25](#heading=)

[4.7 Commissioning → COD Verification → Registration Number/Certificate Flow	27](#heading=)

[**5\. Data Requirements	29**](#heading=)

[5.1 Master Data	29](#heading=)

[5.2 Transaction Data	29](#heading=)

[**6\. External Interface Requirements	30**](#heading=)

[6.1 Software Interfaces	30](#heading=)

[6.2 User Interface Requirements (high-level)	30](#heading=)

[**7\. Non-Functional Requirements	31**](#heading=)

[7.1 Performance	31](#heading=)

[7.2 Availability & Reliability	31](#heading=)

[7.3 Security	31](#heading=)

[7.4 Compliance	31](#heading=)

[7.5 Usability & Accessibility	31](#heading=)

[7.6 Maintainability	31](#heading=)

[**8\. Hosting and Deployment Environment	32**](#heading=)

[8.1 Hosting & Infrastructure	32](#heading=)

[8.2 Operations & Maintenance (O\&M)	32](#heading=)

[**9\. Acceptance Criteria	33**](#heading=)

[**Appendix A — Forms & Field Inventory	34**](#heading=)

[**Appendix B — Glossary	37**](#heading=)

[**Appendix C — SLA Reference Table	38**](#heading=)

[**Appendix D — Acknowledgement & Registration Certificate Formats	39**](#heading=)

[**Queries & Decisions Required (BREDA / Department Input Needed)	40**](#heading=)

# **BREDA SWCS — SRS**

**Bihar Renewable Energy Development Agency — Single Window Clearance System**

# **1\. Introduction**

## **1.1 Purpose**

Standard statement — SRS as agreed baseline for design, development, testing, acceptance; sign-off required before development begins.

## **1.2 Scope**

**Phase 1 (Go-Live)**

* Full design and development of the BREDA SWCS portal

* 17 modules (Section 3), organized under three layers — applicant-facing, workflow/processing, and platform/admin

* Phase 1 department integrations (15 departments \+ payment gateway \+ auth/SMS/email gateway)


**Optional / Post Go-Live (within overall project scope, sequenced later)**

* BSPGCL, BHPC, Dept. of Agriculture, Registration Dept. (stamp duty), MNRE/SECI Portal, Aadhaar/e-KYC integration

* WhatsApp notification gateway


## **1.3 Definitions, Acronyms & Abbreviations**

See Appendix B — Glossary.

## **1.4 References**

* Bihar Policy for Promotion of New and Renewable Energy Sources, 2025 (Resolution No. 3239, 10.07.2025)

* BREDA RFP

* GIGW guidelines

* CERT-In Audit

# **2\. Overall Description**

## **2.1 Background**

Fragmented approval processes, parallel clearances, lack of time-bound accountability; SWCS as centralized digital platform for the full project lifecycle.

## **2.2 Roles & Access Overview**

| Role | Scope of Access | Key Permissions |
| :---- | :---- | :---- |
| applicant | Own applications, documents, payments, clearances | Fill/edit own application (until submission), upload documents, make payments, respond to queries, apply for clearances, track status |
| breda\_admin | All applications — review | Review applications, raise/resolve queries, approve/reject application, view consolidated clearance status, manage helpdesk tickets |
| system\_admin | Platform configuration | Manage users/roles, master data, fee table, SLA configuration, view audit logs |
| dept\_officer | Clearances assigned to their own department | Review clearance applications for their department, raise queries, approve/reject |
| finance\_officer  | Offline payment verification | Verify offline payment receipts, approve/reject payment proofs |

## **2.3 Module Summary**

All 17 modules, organized under the three layers (as in the Module Feature List):

**Layer 1 — Applicant-Facing**

| Module | Name | Scope / Core Function |
| :---- | :---- | :---- |
| M01 | Public Portal & Landing Page | Public portal — landing page, navigation, info sections |
| M02 | User Registration & Authentication | Self-registration, OTP, login, profile, dept user onboarding |
| M03 | Project Registration — Multi-Part Application Form | Step 0 \+ Sections A–F application form |
| M04 | Document Management | Upload, validation, versioning across all sections |
| M05 | Fee Payment & Acknowledgement | Fee calculation, GRAS/e-Treasury, online/offline payment, acknowledgement |
| M06 | Applicant Dashboard | Stats, projects table, status badges, navigation |
| M07 | Application Status Tracker | Stepper timeline, query loops, department-wise status |

**Layer 2 — Workflow & Processing**

| Module | Name | Scope / Core Function |
| :---- | :---- | :---- |
| M08 | Application Processing & BREDA Review | BREDA review of application, approval → unlocks clearances |
| M09 | Inter-Departmental Clearance Workflow | Apply for Clearances, department review, approve/query/reject |
| M10 | Commissioning & Compliance | COD entry, certificate upload, BREDA verification, Registration Number/Certificate |
| M11 | Query Management | Field-level & general queries, response cycle (BREDA \+ departments) |
| M12 | SLA Management | SLA timers, pause/resume on query, breach & escalation |

**Layer 3 — Platform & Admin**

| Module | Name | Scope / Core Function |
| :---- | :---- | :---- |
| M13 | Notifications & Communication | Event → recipient → channel matrix (SMS, email, in-app) |
| M14 | Admin Console & Configuration | User/role management, master data, fee/SLA config, audit log viewer |
| M15 | MIS & Reporting | BREDA Admin dashboards, KPIs, charts, exports |
| M16 | Helpdesk, Grievance & Appeal Management | Ticketing, escalation, resolution tracking |
| M17 | Security, Audit & Compliance | RBAC, encryption, audit trail, accessibility, SSL — see Section 7 (NFRs) |

## **2.4 Operating Environment**

* Language: English only

* Hosting: BREDA-provided Data Centre \+ DR

* Browsers: Chrome, Firefox, Safari, Edge — latest two versions

* Devices: Desktop, Tablet (responsive)

* Connectivity: Full internet required

## **2.5 Assumptions and Constraints**

**Assumptions**

* Department-provided master data (districts/blocks/panchayats/villages, PSS/GSS station lists) available before relevant module development

* Clearance-service-specific fields/documents provided by each department

* GRAS/e-Treasury integration details confirmed by BREDA before payment module build

**Constraints**

* GIGW \+ WCAG 2.1 AA accessibility

* CERT-In Audit ("Safe to Host" clearance) mandatory before production

# **3\. Functional Requirements**

*REQ-ID format keyed to module, e.g. REQ-M01-001, REQ-M02-001 ... REQ-M16-001. Dropdown/radio option values for Step 0 – Section C fields are listed in Appendix A.*

## **3.1 Layer 1 — Applicant-Facing**

#### **3.1.1 M01 — Public Portal & Landing Page**

* Public landing page, accessible without login

* Standard government-portal sections — about/intro, schemes & programmes, integrated departments list, footer with mandatory links (Sitemap, Website Policy, Help, Feedback, etc.)

* Entry points to Login / Register

* Basic accessibility (skip-to-content, accessibility controls)

* No application tracking shown or offered on the public landing page; tracking is available only to logged-in applicants (see M07)

#### **3.1.2 M02 — User Registration & Authentication**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M02-001 | Applicant | Complete 2-step self-registration (Step 1: basic details, Step 2: communication address) |
| REQ-M02-002 | Applicant | Step 1 captures Name, Applicant Type, Mobile Number (+91), Email ID, Password |
| REQ-M02-003 | Applicant | Select Applicant Type at registration; if "Other," a free-text field appears to specify |
| REQ-M02-004 | System | OTP verification of mobile and email required before registration completes |
| REQ-M02-005 | Applicant | Step 2 captures Communication Address — Street/House No. (free text), Village (mandatory), City (optional), District/Block/Panchayat (dropdowns from master data), Pin Code |
| REQ-M02-006 | System | User ID \= applicant's email address, permanent (cannot be changed); communicated via SMS and email on successful registration |
| REQ-M02-007 | Applicant | Login via Mobile Number or Email ID, with Password-based or OTP-based authentication |
| REQ-M02-008 | System | Forgot-password / reset-password flow available |
| REQ-M02-009 | System | Session timeout and session management enforced |
| REQ-M02-010 | BREDA Admin | Create department user accounts; activation link sent via email; forced password change on first login |
| REQ-M02-011 | Applicant | Edit Mobile and Aadhaar/PAN from profile (Email / User ID is permanent and cannot be changed) |

#### **3.1.3 M03 — Project Registration (Multi-Part Application Form)**

*Covers form structure, Step 0, and Sections A, B, C, E. Section D → M04, Section F → M05. "Apply for Clearances" → M09.*

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M03-001 | System | Multi-section form (Step 0 – Section F) with step indicator; all sections visible and navigable at all times |
| REQ-M03-002 | System | Section E (Declaration) and Section F (Payment) locked until Step 0 – Section D fully and validly complete |
| REQ-M03-003 | System | Auto-save form data throughout entry, plus an explicit "Save as Draft" option |
| REQ-M03-004 | System | Form becomes fully read-only for applicant after submission. Applicant may initiate a Change Request (see REQ-M08-010) for field modifications; BREDA Admin reviews and approves or rejects. Once the acknowledgement number is generated no further change request will be entertained.  |
| REQ-M03-005 | Applicant | Step 0 — enter Plant Capacity and select Purpose of Project; if Purpose \= "Other," a free-text field appears |
| REQ-M03-006 | System | Section A — Name of Applicant and Type of Applicant shown read-only, auto-filled from registration |
| REQ-M03-007 | Applicant | Section A — Communication Address (pre-filled, editable) and Project Address; "Same as Communication Address" checkbox copies current Communication Address values into Project Address fields (one-time copy, toggleable anytime); Project Address fields remain independently editable thereafter and include mandatory Latitude & Longitude |
| REQ-M03-008 | Applicant | Section A — Mobile/Email shown view-only (pre-filled); optional Alternate Mobile/Email |
| REQ-M03-009 | Applicant | Section A — PAN Number (mandatory), Aadhaar Number (mandatory), GST Number (mandatory if applicable) |
| REQ-M03-010 | Applicant | Section B — select Power Plant type via two-step conditional (type, then sub-type/storage as applicable); enter DISCOM, Nearest PSS/GSS (auto-ruled by capacity), Distance (km, with metres shown), Type of Power Evacuation (free-text appears if “Other”), Proposed COD (now mandatory, editable before submission), Project Cost, and EPC Contractor details |
| REQ-M03-011 | Applicant | Section C — select Land Ownership Document Type (self-attested copy upload becomes mandatory once selected), Land Category, and Land/Rooftop Area |
| REQ-M03-012 | System | Section E — read-only declaration with auto-filled applicant/organisation name, confirmation checkbox, Print Preview, including the clearance-responsibility paragraph (confirmed retained by department), displayed as a highlighted “Note\*” callout.  |
| REQ-M03-013 | System | "Proceed to Payment" enabled only after Section E checkbox ticked and Step 0 – D complete |

#### **3.1.4 M04 — Document Management (Section D)**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M04-001 | Applicant | Section D upload slots: DPR, Firm's Registration Certificate, Startup India/Udyam/NSIC/Industry Certificate, GST Certificate, PAN Detail, Aadhaar Detail, Audited Balance Sheet (non-mandatory), ITR Certificate, PPA (mandatory only if Purpose \= PPA with DISCOM) |
| REQ-M04-002 | Applicant | Add unlimited custom documents — free-text label \+ upload; each gets a system-assigned document ID, so reviewers can flag it in a query (REQ-M11-001) by ID, with the applicant's label shown for context |
| REQ-M04-003 | System | Each upload shows status indicator, filename, remove option |
| REQ-M04-004 | System | File size limits, applied across every upload point in the portal (Section C, D, F, and per-clearance uploads in M09) — DPR up to 25 MB, all other documents up to 5 MB, scanned images up to 2 MB |
| REQ-M04-005 | System | Supported formats — PDF, DOC, DOCX, Excel, image; validated on upload |
| REQ-M04-006 | System | Version control for resubmitted documents (e.g. revised DPR); older versions retained for audit |
| REQ-M04-007 | BREDA Admin / Dept Officer | View uploaded documents |

#### **3.1.5 M05 — Fee Payment & Acknowledgement (Section F)**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M05-001 | System | Auto-calculate processing fee from Step 0 capacity against the admin-configurable fee table |
| REQ-M05-002 | System | Display fee reference table (read-only) and auto-calculated fee incl. GST (rate per Q-04) |
| REQ-M05-003 | Applicant | Choose Payment Method — Online or Offline (DD/cheque only) |
| REQ-M05-004 | System | Online — GRAS/e-Treasury integration; on success show Transaction ID, generate Acknowledgement Number immediately (number format per OD-01; acknowledgement document format — see Appendix), confirm via SMS/email |
| REQ-M05-005 | Applicant | Offline — upload DD/cheque (Demand Draft) copy/number only; status moves to PAYMENT\_VERIFICATION\_PENDING |
| REQ-M05-006 | Finance Officer / BREDA Admin | Verify offline receipt; Acknowledgement Number generated only after verification; acknowledgement document format — see Appendix.  |
| REQ-M05-007 | System Admin | Fee table changes apply to future applications only, never retroactively |
| REQ-M05-008 | System | Registration/processing fee is non-refundable. On application rejection at any stage after payment, no refund is issued. The non-refundable nature of the fee must be clearly stated on the payment screen before the applicant confirms payment. |
| REQ-M05-009 | System | Once the registration fee is paid, the applicant cannot voluntarily cancel or withdraw the application. No cancellation path is provided to the applicant post-payment. |

#### **3.1.6 M06 — Applicant Dashboard**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M06-001 | System | Display summary stats (e.g. total projects, under review, approved, payment pending) |
| REQ-M06-002 | System | Display a list/table of the applicant's projects with status and quick actions (View / Continue / Track) |
| REQ-M06-003 | System | Provide sidebar navigation to key areas — applications, payments, notifications, profile |
| REQ-M06-004 | System | Show notification/badge indicators for pending actions |
| REQ-M06-005 | System | Align status badges with the application status list (see M07) |

#### **3.1.7 M07 — Application Status Tracker**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M07-001 | System | Show visible progression through key stages: submission → payment → BREDA review → Apply for Clearances → department review → outcome (approved/rejected) |
| REQ-M07-002 | System | Show visibility into queries raised at any stage (BREDA or department-specific), with a reply/upload mechanism for the applicant |
| REQ-M07-003 | System | Show per-department status and SLA visibility once the application is at clearance stage |
| REQ-M07-004 | Applicant | Access the tracker only when logged in (no tracking without login) |

**Application Status Reference**

| Status | Description |
| :---- | :---- |
| DRAFT | Application being filled, not yet submitted |
| PAYMENT\_PENDING | Form complete, payment not done |
| PAYMENT\_VERIFICATION\_PENDING | Offline receipt uploaded, awaiting Finance verification |
| PAYMENT\_VERIFIED | Internal only; moves immediately to SUBMITTED\_TO\_BREDA |
| SUBMITTED\_TO\_BREDA | Payment confirmed (Acknowledgement Number generated); under BREDA review |
| BREDA\_QUERY\_RAISED | BREDA has raised a query |
| RESUBMITTED\_TO\_BREDA | Applicant has responded and resubmitted to BREDA |
| BREDA\_APPROVED | BREDA accepted; "Apply for Clearances" now available to applicant |
| UNDER\_DEPARTMENT\_REVIEW | Applicant has submitted at least one clearance; one or more clearances under department review |
| DEPARTMENT\_QUERY\_RAISED | A department has raised a query on a clearance |
| DEPARTMENT\_QUERY\_RESPONDED | Applicant has responded to a department query |
| APPROVED | All submitted clearances approved; Registration Number/Certificate generated after commissioning |
| REJECTED | Rejected at BREDA stage, or relevant clearance(s) rejected |
| COD\_PENDING | All clearances approved; Registration Number generation pending as actual COD has not yet been entered by the applicant |
| COD\_REJECTED | BREDA has raised a query or rejected the uploaded COD certificate; applicants must re-upload the correct document |

## **3.2 Layer 2 — Workflow & Processing**

#### **3.2.1 M08 — Application Processing & BREDA Review**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M08-001 | BREDA Admin | Receive submitted applications after payment confirmation and Acknowledgement Number generation |
| REQ-M08-002 | BREDA Admin | Review the application (Step 0 – Section E) for completeness and correctness |
| REQ-M08-003 | System | Run BREDA review SLA timer (15 days, per SLA reference table); pause on query raised, resume on applicant response (per M12) |
| REQ-M08-004 | BREDA Admin | Raise a query (field-level or general — see M11) on the application |
| REQ-M08-005 | Applicant | Respond to and resubmit against BREDA query; review loop continues until accepted or rejected |
| REQ-M08-006 | BREDA Admin | Reject or cancel the application if found non-compliant — status REJECTED, no further clearance flow. BREDA Admin can reject or cancel at any point in time. Reasons must be recorded in the audit trail. |
| REQ-M08-007 | System | On BREDA acceptance, set status to BREDA\_APPROVED and enable "Apply for Clearances" on the applicant dashboard |
| REQ-M08-008 | System | BREDA does not review individual clearance applications — these go directly to departments |
| REQ-M08-009 | Applicant | Add a new clearance at any time after initial submission without BREDA re-review |
| REQ-M08-010 | Applicant / BREDA Admin | Change Request workflow: applicant submits a change request for modifications to a submitted application. BREDA Admin approves (fields unlocked; BREDA re-reviews updated content) or rejects (fields remain locked; reason recorded). Scope of changes and impact on in-progress clearances to be confirmed by BREDA. |

#### **3.2.2 M09 — Inter-Departmental Clearance Workflow**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M09-001 | Applicant | Select clearance services (department cards, individual clearance services as checkboxes); at least one selection mandatory |
| REQ-M09-002 | System | Each selected service expands to show service-specific fields and document uploads (per department-provided definitions; file size limits per REQ-M04-004) |
| REQ-M09-003 | Applicant | Upload or enter receipt number as proof of clearance fee payment made outside the system, per clearance where applicable |
| REQ-M09-004 | System | Submit each clearance directly to the concerned department officer; BREDA notified if needed, but does not route |
| REQ-M09-005 | System | Notify department nodal officer via email/SMS with a direct portal link on each submission |
| REQ-M09-006 | Dept Officer | Review clearance application — Approve / Raise Query / Reject |
| REQ-M09-007 | System | Track each clearance independently with its own status and SLA |
| REQ-M09-008 | System | Keep overall application status as UNDER\_DEPARTMENT\_REVIEW until all submitted clearances reach a final decision |
| REQ-M09-009 | System | Set overall status to APPROVED only when all submitted clearances are approved |
| REQ-M09-010 | Applicant | Reapply separately for any clearance that was rejected |
| REQ-M09-011 | BREDA Admin | Issue a consolidated clearance status once all submitted clearances are approved (does not substitute statutory approvals not applied for) |

#### **3.2.3 M10 — Commissioning & Compliance**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M10-001 | Applicant | Submit commissioning details after all submitted clearances are approved |
| REQ-M10-002 | Applicant | Enter actual Commercial Operation Date (COD). A visible placeholder for the actual COD date must be shown at all times. If not entered, application status remains COD\_PENDING — no auto-cancellation. |
| REQ-M10-003 | Applicant | Upload COD Certificate received from concerned department (PDF, up to 5 MB) |
| REQ-M10-004 | BREDA Admin | Verify the uploaded COD certificate, or raise a query if an incorrect document is uploaded. On query, applicant must re-upload the correct COD certificate. Status moves to COD\_REJECTED; reverts to pending COD verification on re-upload. |
| REQ-M10-005 | System | On verification, generate Registration Number (format: BREDA/SPV/Power Plant/\<sequence\>/\<year\>, per Registration Certificate format — Appendix D) and issue Registration Certificate (Appendix D) |
| REQ-M10-006 | System | If actual COD is not entered after the expected/proposed date, flag status as COD\_PENDING and escalate via alert only — no auto-cancellation. Application remains COD\_PENDING until actual COD is entered. |
| REQ-M10-007 | Applicant | If BREDA raises a query or rejects the uploaded COD certificate, the applicant can re-upload the correct COD document for the same project without restarting the full application. Applies to both query-based and direct rejection scenarios at the COD verification stage. |

#### **3.2.4 M11 — Query Management**

*Applies to M08 (BREDA review), M09 (department clearance review), and M10 (COD document verification)*

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M11-001 | BREDA Admin / Dept Officer | Raise a field-level query — flags specific fields/sections/documents requiring correction |
| REQ-M11-002 | BREDA Admin / Dept Officer | Raise a general/free-text query for clarifications not tied to a specific field |
| REQ-M11-003 | System | Support combined queries — field-level flags plus a general comment in one instance |
| REQ-M11-004 | System | Notify applicant via SMS, email, and in-app notification when a query is raised |
| REQ-M11-005 | Applicant | Respond — correct flagged fields, upload corrected documents, and/or provide a free-text reply, in one resubmission |
| REQ-M11-006 | System | Repeat the query-response cycle until the reviewer gives a final decision (Approve/Reject) |
| REQ-M11-007 | System | Time-stamp and log every query and response in the audit trail |
| REQ-M11-008 | System | During a query-response cycle, only the fields/documents flagged in the query become editable/re-uploadable; all other fields remain locked (read-only). \[Pending BREDA confirmation: whether query response allows field-level edits only or full section re-open — to be confirmed before UI development.\] |

#### **3.2.5 M12 — SLA Management**

*Applies primarily to M09 (department clearances); M08's BREDA review SLA follows the same pause/resume principle*

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M12-001 | System | Define SLA per activity (department/clearance service), each with an independent timer |
| REQ-M12-002 | System | Start the SLA timer when the concerned authority receives the application/clearance |
| REQ-M12-003 | System | Run all SLA timers independently — no enforced sequence between them |
| REQ-M12-004 | System | Pause the SLA timer when a query is raised; resume from remaining time on applicant response (no minimum window) |
| REQ-M12-005 | System | Auto-escalate on SLA breach — notify department nodal officer and BREDA Admin via SMS/email |
| REQ-M12-006 | System | Show SLA breach status on the BREDA Admin Dashboard (department-wise pending clearances) |
| REQ-M12-007 | System Admin | SLA values configurable via Admin Console (M14) |

## **3.3 Layer 3 — Platform & Admin**

#### **3.3.1 M13 — Notifications & Communication**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M13-001 | System | Maintain an event → recipient → channel matrix (registration confirmed, application submitted, payment confirmed, query raised/responded, approved/rejected, SLA breach, COD alert) covering SMS, email, in-app (WhatsApp deferred) |
| REQ-M13-002 | System | Send notifications via SMS and Email gateways (Phase 1 mandatory) |
| REQ-M13-003 | System | Show in-app notifications with badge count on the applicant dashboard |
| REQ-M13-004 | System | Show toast notifications for key UI actions (save, submit, payment success, query raised, query responded) |
| REQ-M13-005 | System | Notify department nodal officers via email/SMS with a direct portal link on each routing/submission event |
| REQ-M13-006 | Applicant | View the notification log per application |

#### **3.3.2 M14 — Admin Console & Configuration**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M14-001 | System Admin | Create, edit, activate, deactivate, and delete user accounts |
| REQ-M14-002 | System Admin | Assign roles to users (applicant, breda\_admin, system\_admin, dept\_officer, finance\_officer).  |
| REQ-M14-003 | BREDA Admin | Create department user accounts with activation link (no self-registration for dept users) |
| REQ-M14-004 | System Admin | Configure the fee table (slabs by capacity); changes apply to future applications only |
| REQ-M14-005 | System Admin | Manage master data — Districts, Blocks, Panchayats, Villages, DISCOM list, PSS/GSS reference data |
| REQ-M14-006 | System Admin | Configure SLA per department/clearance service |
| REQ-M14-007 | BREDA Admin / System Admin | View audit log — logins/logouts, data changes, approvals, rejections, all time-stamped |
| REQ-M14-008 | System Admin | Reset passwords and unlock accounts |

#### **3.3.3 M15 — MIS & Reporting**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M15-001 | BREDA Admin | View KPI row — Total Applications (this month), Pending BREDA Review, Pending Dept Clearances, Approved This Month, Rejected |
| REQ-M15-002 | BREDA Admin | View bar chart — Applications by Month (last 6 months) |
| REQ-M15-003 | BREDA Admin | View pie chart — Project Type Distribution |
| REQ-M15-004 | BREDA Admin | View horizontal bar chart — Department-wise Pending Clearances |
| REQ-M15-005 | BREDA Admin | View applications table — File No., Applicant, Capacity, Type, DISCOM, Status, Submitted, Actions |
| REQ-M15-006 | BREDA Admin | Export reports in PDF, Excel, CSV formats |
| REQ-M15-007 | System | Provide role-based dashboards for Applicant, BREDA Admin, Department Officer |
| REQ-M15-008 | BREDA Admin | Generate MIS reports — approvals, timelines, capacity addition, SLA compliance, and rejections per department — with KPIs (average approval time, SLA compliance rate, rejections per department), exportable per REQ-M15-006 |

#### **3.3.4 M16 — Helpdesk, Grievance & Appeal Management**

| REQ ID | Role | Requirement |
| :---- | :---- | :---- |
| REQ-M16-001 | Applicant / Officer | Raise helpdesk tickets, grievances, and appeals via the portal |
| REQ-M16-002 | System | Auto-generate a unique reference number for every ticket |
| REQ-M16-003 | System | Support an escalation matrix with defined tiers |
| REQ-M16-004 | BREDA Admin | Track resolution status and update tickets |
| REQ-M16-005 | Applicant | File a grievance against a rejection decision |

#### **3.3.5 M17 — Security, Audit & Compliance**

* See Section 7 (Non-Functional Requirements) — no separate REQ table here

# **4\. Role and Form Based Workflows**

## **4.1 Registration & Login Flow**

![][image3]

1. Applicant opens portal, selects Register

2. Step 1 — basic details entered

3. **Decision** — OTP verification, mobile and email, both required:

   * a. Mobile OTP verified and Email OTP verified → continue to step 4

   * b. Mobile OTP not received/expired → resend mobile OTP → retry that part of step 3

   * c. Email OTP not received/expired → resend email OTP → retry that part of step 3

4. Step 2 — communication address entered

5. Account created; User ID \= email

6. Confirmation sent via SMS/email

7. **Decision** — Login:

   * a. Valid credentials → logged in

   * b. Invalid credentials → error shown → retry step 7

   * c. Forgot password → reset-password flow → return to step 7

## **4.2 Project Application Submission Flow (Step 0 → Section F, payment)**

![][image4]

1. Applicant starts a new application

2. Step 0 — capacity & purpose

3. Section A — profile, addresses, contact details

4. Section B — technical & financial details

5. Section C — land availability

6. Section D — document uploads

7. **Decision** — applicant action (auto-save runs throughout):

   * a. Save as Draft → application stays DRAFT, resumable via "Continue" → flow paused here

   * b. Continue → proceed to step 8

8. Section E — declaration accepted, Print Preview available

9. "Proceed to Payment" enabled, Step 0 – D complete and Section E ticked

10. **Decision** — Payment method, Section F:

    * a. Online → GRAS/e-Treasury:

      * i. Success → Transaction ID \+ Acknowledgement Number generated, status SUBMITTED\_TO\_BREDA → step 11

      * ii. Failure → status remains PAYMENT\_PENDING → retry step 10a

      * b. Offline → status PAYMENT\_VERIFICATION\_PENDING → go to Flow 4.3

11. Form becomes read-only for applicant

## **4.3 Offline Payment & Finance Verification Flow**

![][image5]

*Entered from Flow 4.2, step 10b*

1. Payment made outside the portal; cheque/DD copy uploaded / number entered → status PAYMENT\_VERIFICATION\_PENDING

2. **Decision** — Finance Officer / BREDA Admin verification:

   * a. Verified → Acknowledgement Number generated, status → SUBMITTED\_TO\_BREDA → go to Flow 4.4

   * b. Rejected → applicant notified, asked to re-upload → return to step 1

## **4.4 BREDA Query — Resubmission Loop**

![][image6]

*Entered once application status \= SUBMITTED\_TO\_BREDA*

1. BREDA Admin reviews the application; SLA timer running (15 days)

2. **Decision** — BREDA Admin's outcome:

   * a. Accept → status BREDA\_APPROVED, "Apply for Clearances" enabled → go to Flow 4.5

   * b. Reject → status REJECTED → flow ends (fees is non-refundable)

   * c. Raise query (field-level/general) → SLA timer pauses → continue to step 3

3. Applicant notified via SMS/email/in-app

4. Applicant corrects fields, re-uploads documents, and/or replies, then resubmits; SLA timer resumes

5. Return to step 1 (every cycle time-stamped)

## **4.5 Apply for Clearances Flow (per department/service)**

![][image7]

*Entered from Flow 4.4, step 2a*

1. "Apply for Clearances" action appears on applicant dashboard

2. Applicant selects clearance services across department cards

3. Service-specific fields/documents shown per selection

4. Offline fee proof / receipt number entered /  uploaded per clearance where applicable

5. Clearance submitted directly to concerned department officer; BREDA notified if needed

6. Department nodal officer notified with portal link

7. SLA timer starts for that clearance

8. **Decision** — Department officer's outcome, per clearance:

   * a. Approve → that clearance \= APPROVED

   * b. Reject → that clearance \= REJECTED; applicant may reapply separately

   * c. Raise query → go to Flow 4.6 → returns to this decision on resubmission

9. **Decision** — once all selected clearances reach a final decision:

   * a. All approved → overall status APPROVED, consolidated clearance status issued → go to Flow 4.7

   * b. One or more rejected → overall status remains UNDER\_DEPARTMENT\_REVIEW for the remaining clearances; rejected clearance(s) handled per step 8b

## **4.6 Department Query — Resubmission Loop**

![][image8]

*Entered from Flow 4.5, step 8c*

1. Department officer raises a query; SLA timer for that clearance pauses

2. Applicant notified via SMS/email/in-app

3. Applicant corrects fields, re-uploads documents, and/or replies, then resubmits; SLA timer resumes

4. **Decision** — Department officer reviews again:

   * a. Approve → return to Flow 4.5, step 8a

   * b. Reject → return to Flow 4.5, step 8b

   * c. Raise another query → return to step 1

5. Every cycle time-stamped in audit trail

## **4.7 Commissioning → COD Verification → Registration Number/Certificate Flow**

![][image9]

*Precondition: all submitted clearances approved*

1. Applicant submits commissioning details

2. **Decision** — COD entry timing:

   * a. COD entered on/before expected date → continue to step 3

   * b. Expected date passed, COD not entered → system flags and escalates via alert only; application remains COD\_PENDING indefinitely — no auto-cancellation

3. Applicant enters actual COD and uploads COD Certificate

4. **Decision** — BREDA Admin verification:

   * a. Verified → Registration Number generated and Registration Certificate issued → flow ends

   * b. Discrepancy found → applicant notified for correction → return to step 3

# **5\. Data Requirements**

## **5.1 Master Data**

| Entity | Notes |
| :---- | :---- |
| Districts, Blocks, Panchayats, Villages | Cascading dropdowns — Communication & Project Address |
| PSS/GSS Stations | Section B — auto-ruled by capacity (PSS \< 10 MW, GSS ≥ 10 MW) |
| Departments & Role Labels | 15 departments; used in Apply for Clearances (M09) |
| Clearance Services \+ Default SLA Days | Admin-configurable (M14); see Appendix C |
| Fee Slabs | Capacity-based; admin-configurable (M14); changes apply to future applications only |
| DISCOM List | NBPDCL / SBPDCL — fixed, 2 options |
| User Roles & Permissions | See Roles & Access Overview (2.2) |
| Notification Event Matrix / Templates | Admin-configurable (M14); event → recipient → channel mapping (M13) |
| Users | Account, profile, role assignment — applicants, BREDA staff, department officers |

## **5.2 Transaction Data**

| Entity | What's Captured | Notes |
| :---- | :---- | :---- |
| Applications | Step 0 – Section F data | One per project registration |
| Documents | Category, file metadata, version | Versioned for resubmissions |
| Payments | Method, amount, status, verification | Online/offline |
| Application Clearances | Per-service status, SLA timestamps | One row per selected clearance service |
| Queries & Responses | Query type, flagged fields, response text | BREDA \+ department |
| Notifications | Event, channel, status | SMS / email / in-app |
| Audit Log | Actor, action, before/after values, timestamp | Immutable |

# **6\. External Interface Requirements**

## **6.1 Software Interfaces**

| Interface | Purpose | Pattern |
| :---- | :---- | :---- |
| GRAS / e-Treasury | Online registration & clearance fee payment | Synchronous (gateway redirect \+ callback) |
| SMS Gateway | OTP, status/event notifications | Async queue |
| Email Gateway | OTP, status/event notifications, acknowledgements | Async queue |
| Department APIs (15 departments) | Clearance submission & status updates | Async — push to department, webhook or polling for response |
| WhatsApp Gateway | Department nodal officer alerts | Deferred (Optional / Post Go-Live) |

## **6.2 User Interface Requirements (high-level)**

* Responsive — desktop, tablet, and mobile

* Skip-to-content link, accessibility controls (font size, high-contrast)

* English only

# **7\. Non-Functional Requirements**

## **7.1 Performance**

* Respond to user actions within 3 seconds for at least 99.9% of transactions under normal load

* Support a minimum of 100 concurrent users without performance degradation

* Architecture horizontally scalable for growth in applicants, projects, departments, and integrations

## **7.2 Availability & Reliability**

* Minimum 99.9% uptime (excluding scheduled maintenance)

* Scheduled downtime communicated in advance, preferably outside business hours

## **7.3 Security**

*(covers M17 — Security, Audit & Compliance)*

* Role-Based Access Control — users access only functions/data relevant to their role (applicant, breda\_admin, system\_admin, dept\_officer, finance\_officer)

* Encryption at rest and in transit (industry-standard)

* Audit trail — logins/logouts, data create/modify/delete, approvals/rejections, all time-stamped and attributable

* Protection against OWASP Top 10 — SQL injection, XSS, CSRF, broken authentication

* Sensitive fields (mobile number, email, etc.) masked in system logs and non-production environments

* Valid SSL certificate installed and maintained throughout portal operation

## **7.4 Compliance**

* GIGW guidelines

* WCAG 2.1 AA accessibility

* CERT-In Audit — Safe-to-Host certification before Go-Live, and annually during O\&M

## **7.5 Usability & Accessibility**

* English only; UI suitable for users with varying digital literacy (developers/investors and government officers alike)

* Responsive across desktops, tablets, and mobile devices

## **7.6 Maintainability**

* Documentation handover — source code, SRS, user manuals kept current

* Version control and change management process for all updates

* Historical versions of submitted documents/DPRs preserved for audit and reference

# **8\. Hosting and Deployment Environment**

## **8.1 Hosting & Infrastructure**

* Primary hosting at BREDA Data Centre, with Disaster Recovery (DR) site

* BREDA provides: rack space, cooling, UPS power, firewall, Anti-DDoS services

* SI manages: application deployment, configuration, storage, and ongoing maintenance

* High-availability deployment at the primary DC (no single point of downtime for core services)

* Separate staging environment for testing — develop locally → staging → production; no testing on production

* Backup — operating system, database, and application backed up as per guidelines, with monitoring of backup jobs

* SSL/HTTPS enforced for all client-server communication

## **8.2 Operations & Maintenance (O\&M)**

* O\&M period: 60 months from Go-Live (per BREDA RFP payment schedule)

* SI responsible for application support, bug fixes, patches, and infrastructure support at no additional cost during this period

* Annual CERT-In security audit and Safe-to-Host re-certification during O\&M

* Indicative support SLA (severity-based; to be finalized with BREDA):

| Severity | Example | Response Time | Resolution Time |
| :---- | :---- | :---- | :---- |
| Critical | Portal down, payment failure | 1 hour | 4 hours |
| High | Major feature broken (e.g. clearance submission fails) | 4 hours | 1 business day |
| Medium | Minor feature issue, workaround available | 1 business day | 3 business days |
| Low | Cosmetic/UI issue, documentation | 2 business days | Next release cycle |

# **9\. Acceptance Criteria**

| Layer | Acceptance Criteria |
| :---- | :---- |
| Layer 1 — Applicant-Facing | End-to-end application submission (Step 0 – Section F), document upload, and both online/offline payment flows verified via UAT; acknowledgement generated correctly in each case |
| Layer 2 — Workflow & Processing | BREDA review, query loops, clearance submission/review, SLA pause-resume, and commissioning flows verified end-to-end across all 15 departments |
| Layer 3 — Platform & Admin | Admin console (user/role/master data/fee/SLA config), MIS dashboards, notifications, helpdesk, and audit log verified |

**Overall sign-off conditions**

* SRS reviewed and signed off by BREDA as the agreed baseline before development begins

* CERT-In Safe-to-Host certificate obtained (security audit)

* Go-Live approval from BREDA following successful UAT

# **Appendix A — Forms & Field Inventory**

**Step 0 — Capacity & Purpose**

| Field | Type | Options / Values | Mandatory? |
| :---- | :---- | :---- | :---- |
| Plant Capacity | Numeric | — | Yes |
| Capacity Unit | Radio | kW, MW | Yes |
| Purpose of Project | Radio | Captive/Individual/Domestic Use, Open Access, Third Party Sale, PPA with DISCOM, Other | Yes |
| Purpose — Other (free text) | Text | — | If "Other" selected |

**Section A — General Profile of Applicant**

| Field | Type | Options / Values | Mandatory? |
| :---- | :---- | :---- | :---- |
| Name of Applicant | Text (read-only, auto-filled) | — | — |
| Type of Applicant | Text (read-only, auto-filled) | Individual, Proprietary Firm, Partnership, Public Ltd., Pvt. Ltd., Other (set at registration) | — |
| Communication Address — Street/House No. | Text | — | Yes |
| Communication Address — Village | Dropdown | From master data | Yes |
| Communication Address — City | Dropdown | From master data | No |
| Communication Address — District / Block / Panchayat | Dropdown (cascading) | From master data | Yes |
| Communication Address — Pin Code | Text | — | Yes |
| Project Address — "Same as Communication Address" | Checkbox | — | No |
| Project Address — Street/House No., Village, City, District, Block, Pin Code | Text/Dropdown | Village/District/Block from master data | Yes (City optional) |
| Project Address — Latitude / Longitude | Numeric | — | Yes |
| Mobile No. | Text (view-only, pre-filled) | — | — |
| Email | Text (view-only, pre-filled) | — | — |
| Alternate Mobile No. | Text | — | No |
| Alternate Email | Text | — | No |
| PAN Number | Text | — | Yes |
| Aadhaar Number | Text | — | Yes |
| GST Number | Text | — | Mandatory if applicable |

**Section B — Technical & Financial Details**

| Field | Type | Options / Values | Mandatory? |
| :---- | :---- | :---- | :---- |
| Power Plant Type — Step 1 | Radio | Solar, Non-Solar | Yes |
| Power Plant Sub-type (if Solar) | Dropdown | Rooftop, Ground Mounted, Floating, Canal Bank/Top, Others | If Solar |
| Storage (if Solar) | Toggle | With, Without | If Solar |
| Power Plant Sub-type (if Non-Solar) | Dropdown | Bio, Wind, Waste to Energy, Geothermal, Hydro, Others | If Non-Solar |
| DISCOM | Dropdown | NBPDCL, SBPDCL | Yes |
| Name of PSS/GSS | Dropdown | From PSS/GSS master data (auto-ruled PSS/GSS) | Yes |
| Distance to PSS/GSS (km) | Numeric | — | Yes |
| Type of Power Evacuation | Radio | LILO, Direct Evacuation, Other | Yes |
| Evacuation — Other (free text) | Text | — | If "Other" selected |
| Proposed Date of Commissioning | Date | — | Yes (mandatory, editable) |
| Approximate Project Cost (Rs.) | Numeric | — | Yes |
| EPC Contractor — Name, Address, Contact Person, Mobile, Email | Text | — | Yes (all 5\) |

**Section C — Land Availability**

| Field | Type | Options / Values | Mandatory? | Max Size |
| :---- | :---- | :---- | :---- | :---- |
| Land Ownership Document Type | Dropdown | Copy of House Tax Bill, Property Card, Rent/Lease Agreement or NOC of Society, Building Use (BU) Permission | Yes | — |
| Upload self-attested copy | File upload | — | Yes | 5 MB |
| Land Category | Radio | Private, Government | Yes | — |
| Land/Rooftop Area (acres) | Numeric | — | Yes | — |

*Note: document type list is rooftop-biased — see OD-03 for conditional sets per project type.*

**Section D — Document Uploads**

| Document | Mandatory? | Max Size |
| :---- | :---- | :---- |
| Detailed Project Report (DPR) | Yes | 25 MB |
| Firm's Registration Certificate | Yes | 5 MB |
| Startup India / Udyam / NSIC / Industry Certificate | Yes | 5 MB |
| GST Certificate | Yes | 5 MB |
| PAN Detail | Yes | 5 MB |
| Aadhaar Detail | Yes | 5 MB |
| Audited Balance Sheet | No | 5 MB |
| Income Tax Return Certificate | Yes | 5 MB |
| Power Purchase Agreement (PPA) | If Purpose \= PPA with DISCOM | 5 MB |
| Custom Documents (label \+ upload, repeatable) | No | 5 MB |

*Scanned images (where a document is a photo/scan rather than a native PDF): 2 MB, across all of the above.*

**Section E — Declaration**

| Field | Type | Mandatory? |
| :---- | :---- | :---- |
| Declaration text (auto-filled name/org) | Read-only | — |
| Confirmation checkbox | Checkbox | Yes |
| Print Preview | Button | — |

**Section F — Registration Fees & Payment**

| Field | Type | Mandatory? | Max Size |
| :---- | :---- | :---- | :---- |
| Payment Method (Online/Offline) | Toggle | Yes | — |
| Online — Pay Now | Button | — | — |
| Offline — Upload DD/cheque Copy | File upload | Yes, for offline | 5 MB |

# **Appendix B — Glossary**

| Term | Meaning |
| :---- | :---- |
| BREDA | Bihar Renewable Energy Development Agency |
| DISCOM | Distribution Company (NBPDCL or SBPDCL) |
| NBPDCL | North Bihar Power Distribution Company Limited |
| SBPDCL | South Bihar Power Distribution Company Limited |
| BSPTCL / STU | Bihar State Power Transmission Company Limited (State Transmission Utility) |
| SLDC | State Load Dispatch Centre |
| BSPCB | Bihar State Pollution Control Board |
| MRT | Metering-related testing unit under NBPDCL/SBPDCL |
| COD | Commercial Operation Date (commissioning date) |
| GRAS | Government Receipt Accounting System (e-Treasury) |
| PSS / GSS | Power Sub-Station / Grid Sub-Station |
| LILO | Line In Line Out (a power evacuation method) |
| SLA | Service Level Agreement |
| OD | Open Decision (tracked in the Queries section) |
| Acknowledgement Number | Generated on payment confirmation |
| Registration Number | Generated after commissioning and COD verification |
| GIGW | Government of India Guidelines for Websites |

# **Appendix C — SLA Reference Table**

| \# | Activity | Concerned Authority | SLA (Days) |
| :---- | :---- | :---- | :---- |
| 1 | User Application Registration | BREDA | Same day |
| 2 | Project Registration Review | BREDA | 15 |
| 3 | Grid Connectivity | DISCOMs / BSPTCL | 30 |
| 4 | Plan Approval | Electrical Inspector Office | 4 |
| 5 | Metering Specification Approval | DISCOMs | 13 |
| 6 | Startup Power Approval | DISCOMs | 15 |
| 7 | Charging Permission | Electrical Inspector Office | 10 |
| 8 | NOC for Special Energy Meter | DISCOMs (MRT) | 7 |
| 9 | Installation of SEM Meter | DISCOMs | 7 |
| 10 | Final Grid Connectivity Approval | BSPTCL | 120 |
| 11 | Synchronization Permission | SLDC | 7 |
| 12 | Permission to Commission (PTC) | DISCOMs | 7 |
| 13 | Project Commissioning by field office | DISCOMs | 3 |
| 14 | Pollution Clearance | BSPCB | 15 |
| 15 | Environment Clearance | Environment & Forest Dept. | 30\* |
| 16 | State Goods and Service Tax | Dept. of Commercial Taxes, GoB | 30 |
| 17 | NOC for Canal / Reservoir / Pond | Water Resource Dept. / PHED | 75 |
| 18 | NOC for Fisheries Pond | Animal & Fisheries Resources Dept. | 30 |
| 19 | Change of Land Use / Certificate of Land Use | Land & Revenue Dept. | 60 |
| 20 | Category of Project | Industry Dept. | 30 |

*Environment Clearance SLA marked with asterisk in source — conditions to be confirmed.*

# **Appendix D — Acknowledgement & Registration Certificate Formats**

**Acknowledgement Format**

*Acknowledgement No.: \_\_\_\_\_\_\_ Date: \_\_\_\_\_\_\_*

***Acknowledgement for RE-Power Plant Registration***

*The application form for setting/Registration of \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ Renewable Energy Project \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ has been submitted under the provisions of the Bihar Policy for Promotion for New and Renewable Energy Sources, 2025\. The application charge has also been submitted in the form of Demand Draft/Banker's Cheque drawn on \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ Bank, \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ Branch, bearing DD/BC No. \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ dated \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ in the name of Bihar Renewable Energy Development Fund. Registration Certificate will be issued after final commissioning of the Renewable Energy Project, subject to submission of approval/NoC from State DISCOMs.*

*Project Director, BREDA*

**Registration Certificate Format**

*Government of Bihar*

*Bihar Renewable Energy Development Agency (BREDA)*

*(A Govt. Agency under Energy Department)*

*2nd Floor, Vidyut Bhawan-II, Bailey Road, Patna-800021*

*Website: breda.co.in | Email: breda.gov@gmail.com | Tel: 0612-2505734, 1800-345-6204*

*Reg No.: BREDA/SPV/Power Plant/\_\_\_\_\_\_\_/\_\_\_\_\_\_\_  Issue Date: \_\_\_\_\_\_\_*

***CERTIFICATE OF REGISTRATION***

*This is to certify that the \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ Renewable Energy Project of Capacity \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ has been registered under the provisions of the Bihar Policy for Promotion of New and Renewable Energy Sources, 2025\. This Certificate is presented to \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_.*

*Project Director, BREDA*

*Note: the Acknowledgement Number format is Ack/\<year\>/\<sequence\> (e.g. Ack/2025/001) — OD-01 resolved. The Registration Number follows the “Reg No.” pattern shown above (BREDA/SPV/Power Plant/\<sequence\>/\<year\>).*

# **Queries & Decisions Required (BREDA / Department Input Needed)**

*Resolved items include the clarified resolution for department reference. Open items require further confirmation from BREDA or departments before development.*

**Resolved & Clarified Items**

| \# | Topic | Clarification / Resolution |
| :---- | :---- | :---- |
| OD-01 | Acknowledgement Number format | RESOLVED: Format confirmed as Ack/\<year\>/\<sequence number\> (e.g. Ack/2025/001). See Appendix D. |
| OD-02 | Finance Officer role | RESOLVED: Finance Officer is a confirmed separate role for offline payment verification. Not merged with BREDA Admin. |
| OD-03 | Conditional land ownership documents | RESOLVED: Conditional document sets per project type (Rooftop / Ground Mounted / Floating / Canal / Water Body) are not required. A single document set applies across all project types. |
| Q-01 | Post-rejection resubmission | RESOLVED: The same applicant cannot file a fresh application for the same cancelled or rejected project. A different applicant may apply for the same project. No fee carryforward — a new application requires a new fee payment. |
| Q-02 | COD cancellation | RESOLVED: Non-entry of actual COD does not auto-cancel the project. Status moves to COD\_PENDING with alert-only escalation. No SLA applies to the applicant for COD entry. Proposed COD is mandatory at application stage (see REQ-M03-010). |
| Q-03 | Section B ↔ DISCOM pre-filter | RESOLVED: DISCOM selected in Section B pre-filters the DISCOM department card in the Apply for Clearances screen. |
| Q-04 | GST on processing fees | RESOLVED: GST at 18% is applicable on the registration/processing fee (see REQ-M05-002). |
| Q-05 | Post-submission change requests (confirmed part) | RESOLVED (partial): Change Request option is confirmed for applicants. No change request is permitted once the Acknowledgement Number is generated (see REQ-M03-004, REQ-M08-010). Remaining open items moved to Open Items below. |
| Q-07 | DPR file size / sample data | RESOLVED: DPR maximum file size is 25 MB (updated in REQ-M04-004 and Appendix A). Real/sample applications  needed from BREDA for development and testing. |
| Q-08 | Refund on rejection | RESOLVED: Registration/processing fee is non-refundable. No refund is issued upon application rejection at any stage after payment confirmation (see REQ-M05-008). |
| Q-10 | No response to a query | RESOLVED: If the applicant never responds to a BREDA or department query, the application/clearance does not auto-reject. It remains open indefinitely until a response is received or BREDA/department takes a manual action. |
| Q-11 | Voluntary withdrawal / BREDA cancellation | RESOLVED: Applicant cannot voluntarily withdraw or cancel an application once the registration fee has been paid (see REQ-M05-009). BREDA Admin can cancel or reject a submitted application at any point in time (see REQ-M08-006). |
| Q-12 | Change request — scope & clearance impact | RESOLVED: No change request will be entertained once BREDA has approved the project.  |

**Open Items — Pending Confirmation**

| \# | Topic | Open Question / Pending Input |
| :---- | :---- | :---- |
| Q-01 | Change request — scope & clearance impact | OPEN: Scope of permissible change request fields during clearance application |
| Q-02 | Department-specific clearance fields & documents | OPEN: Field-level requirements and document lists per clearance service, per department (required for M09 — Apply for Clearances) have not yet been received from departments. |
| Q-03 | Draft abandonment | OPEN: Is there a retention / cleanup policy for applications left in DRAFT status indefinitely (e.g. auto-archive after X months)? |
| Q-04 | Query response — field-level vs. full section | OPEN: Whether a query response allows the applicant to edit only the flagged fields (field-level unlock) or re-open the entire section — pending BREDA confirmation before UI development. |
