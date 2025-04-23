Thanks for sharing both the images — they give a strong overall architecture of your new project. Let’s break them down in detail so you can explain it well in interviews or to your team.


---

1. Image 1: GTPS CASHFIT Pricing Invoicing and Billing – Simplified View

Overview:

This architecture represents the end-to-end pipeline for trade-related fee calculation, billing, and invoicing across SG Markets (Société Générale’s trading systems).


---

Key Components & Flow:

1. Products & Services Catalog (Referential)

Acts as a source for pricing and service configuration.

Feeds into the pricing and contract negotiation platform.


2. Contract Negotiation Platform

Handles:

Offers and pricing logic.

Contract life cycle.

Client preferences (like discounts, fee arrangements).


Two main flows: Standard Pricing and Negotiated Pricing.


3. Billing Enrich & Transformer Gateway

Transforms trade-related data to enrich it with additional information needed for fee resolution.


4. New FeesOne

The core billing engine.

Steps involved:

Fee Computation

VAT Computation

Fee Validation

Invoice Preparation

Invoice Data Generation

Fee Manager (Workflow + Audit Trail)

Accounting/Aggregation



5. Booking Layer

Books the finalized invoice/fee into the accounting layer and RDI (Accounting tool).


6. Datahub Reporting

Produces client profitability reports, invoice summaries, and tracks fee reconciliation.



---

2. Image 2: FEESONE (Payable Fee) – Technical Architecture

Overview:

This is the technical deployment-level architecture of the FeesOne platform. It involves API interactions, microservices, cloud/on-premise integrations, and external data feeds.


---

Core Modules:

Backend Services (Azure):

Built using Java, Microservices, and PostgreSQL.

Code is managed using GitHub, deployed in AKS (Azure Kubernetes Service).


Microservices:

Trade Service: Handles trade details ingestion and lifecycle.

Invoice Service: Responsible for generating and managing invoices.

Cash Allocation Service: Distributes payable fees appropriately.

Accounting Service: Generates and maintains accounting entries.



---

Inputs:

DPAs (Data Providers) like ELIOT, TPS, EOLE: These push trade data into the Data Lake.

GUI Platforms (SCIO, SG-DOCS, SIROCCO): UI systems where trades are managed and booked.

Fee Schedule & Computation systems for calculating payable fees.



---

External Integrations:

SRG (Payments)

RDI (Accounting Rules)

GL (General Ledger)

NXG (Reconciliation)

DUCO, eRECS for cash breaks and reconciliation

Uses FTR/API, Azure Gateway, and On-Prem APIs for communication.



---

Highlights to Remember:

1. Hexagonal architecture is implicitly used — services are separated cleanly by domain (Trade, Invoice, Fee Mgmt, Accounting), and integrated through APIs.


2. FeesOne plays a central role — manages all computations, rules, billing, and traceability.


3. It supports both automated and manual pricing (contract-based, offer-based).


4. You deal with multi-source trade ingestion, fee rule application, and reporting back to client profitability and invoice generation.


5. Security & audit are embedded in every major step (very important for investment banking).


6. Modular microservices help in scalability and managing each component independently.




---

Would you like:

A summarized paragraph you can use in your intro during onboarding?

A diagram or technical doc to explain it better in interviews?

A sample microservice class for one of these modules (like TradeService or FeeComputationService)?


Let me know how you'd like to proceed!

Here is the full end-to-end architectural document for your GTPS CASHFIT Pricing, Invoicing, and Billing System based on the shared images and your inputs:


---

GTPS CASHFIT Architecture – Full End-to-End Overview

1. Introduction

GTPS CASHFIT is a core system used in investment banking by Société Générale. It manages pricing, invoicing, and billing for financial trades, including broker and client fee calculations, across multiple markets (e.g., US, Paris). It integrates with various external/internal systems and is responsible for accurate invoice generation, trade reconciliation, fee schedules, and reporting.


---

2. Core Business Objective

Book trades for clients via brokers.

Calculate and manage various fees (e.g., brokerage, provider, trade-specific).

Generate accurate and timely invoices.

Reconcile financial transactions.

Provide audit trail and traceability for all billing operations.



---

3. Key Domains and Services

Here are the core services/modules as per the technical architecture:

a. Trade Management Service

Handles trade ingestion (from internal SG systems like ELIOT, TPS, etc.).

Supports trade validation and classification.

Stores trade records with metadata.

Integrates with provider and SG market trades via Azure Gateway.


b. Invoice Management Service

Generates invoices based on computed fees.

Handles invoice formatting, grouping, and client/broker splits.

Sends final invoices to downstream systems like DUCO and eRecs.


c. Cash Allocation Service

Responsible for matching payments against invoices.

Interfaces with the Payment (SRG) system.

Supports allocation rules and exceptions.


d. Accounting Service

Performs accounting entries for each transaction.

Sends data to systems like RDJ, GL (Book Account).


e. Fee Computation Engine (FeesOne)

Central engine for all fee calculations.

