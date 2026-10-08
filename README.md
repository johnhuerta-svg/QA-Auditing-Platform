# Universal Quality Assurance & Dynamic Web App Database Schema 
#QA-Auditing-Platform
An enterprise-grade SQL Server compliance architecture built as a portfolio project to showcase advanced system design, zero-trust security, cryptographic integrity, and AI-assisted engineering.
Schema Version: 3.0.0-UNIVERSAL | Classification: ENTERPRISE TEMPLATE
Target Platform: SQL Server 2022+ (Enforces Enterprise OAuth2 / Federated Identity & Audit Logging)
📌 Project Overview
The Universal QA & Dynamic Web App Database Schema is an enterprise-grade SQL Server database architecture designed for high-compliance environments. It provides complete data isolation, federated identity management (Entra ID / OAuth2), statutory audit compliance (FTI, PHI, PII, 42 CFR Part 2), cryptographic data integrity validation (SHA-256 signatures), automated scoring lockdowns, multi-tier organizational scorecards, associate development tracking, error heatmaps, and structured rebuttal workflows.

🚀 Key Features & Capabilities
Federated Identity & Access Control (RBAC): Integrated with enterprise OAuth2 / Entra ID with 15 granular bitwise permission bits and AccessLevel tiering (1-5).
Immutable System & Access Audit Logging: Tracks all data modifications (INSERT, UPDATE, DELETE, SELECT, EXPORT, BREAK_GLASS) with automated statutory purge eligibility windows (6 to 7 years) and triggers preventing physical log deletion.
Break-Glass Emergency Protocol Engine: Controlled emergency override mechanism with 24-hour SLA review windows and supervisor webhook notifications.
Cryptographic Score Lockdown & Integrity Verification: SHA-256 cryptographic hashing (HASHBYTES) guarantees post-release immutability for QA evaluation scores unless authorized break-glass sessions are initiated.
Organizational Hierarchy & Multi-Level Scorecards: Hierarchical tracking across Departments, Units, and Caseworkers with dedicated reporting views for caseworkers, QA analysts, and department leads.
Granular Error Taxonomy & Heatmap Analysis: Sub-category scoring, severity weighting (CRITICAL, MAJOR, MINOR, INFORMATIONAL), and procedural heatmap aggregation.
End-to-End Rebuttal & Dispute Workflow: Structured rebuttal handling with status transitions (DRAFT → SUBMITTED_TO_ASSOCIATE → REBUTTAL_PENDING → UNDER_QA_REVIEW → FINALIZED → RELEASED) and discussion threading between agency personnel and QA analysts.
Associate Coaching & Development Tracking: Tracks coaching action plans, targeted error categories, and completion dates.
🏗️ Architecture & Database Structure
1. Identity, Federation, & Access Control
Roles: Access tier mapping, permission bitwise flags, and Identity Group Object GUIDs.
Users: Federated login tracking, subject identifiers (IdentityUserGUID), employee account IDs, dynamic data masking, and session state.
2. Audit & Compliance Logging
SystemAuditLog: Full state change tracking (JSON snapshots) categorized by sensitivity (FTI, PHI, MHI, 42CFR, PII, STANDARD).
FTI_AccessLog: Granular query parameter logging and business justification tracking for sensitive data access.
BreakGlassLog: Emergency override requests with mandatory justification and immutable 24-hour review SLA.
AuditScoreHistory: Historical audit trail for post-submission evaluation score alterations.
3. Core Quality Assurance & Cryptographic Integrity
AuditReviews: Evaluation scores (Cat1_Score, Cat2_Score, Cat3_Score), pass/fail logic (≥ 96.80%), double jeopardy flags, data sensitivity tags, and SHA-256 cryptographic signatures (FinalizedHash).
SchemaVersionLog: Semantic version control script deployment and rollback tracking.
4. Immutability & Safety Triggers
tr_PreventAuditTampering_SystemLog & tr_PreventAuditTampering_FTILog: INSTEAD OF DELETE triggers blocking physical erasure of system logs.
tr_AuditReviews_ScoringControl: AFTER UPDATE trigger locking score modifications post-release unless an approved Break-Glass session exists, and auto-generating SHA-256 state signatures.
5. Enterprise Interfaces & Stored Procedures
sp_SubmitRebuttal: Enforces a strict 5-day post-release dispute window.
sp_VerifyAuditIntegrity: Validates record state against calculated SHA-256 signatures to detect out-of-band tampering.
sp_PurgeExpiredData: Automated dry-run and transaction-safe soft purge based on statutory retention expiration.
6. Role-Based Segregated Reporting Views
vw_AuditReviews_Caseworker: Restricted view for caseworker score lookup.
vw_AuditReviews_QATech: Complete processing analysis view for QA technical analysts.
vw_AuditReviews_Supervisor: Management view including sensitivity indicators and retention expiration.
7. Dynamic Web App & Quality Auditing Schema
Organizational Structure: Departments, Units, UserOrgAssignments.
Error Taxonomy Engine: ErrorCategories, ErrorSubCategories, ErrorElements.
Audit Execution & Line Items: AuditReviewHeader, AuditErrorDetails.
Rebuttal Workflow & Dialogue Threading: Rebuttals, RebuttalThread.
Associate Coaching: AssociateDevelopmentPlans.
Scorecards & Heatmaps: vw_Caseworker_MonthlyScorecard, vw_Unit_MonthlyScorecard, vw_Department_MonthlyScorecard, vw_ErrorTrends_HeatmapData, sp_GetErrorHeatmap, sp_SubmitAssociateRebuttal.
⚙️ Deployment & Middleware Integration
System Requirements
Platform: SQL Server 2022+ Enterprise or Developer Edition.
Authentication: Enterprise Identity Provider / Entra ID (OAuth2 JWT tokens).
Initialization & Session Context Command
Before issuing application requests, initialize the session context within the database connection middleware:l
EXEC sp_set_session_context @key = N'IDENTITY_USER_GUID', @value = @InboundUserGUIDClaim;


## 🔒 Security & Data Retention Rules
* **PHI / PII Retention:** Retained for 6 years from log creation (`DATEADD(year, 6, LogDateTime)`).
* **FTI / Security / Audit Retention:** Retained for 7 years from log creation (`DATEADD(year, 7, LogDateTime)`).
* **Standard Data Retention:** Retained for 3 years.
* **Data Masking:** Phone numbers masked via SQL Server Dynamic Data Masking (`partial(0,"XXX-XXX-",4)`).