Supports both manual and consensus fee scheduling.

Computes payable fees and sends them to invoice generation logic.


f. Referential Service

Stores static data such as client preferences, FX rates, portfolios, and service catalogs.

Provides enriched and validated referential data to other services.


g. Reporting & Restitution Services

Sends structured invoice data to DataHub for reporting and steering.

Supports profitability reporting via Lucid.



---

4. Technical Stack

Language: Java

Architecture: Microservices (Hexagonal Architecture)

DB: PostgreSQL

Tools: Maven, GitHub, AKS (Azure Kubernetes Service)

UI: Angular (Front-end)

Communication: REST APIs, FTR (file transfer), Azure Gateway



---

5. Flow of Execution (End-to-End)

1. Trade Ingestion

Trades are received from SG systems (ELIOT, TPS, etc.) or external providers via APIs.

Registered in Trade Service.



2. Fee Schedule & Computation

Fee rules and schedules are retrieved from Referential/Fee Schedule services.

FeesOne engine calculates the applicable fee per trade.



3. Invoice Generation

Computed fees and trades are passed to Invoice Service.

Invoice Manager groups them based on client/broker structure.



4. Cash Allocation

Once payment is made, it is matched to the invoice in the Cash Allocation Service.



5. Accounting

Each transaction’s accounting is logged and pushed to accounting systems (RDJ, GL).



6. Reporting

Invoice and fee data is sent to DataHub and Lucid for reporting, client profitability analysis, and audits.





---

6. Integration Points

External: DUCO, eRecs (Reconciliation), SRG (Payments), NXG (Break Management)

Internal SG: SCIO, SG-DOCS, SIRROCO (for trade inputs), RDJ/GL (accounting)

On-Prem & Cloud: Services run both on-premises and Azure cloud.



---

7. Hexagonal Architecture Understanding

Focuses on separating core logic from external interactions.

Uses Ports (interfaces) and Adapters (implementations).

For example:

Core logic: Fee Computation

Adapter: API to get fee rules from COOFEE system

Port: FeeRuleService interface




---

8. Key Benefits

Scalability via microservices

Audit-ready with Fee Manager and Accounting Trail

Highly modular and easy to maintain

Hybrid integration (cloud + on-premises)

Support for multi-market trading and fee strategies



---

Let me know if you'd like to extend this with:

Diagrams (architecture, flowcharts)

Individual service responsibilities and sample APIs

Git repo structure breakdown

Jenkins pipeline or deployment flow


I can build that too.

Great! Based on the two architecture diagrams you shared and your notes, here is a complete end-to-end technical explanation of the GTPS CASHFIT Pricing, Invoicing, and Billing system. You can treat this like your project documentation or thesis:


---

GTPS CASHFIT Pricing, Invoicing & Billing System – Technical Overview

Project Overview

The GTPS CASHFIT platform is an internal Société Générale system that handles:

Trade booking and management

Fee computation (including brokerage and payable fees)

Invoice generation

Accounting and reconciliation

Reporting for internal teams and clients


It supports Investment Banking operations, particularly for Cash Management Services, by billing and invoicing various fees associated with trades across global markets (Paris, US, etc.).


---

1. Key Modules/Services

a. Trade Service

Purpose: Receives and manages trades from different internal systems and front-office tools.

Source: DataLake (Eliot, X-One, TPS, etc.)

Used For: Booking trades, capturing trade metadata, initiating fee computation.

Integration: Connects with Invoice Service and Referential for product type info.


b. Invoice Service

Purpose: Manages invoice generation logic.

Triggers: After fees are computed and approved.

Role: Aggregates fees, applies client preferences, and finalizes invoice data.

Output: Final invoice lines to the Datahub and Reporting.


c. Referential Service

Purpose: Central source of truth for products, services, pricing rules, client preferences.

Integration: Used by Fee Engine, Invoice Service, Trade Service.

Type: On-prem and Azure hybrid setup.


d. Fee Computation (FeesOne)

Purpose: Main engine that calculates the amount to be charged.

Modes: Manual and Automatic

Components:

Fee Scheduler

Fee Computation Core

VAT handling


Supports: Different contract types (standard, negotiated), client-specific conditions.


e. Cash Allocation Service

Purpose: Allocates cash/fees to appropriate accounts or trades.

Features:

Works with Accounting

Handles reconciliation input



f. Accounting Service

Purpose: Generates accounting movements for all invoices.

Interacts With:

RDJ (Rules Engine)

GL (Book Acc.)

Reconciliation Modules like DUCO and NXG



g. Reporting & Datahub (Lucid, RESTITUTION)

Purpose: Final consumer of invoice and fee data.

Output: Client invoice reports, internal dashboards, profitability reports.

Tools Used: Extraction Engine, Steering, Profitability Analysis



---

2. Workflow - End-to-End Process

Step 1: Trade Registration

Trades are booked by front-office tools.

Data is pushed to the Trade Management Service (via API/FTR).

Stored with relevant metadata.


Step 2: Fee Trigger

Fee Schedule (manual or consensus) triggers the Fee Engine.

Contract, client, and market info are fetched from Referential.

Fees and VAT are computed.


Step 3: Invoice Creation

Computed fees passed to Invoice Service.

Aggregation per client/account.

Invoice formatted per regulatory requirements.

Ready for DataHub submission.


Step 4: Cash Allocation

Links trade and invoice to actual payments.

Ensures amounts are reconciled and attributed to the correct accounts.


Step 5: Accounting

Generates journal entries based on fee/invoice data.

Applies rules from RDJ (Accounting Rules Engine).

Sent to General Ledger systems.


Step 6: Reporting

Final data is fed to Lucid and DataHub.

RESTITUTION provides dashboards.

Reports include:

Invoice history

Profitability

Reconciliation gaps




---

3. Architecture & Technologies

Backend

Languages: Java

Framework: Spring Boot / Microservices

Build Tools: Maven

Database: PostgreSQL

Deployment: Azure Kubernetes Services (AKS)

CI/CD: GitHub + Azure DevOps


Communication

Internal: REST API, Azure Gateway

External Apps: FTR (File Transfer), COOFEE (Fee Schedule Engine)

Security: On-prem vs. Azure controlled access, not exposed to public



---

4. Observability

Sentinel: Service health, transaction tracking

Monitoring: Logs, latency metrics, failure reports

Audit Trail: All changes are logged for traceability (especially in Fee and Accounting modules)



---

5. Special Business Concepts

Payable Fees: For trades booked in markets where Société Générale lacks a local broker, external brokers (e.g., JPMorgan, Goldman Sachs) are used.

These fees are “payable” and require special computation + reporting.


Client vs Broker Fee Mapping: Some fees are charged to clients, others are absorbed by SG or passed to brokers.

Contract Types: Standard vs Negotiated (Contract Management System maintains rules).



---

6. Hexagonal Architecture

The system loosely follows Hexagonal Architecture principles:

Core Domain Logic: Fee computation, trade, and invoice logic live in independent services.

Ports & Adapters: APIs, file readers (FTR), database connectors.

Decoupling: External systems plug into the core via adapters (Azure Gateway, FTR connectors).



---

If you’d like, I can also:

Generate a component diagram or flowchart

Write individual module explanations (code-level trace)

Help create a template for onboarding developers to your team


Let me know how deep you want to go or if you want this in a Word/PDF format.
GTPS CASHFIT Pricing, Invoicing & Billing System – Technical Overview

Project Overview

The GTPS CASHFIT platform at Société Générale handles:

Trade booking and management

Fee computation (brokerage, payable fees)

Invoice generation

Accounting and reconciliation

Reporting for internal teams and clients


This platform supports investment banking operations for Cash Management Services.


---

1. Key Modules/Services

a. Trade Service

Receives trades from front-office systems

Integrates with DataLake (Eliot, X-One, TPS)

Sends trades to Fee and Invoice services


b. Invoice Service

Aggregates computed fees

Applies client preferences

Generates invoice lines for reporting


c. Referential Service

Central source for pricing rules, products, client info

Used across all services for consistency


d. Fee Computation (FeesOne)

Calculates fees using scheduled/manual triggers

Fetches rules from Referential

Handles VAT and contract-specific logic


e. Cash Allocation Service

Links payments with trades/invoices

Supports reconciliation and accounting


f. Accounting Service

Generates journal entries

Applies RDJ rules

Sends data to General Ledger (GL)


g. Reporting & Datahub (Lucid, RESTITUTION)

Generates dashboards, reports

Provides data for profitability and invoice analysis



---

2. Workflow - End-to-End Process

Step 1: Trade Registration

Trades booked in front-office

Sent to Trade Management Service


Step 2: Fee Trigger

Triggered by schedule or manual request

Rules fetched from Referential

Fees computed, VAT applied


Step 3: Invoice Creation

Fees aggregated

Invoice lines created per client preferences

Data sent to DataHub


Step 4: Cash Allocation

Payments linked to invoices/trades

Ensures correct account attribution


Step 5: Accounting

Journal entries generated

Sent to GL system


Step 6: Reporting

Lucid & RESTITUTION provide insights

Reports: Invoice history, Profitability, Reconciliation



---

3. Architecture & Technologies

Backend: Java, Spring Boot

Database: PostgreSQL

Deployment: Azure Kubernetes Services

CI/CD: GitHub + Azure DevOps

Integration: REST API, Azure Gateway, FTR

Security: On-prem & Azure controlled



---

4. Observability

Sentinel for monitoring

Logs, metrics, and failure reports

Audit trail for all modules



---

5. Business Concepts

Payable Fees: For trades with external brokers

Client vs Broker Fee Mapping

Contract Types: Standard vs Negotiated



---

6. Hexagonal Architecture

Core logic isolated from external integrations

Ports and Adapters for flexibility

Decoupled module design



---

This document serves as a base for tracking the project architecture and understanding the code structure in GTPS CASHFIT.


