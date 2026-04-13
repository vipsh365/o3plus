# O3+ (Visage Beauty & Health Care Pvt Ltd) : ERP System Specification Document

**Version:** 1.0
**Date:** 12 April 2026
**Prepared for:** Vineet Sharma (Promoter & MD), Vidur Sharma (Director)
**Prepared by:** Vipul Udbhav (COO)
**Classification:** Confidential

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [System Architecture](#2-system-architecture)
3. [Module Specifications](#3-module-specifications)
   - 3.1 Finance & Accounting (FIN)
   - 3.2 Inventory & Warehouse Management (INV)
   - 3.3 Procurement & Vendor Management (PRC)
   - 3.4 Process Manufacturing & Production (MFG)
   - 3.5 Quality Control & Compliance (QC)
   - 3.6 Sales & Distribution (SD)
   - 3.7 E-Commerce Integration (ECOM)
   - 3.8 Salon B2B & Channel Management (B2B)
   - 3.9 Human Resources & Performance Management (HR)
   - 3.10 Product Lifecycle Management (PLM)
   - 3.11 Supply Chain Planning & Forecasting (SCM)
   - 3.12 Regulatory & Compliance Management (REG)
   - 3.13 MIS & Business Intelligence (MIS)
   - 3.14 CRM & Customer Management (CRM)
4. [AI/ML Layer Specifications](#4-aiml-layer-specifications)
5. [Integration Specifications](#5-integration-specifications)
6. [Data Migration Plan](#6-data-migration-plan)
7. [Non-Functional Requirements](#7-non-functional-requirements)
8. [Implementation Phasing](#8-implementation-phasing)
9. [Appendix: MIS Report Catalog](#9-appendix-mis-report-catalog)

---

## 1. Executive Summary

### 1.1 Business Context

O3+ (Visage Beauty & Health Care Pvt Ltd) is a 21-year-old Indian professional skincare company with Rs 378 crore revenue in FY25, targeting Rs 490 crore by FY27 and Rs 780 crore by FY29. The company operates 237+ SKUs across 7+ brands (O3+ Professional, DermaCult, Sara Beauty, Agelock, Biozoma, O3+ x Mijoo, Laamis), manufactures from a single GMP-certified plant in Parwanoo (Himachal Pradesh), and sells through five distinct verticals: Salon B2B (50,000+ salons), Modern Trade, E-commerce (D2C via Shopify, marketplaces via Amazon/Nykaa, quick commerce via Blinkit/Zepto), General Trade/Wholesale, and Export (Nepal, Bangladesh, UAE, Mauritius, Canada).

### 1.2 Current State

The company currently runs Microsoft Dynamics NAV (Navision) at low utilization, with heavy reliance on Excel spreadsheets, WhatsApp coordination, and manual processes for critical business operations. This results in limited real-time visibility, fragmented data, delayed decision-making, and an inability to enforce process discipline systematically. There is no integrated view of inventory across locations, no automated MIS, no FIFO enforcement at the system level, and limited batch traceability.

### 1.3 Transformation Objectives

This specification defines the target-state ERP system that will deliver:

1. A single command system across inventory, production, dispatch, and finance with real-time visibility
2. Elimination of manual coordination and Excel-based workarounds
3. Full batch traceability (forward and backward) for regulatory compliance
4. FIFO/FEFO enforcement at every warehouse operation
5. Channel-wise profitability analysis at SKU level
6. Integrated supply chain from forecasting through procurement, manufacturing, warehousing, and dispatch
7. Live MIS dashboards refreshed in real time for leadership decision-making
8. 20-25% efficiency improvement through SKU rationalization, vendor consolidation, and demand-driven operations
9. AI-ready data architecture enabling future automation of MIS, skin prescription/recommendation, and performance management
10. Full GST compliance including e-invoicing, e-way bill generation, TDS, and TCS
11. Multi-channel e-commerce integration with real-time inventory synchronization
12. Regulatory compliance tracking for BIS, CDSCO, FDA/MoCRA, EU 1223/2009, and cruelty-free certifications

### 1.4 Recommended Platform

Microsoft Dynamics 365 Business Central (SaaS) as the core ERP, selected for:

- Natural upgrade path from existing Navision installation (familiar data model, reduced migration complexity)
- Strong process manufacturing extensions available in the Indian partner ecosystem
- Native GST compliance for Indian operations
- Cloud-first architecture with Azure infrastructure
- Extensibility via AL language and Power Platform integration
- Lower total cost of ownership versus SAP S/4HANA or Oracle Cloud for a company at O3+'s scale
- Mature Power BI integration for MIS dashboards

---

## 2. System Architecture

### 2.1 Platform Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Core ERP | Microsoft Dynamics 365 Business Central (SaaS) | Finance, inventory, procurement, manufacturing, sales, distribution |
| Process Manufacturing Extension | Insight Works or ProcessPro (BC-native ISV) | Formulation management, batch processing, potency tracking, yield management |
| WMS Extension | Insight Works Advanced Warehousing or TaskletFactory | Bin management, FIFO/FEFO enforcement, barcode/RF scanning, zone management |
| Quality Management | Clever Dynamics QM or custom AL extension | QC workflows, inspection plans, CoA, OOS investigation, CAPA |
| HRMS | Darwinbox | Organizational structure, JD/KRA/KPI, attendance, payroll, reviews, performance management |
| E-commerce | Shopify Plus (D2C), Amazon SP-API, Nykaa API, Quick Commerce APIs | Multi-channel order management, inventory sync, fulfillment |
| CRM | D365 Sales or HubSpot | Salon relationship management, lead tracking, territory management |
| BI & MIS | Power BI Premium | Real-time dashboards, scheduled reports, embedded analytics |
| AI/ML Platform | Azure AI Services + Azure ML | MIS automation, skin recommendation engine, demand forecasting, performance scanning |
| Integration Platform | Azure Logic Apps + Azure API Management | Middleware for all system integrations |
| Document Management | SharePoint Online | SOPs, CoAs, MSDS, regulatory documents, batch records |

### 2.2 Cloud Strategy

| Aspect | Decision | Rationale |
|--------|----------|-----------|
| Hosting | Microsoft Azure (India South region) | Data residency compliance, lowest latency for Indian operations, native BC integration |
| Deployment | SaaS for BC, PaaS for custom services | Minimal infrastructure management, automatic updates for BC |
| Backup | Azure-managed backup with geo-redundancy | RPO of 5 minutes, RTO of 1 hour |
| Disaster Recovery | Azure Site Recovery to India West region | Business continuity for critical operations |
| Network | Azure ExpressRoute to Parwanoo plant, VPN to Noida HQ | Low-latency connectivity for plant operations |

### 2.3 Integration Architecture

The integration architecture follows a hub-and-spoke model with Azure API Management as the central hub.

```
                                 +-------------------+
                                 |   Power BI        |
                                 |   Dashboards      |
                                 +--------+----------+
                                          |
+-------------+    +----------+    +------+------+    +-----------+
| Shopify     +--->|          |    |             |<---+ Amazon    |
| Plus        |    |  Azure   |    | Dynamics 365|    | SP-API    |
+-------------+    |  API     +--->| Business    |<---+-----------+
| Nykaa       +--->| Manage-  |    | Central     |    | GST Portal|
+-------------+    | ment +   |    |             |<---+-----------+
| Blinkit/    +--->| Logic    |    +------+------+    | Bank APIs |
| Zepto       |    | Apps     |           |           +-----------+
+-------------+    +----+-----+    +------+------+
| Payment     +--->|    |          | Darwinbox   |
| Gateways    |    +----+-------->| HRMS        |
+-------------+         |         +-------------+
                        |         | SharePoint  |
                        +-------->| Online      |
                                  +-------------+
                                  | Azure AI    |
                                  | Services    |
                                  +-------------+
```

All integrations use REST APIs with OAuth 2.0 authentication. Data flows are logged, monitored, and retried on failure. No point-to-point integrations: everything routes through the API management layer.

### 2.4 Environment Strategy

| Environment | Purpose | Refresh Cycle |
|-------------|---------|---------------|
| Production | Live operations | N/A |
| UAT/Staging | User acceptance testing, training | Weekly refresh from Production |
| Development | Customization development and testing | On-demand |
| Sandbox | Partner development, ISV extension testing | On-demand |

---

## 3. Module Specifications

### 3.1 Finance & Accounting (FIN)

**Purpose:** Serve as the single source of financial truth across all entities, channels, and locations. Deliver real-time financial visibility, GST compliance, multi-channel reconciliation, and cost accounting at the granularity required for SKU-level and channel-level profitability analysis.

#### Functional Requirements

**General Ledger & Chart of Accounts**

- FIN-001: Maintain a unified Chart of Accounts structured by: company, department, cost center, brand (O3+ Professional, DermaCult, Sara Beauty, Agelock, Biozoma, O3+ x Mijoo, Laamis), channel (Salon B2B, Modern Trade, E-commerce, General Trade, Export), and location (Parwanoo Plant, Noida HQ, warehouses)
- FIN-002: Support multi-dimensional posting with mandatory dimension tagging on every journal entry: department, brand, channel, location, and project where applicable
- FIN-003: Maintain separate profit centers for each brand and each sales channel combination, enabling P&L generation at brand-channel intersection level
- FIN-004: Support inter-company and inter-location transfer accounting with automatic elimination entries for consolidated reporting
- FIN-005: Maintain fiscal year configuration for Indian financial year (April to March) with 12 monthly periods plus adjustment period
- FIN-006: Support budget allocation at GL account level, department level, brand level, and channel level with multi-year budget tracking (3-year rolling budget aligned with business plan)
- FIN-007: Enforce period-close controls preventing posting to closed periods without authorized override
- FIN-008: Generate trial balance, balance sheet, and P&L statement on demand at any dimensional cut (brand, channel, location, department)

**Accounts Receivable**

- FIN-009: Maintain customer master with complete profile: GSTIN, PAN, billing address, shipping addresses, credit terms, credit limit, payment terms, bank details, sales territory, assigned sales representative, customer category (salon, distributor, modern trade chain, marketplace, export buyer)
- FIN-010: Support tiered credit limit management: customer-level limit, territory-level aggregate limit, channel-level aggregate limit, with configurable approval workflows when limits are approached or breached
- FIN-011: Enforce credit hold on order processing when customer exceeds credit limit, with override requiring approval from finance head or above
- FIN-012: Track receivables aging in standard buckets: current, 1-30 days, 31-60 days, 61-90 days, 91-120 days, 120+ days
- FIN-013: Automate payment reminders via email at configurable intervals (7 days before due, on due date, 7 days overdue, 15 days overdue, 30 days overdue)
- FIN-014: Support payment receipt matching against specific invoices with partial payment allocation
- FIN-015: Handle advance payment receipt, retention, and adjustment against future invoices
- FIN-016: Process credit notes for returns, claims, and trade discounts with linkage to original invoice
- FIN-017: Reconcile marketplace payments (Amazon, Nykaa, Blinkit, Zepto) against orders with commission and fee deduction tracking
- FIN-018: Track TDS receivable (Section 194Q) with certificate reconciliation and annual compliance reporting
- FIN-019: Generate customer account statements on demand and in batch for periodic distribution
- FIN-020: Support bad debt provisioning based on aging analysis with configurable provisioning percentages per aging bucket

**Accounts Payable**

- FIN-021: Maintain vendor master with complete profile: GSTIN, PAN, MSME registration status, bank details, payment terms, approved status, vendor category (raw material supplier, packaging supplier, service provider, logistics, utilities)
- FIN-022: Support three-way matching of purchase order, goods receipt, and vendor invoice before payment authorization
- FIN-023: Enforce invoice approval workflow: department requestor > department head > finance team > finance head, with threshold-based escalation
- FIN-024: Track vendor aging in standard buckets: current, 1-30 days, 31-60 days, 61-90 days, 91-120 days, 120+ days
- FIN-025: Calculate and deduct TDS at applicable rates (Section 194C for contracts, 194J for professional services, 194Q for purchase of goods exceeding Rs 50 lakh) with automatic rate determination based on vendor PAN and threshold tracking
- FIN-026: Generate TDS certificates (Form 16A) and file quarterly TDS returns (26Q) with data export in NSDL format
- FIN-027: Support MSME vendor payment compliance tracking (45-day payment mandate under MSME Act) with alerts and reporting
- FIN-028: Handle advance payment to vendors with adjustment against subsequent invoices
- FIN-029: Support vendor debit notes for returns, quality rejections, and pricing adjustments
- FIN-030: Process vendor payments via NEFT/RTGS/IMPS with bank file generation in standard formats for all major Indian banks
- FIN-031: Manage import vendor payments including foreign currency handling (USD, EUR, AED), bank charges, and forex gain/loss accounting

**GST Compliance**

- FIN-032: Configure GST registration for each state where the company has operations or warehouses, maintaining separate GSTINs per state
- FIN-033: Automatically determine tax type (CGST+SGST for intra-state, IGST for inter-state) based on supplier/customer state and place of supply
- FIN-034: Maintain HSN code master mapped to every item, with correct GST rate assignment (most cosmetics at 18% GST, some at 28%)
- FIN-035: Generate e-invoices via NIC portal API for all B2B invoices exceeding the applicable threshold, capturing IRN (Invoice Reference Number) and QR code
- FIN-036: Generate e-way bills via NIC portal API for all goods movements exceeding Rs 50,000 value, including stock transfers between own locations
- FIN-037: Prepare GSTR-1 (outward supplies) data monthly with invoice-level detail, HSN summary, and document summary
- FIN-038: Prepare GSTR-3B (summary return) data monthly with automatic computation of output tax liability, input tax credit, and net payable
- FIN-039: Perform GSTR-2A/2B reconciliation: match vendor invoices in the system against GSTR-2A/2B data downloaded from GST portal, flag mismatches (missing in 2A, amount mismatch, GSTIN mismatch, duplicate claim)
- FIN-040: Track input tax credit (ITC) by category: eligible ITC, ineligible ITC (blocked credits under Section 17(5)), ITC reversal requirements (Rule 42/43 for common credits)
- FIN-041: Handle reverse charge mechanism (RCM) for specified goods and services (transport by GTA, legal services, import of services) with automatic self-invoice generation
- FIN-042: Support GST on advances received with adjustment on invoice issuance
- FIN-043: Generate Annual Return (GSTR-9) data and reconciliation statement (GSTR-9C) with auditor format export
- FIN-044: Track TCS on e-commerce (Section 52 CGST Act) collected by marketplace operators (Amazon, Nykaa, Flipkart) with reconciliation against Form GSTR-8
- FIN-045: Handle TCS under Section 206C(1H) on sale of goods exceeding Rs 50 lakh to a buyer in a financial year

**Cost Accounting**

- FIN-046: Maintain standard cost for every item (raw material, packaging material, finished good) with annual standard cost revision process
- FIN-047: Calculate formulation cost per kg for each bulk formulation based on BOM ingredients, their standard costs, and yield/loss factors
- FIN-048: Calculate packaging cost per unit for each finished good based on packaging BOM components
- FIN-049: Compute full absorption batch cost including: direct material (formulation + packaging), direct labor (based on batch time and labor rate), manufacturing overhead allocation (based on machine hours or batch hours), quality testing cost allocation
- FIN-050: Compute channel-wise landed cost for each finished good incorporating: manufacturing cost, primary freight, warehousing cost, secondary freight, channel-specific trade margins, marketplace commissions, payment gateway charges, return provision, marketing contribution
- FIN-051: Track cost variances: purchase price variance, material usage variance, labor efficiency variance, overhead spending variance, overhead volume variance
- FIN-052: Support actual costing at batch level alongside standard costing for variance analysis
- FIN-053: Compute import landed cost including: CIF value, basic customs duty, social welfare surcharge, IGST on imports, clearing agent charges, port handling, inland freight, insurance

**Bank & Cash Management**

- FIN-054: Maintain multiple bank accounts with automatic bank statement import (MT940/CSV formats) and bank reconciliation
- FIN-055: Support multi-bank payment processing with payment batch creation and bank file generation
- FIN-056: Track post-dated cheques received and issued with maturity-based accounting
- FIN-057: Manage petty cash with imprest system and expense category tracking
- FIN-058: Generate daily cash position report across all bank accounts and petty cash
- FIN-059: Support LC (Letter of Credit) tracking for import purchases with document submission and maturity tracking

**Fixed Assets**

- FIN-060: Maintain fixed asset register with asset category, location, department, acquisition date, original cost, depreciation method, useful life, and WDV/SLM computation
- FIN-061: Compute depreciation per Companies Act 2013 (Schedule II) and Income Tax Act 1961 (Section 32) in parallel
- FIN-062: Track asset transfers between locations/departments with automatic accounting
- FIN-063: Handle asset disposal with profit/loss on sale computation and GST implications
- FIN-064: Support capital work-in-progress (CWIP) tracking with capitalization workflow

#### Data Entities

- Chart of Accounts, GL Entries, Dimensions, Dimension Values
- Customer Master, Customer Ledger Entries, Customer Aging
- Vendor Master, Vendor Ledger Entries, Vendor Aging
- Bank Account Master, Bank Ledger Entries, Bank Reconciliation
- GST Registration, GST Entries, GST Returns Data, E-Invoice Log, E-Way Bill Log
- TDS Master, TDS Entries, TDS Certificates
- Fixed Asset Master, FA Depreciation Book, FA Ledger Entries
- Budget Master, Budget Entries
- Standard Cost Master, Batch Cost Records, Variance Entries

#### Integration Points

- Bank APIs (HDFC, ICICI, SBI, Kotak) for statement import and payment file export
- GST Portal (NIC) for e-invoicing and e-way bill
- NSDL for TDS return filing
- Power BI for financial dashboards
- All other ERP modules for transaction posting

#### User Roles

| Role | Access |
|------|--------|
| Finance Head | Full access to all finance functions, approval authority for payments and credit limits |
| Accounts Manager | GL posting, journal entries, period close, bank reconciliation |
| AR Executive | Customer management, payment receipt, credit note processing, AR aging |
| AP Executive | Vendor invoice processing, payment preparation, TDS computation |
| GST Compliance Officer | GST return preparation, reconciliation, e-invoicing, e-way bill management |
| Cost Accountant | Standard cost maintenance, batch costing, variance analysis, landed cost computation |
| Finance Analyst | Read-only access for reporting and analysis |

#### Dashboards and Reports

- Real-time cash position across all bank accounts
- AR aging dashboard by customer, territory, channel, and salesperson
- AP aging dashboard by vendor category, payment due date, MSME compliance status
- GST compliance dashboard: return filing status, ITC utilization, reconciliation status
- Monthly P&L by brand, by channel, by brand-channel combination
- SKU-level profitability report with channel-wise landed cost breakdown
- Budget vs actual variance report by department, brand, and channel
- TDS compliance tracker: deduction, deposit, return filing, certificate issuance
- Daily sales and collection summary
- Cost variance analysis by batch, by product, by period

---

### 3.2 Inventory & Warehouse Management (INV)

**Purpose:** Provide real-time visibility into all inventory across all locations, enforce FIFO/FEFO at every movement, manage quarantine and quality zones, track batch aging systematically, and enable channel-wise inventory allocation. This module is the operational backbone of the entire system.

#### Functional Requirements

**Item Master & Classification**

- INV-001: Maintain a unified item master for all inventory types: raw materials (active ingredients, base chemicals, preservatives, fragrances, colorants), packaging materials (primary: bottles, tubes, jars, sachets; secondary: cartons, labels, shrink wrap; tertiary: shippers, pallets), semi-finished goods (bulk formulations), and finished goods (retail-ready SKUs)
- INV-002: Classify items by: item type (raw material, packaging, semi-finished, finished), brand (O3+ Professional, DermaCult, Sara Beauty, Agelock, Biozoma, O3+ x Mijoo, Laamis), product category (facial kit, serum, sunscreen, cleanser, cream, mask, toner, body care, hair care), product sub-category, HSN code, GST rate, unit of measure (kg, litre, piece, set, box)
- INV-003: Maintain multiple units of measure per item with conversion factors: base UoM for stocking, purchase UoM, sales UoM, production UoM (e.g., an ingredient purchased in drums of 200 kg, stocked in kg, consumed in formulation in grams)
- INV-004: Track item attributes relevant to cosmetics: shelf life in months, storage temperature requirement (ambient, cool, cold chain), sensitivity to light/moisture, hazard classification if applicable, allergen flags
- INV-005: Maintain item-level reorder point, reorder quantity, safety stock level, maximum stock level, and lead time (procurement lead time for purchased items, production lead time for manufactured items)
- INV-006: Support item status lifecycle: active, on hold (quality issue), discontinued (no new procurement/production but can still sell existing stock), obsolete (must be cleared)
- INV-007: Maintain item images, technical specifications, regulatory data sheets, and supplier CoA linkage within the item master
- INV-008: Assign default bin locations per item per warehouse location

**Batch Management**

- INV-009: Enforce mandatory batch tracking for all raw materials, packaging materials, semi-finished goods, and finished goods without exception
- INV-010: Implement batch numbering convention: [Plant Code]-[Year (2-digit)]-[Month (2-digit)]-[Sequential Number (4-digit)], e.g., PRW-26-04-0001 for the first batch produced at Parwanoo in April 2026
- INV-011: Record against each batch: manufacturing date, expiry date (calculated from shelf life), batch size (quantity), batch status (quarantine, approved, rejected, recalled), CoA reference, production order reference, quality inspection reference
- INV-012: Enable full forward traceability: from any raw material batch, trace to all production batches that consumed it, and from those production batches to all finished good batches, and from those to all sales orders/invoices/customers that received them
- INV-013: Enable full backward traceability: from any customer complaint or finished good batch, trace back through the production batch to every raw material and packaging material batch used
- INV-014: Support batch splitting: when a batch is partially consumed, partially shipped, or partially rejected, the system must track the split quantities with full traceability maintained
- INV-015: Support batch recall workflow: initiate recall by batch number, automatically identify all downstream batches and customer shipments, generate recall notification list with customer contact details and quantities, track recall response and return quantities
- INV-016: Record batch-level cost: actual material cost, actual labor cost, actual overhead, total batch cost, and per-unit cost

**Warehouse Zones & Bin Management**

- INV-017: Define warehouse zones for the Parwanoo facility: receiving/dock area, raw material quarantine, raw material approved storage, packaging material quarantine, packaging material approved storage, in-process/WIP area, finished goods quarantine, finished goods approved storage, finished goods dispatch staging, rejected material zone, recalled material zone, customer return zone, sample retention zone
- INV-018: Within each zone, define bins (rack-shelf-position) with attributes: bin code, zone assignment, bin capacity (weight and volume), item type restriction (if any), temperature zone (ambient, controlled), bin ranking for pick sequence
- INV-019: Enforce that material cannot move between zones without a system transaction: goods receipt, quality release, production issue, production receipt, pick, ship, transfer, adjustment
- INV-020: Track bin occupancy in real time with visual warehouse map showing utilization percentage by zone
- INV-021: Support temperature zone management: define temperature ranges per zone, track temperature monitoring device readings (manual entry or IoT integration-ready), flag excursions
- INV-022: Manage the sample retention zone: for each production batch, calculate required retain sample quantity (per regulatory requirement), block that quantity from available inventory, track retain sample storage location and destruction date

**FIFO/FEFO Enforcement**

- INV-023: Enforce FEFO (First Expiry, First Out) as the default picking strategy for all finished goods and raw materials with shelf life
- INV-024: Enforce FIFO (First In, First Out) as the picking strategy for packaging materials and items without explicit expiry dates
- INV-025: System must auto-suggest pick lots based on FEFO/FIFO sequence during sales order picking, production order material issue, and stock transfer picking
- INV-026: Require supervisor override with documented reason to pick out of FEFO/FIFO sequence
- INV-027: Block picking of batches that have expired or are within the minimum remaining shelf life threshold configured per customer or channel (e.g., modern trade may require minimum 70% shelf life remaining, salons may accept 50%)
- INV-028: Generate daily FEFO deviation report showing any picks that were executed out of sequence with reason codes

**Inventory Aging Management**

- INV-029: Categorize all inventory into aging buckets based on manufacturing/receipt date: fresh (0-6 months from manufacture), aging (6-12 months), at-risk (12-24 months), dead (beyond 24 months or within 3 months of expiry)
- INV-030: Apply aging categorization separately for: raw material ingredients, packaging materials, and finished goods
- INV-031: Generate automated alerts when any item crosses an aging threshold: notify procurement and planning when raw materials enter "aging" category, notify sales and marketing when finished goods enter "aging" category
- INV-032: Trigger action protocols per aging band:
  - Fresh (0-6 months): normal sales/production flow, no intervention needed
  - Aging (6-12 months): flag for priority allocation to faster-moving channels, eligible for scheme/combo bundling
  - At-risk (12-24 months): mandatory action required within 30 days: cross-selling into adjacent channels, barter arrangements, discounted clearance through designated outlets (not primary channels to protect brand), reprocessing evaluation where ingredient stability permits
  - Dead (beyond 24 months): evaluate salvage value, write-off with finance head approval, mandatory root-cause analysis documented in the system linking back to procurement or forecast error
- INV-033: Track aging-related actions: what action was taken, by whom, what quantity was cleared, what was the realized value versus book value, what was the root cause finding
- INV-034: Report monthly inventory aging profile with trend analysis (is the aging stock increasing or decreasing over time)

**Inventory Transactions**

- INV-035: Support all standard inventory transactions with mandatory batch and bin assignment: purchase receipt, purchase return, production consumption (issue), production output (receipt), sales shipment, sales return receipt, transfer between locations, transfer between bins within location, positive adjustment, negative adjustment, physical count adjustment, quality status change (quarantine to approved, quarantine to rejected), scrap/waste recording
- INV-036: Require reason codes for all inventory adjustments (positive and negative) with configurable reason code master (count variance, quality rejection, expiry write-off, damage, sample issue, testing consumption, marketing sample)
- INV-037: Log every inventory transaction with timestamp, user, transaction type, item, batch, from-bin, to-bin, quantity, reason code, and reference document number
- INV-038: Support serialized tracking for high-value equipment items (salon equipment) in addition to batch tracking for consumables

**Channel-Wise Inventory Allocation**

- INV-039: Maintain channel-wise inventory pools: Salon B2B allocation, Modern Trade allocation, E-commerce allocation (further split by Shopify D2C, Amazon FBA, Amazon FBM, Nykaa, Blinkit, Zepto), General Trade allocation, Export allocation, Unallocated/common pool
- INV-040: Allow inventory reservation against specific channels based on demand forecast and sales orders
- INV-041: Enforce allocation limits per channel: no channel can draw more than its allocated quantity without planning manager approval
- INV-042: Support dynamic reallocation: when one channel is under-consuming its allocation and another is over-demanding, planning manager can transfer allocation between channels with documented approval
- INV-043: Track allocation utilization by channel daily and report over/under-allocation trends weekly

**Cycle Counting & Physical Inventory**

- INV-044: Support perpetual cycle counting: system generates daily count lists based on ABC classification (A items counted monthly, B items quarterly, C items semi-annually)
- INV-045: Support full physical inventory count with system-guided count sheets by zone and bin
- INV-046: Track count variance by item, batch, and bin with automatic variance posting after approval
- INV-047: Require investigation and documented explanation for variances exceeding 2% of item value before write-off approval
- INV-048: Support blind counting (counter does not see system quantity) to prevent confirmation bias
- INV-049: Generate cycle count accuracy KPI by zone, item category, and time period

**Barcode & Scanning**

- INV-050: Support barcode/QR code scanning for all warehouse transactions: receiving, putaway, picking, packing, shipping, transfer, counting
- INV-051: Generate barcode labels for bins, pallets, cases, and individual items
- INV-052: Support handheld RF scanner devices (Zebra or Honeywell) with offline capability for intermittent connectivity areas in the plant
- INV-053: Validate scanned items against expected items in real time (e.g., during picking, confirm scanned batch matches FEFO-suggested batch)

#### Data Entities

- Item Master, Item Category, Item Attributes, Item Unit of Measure Conversion
- Batch Master, Batch Traceability Links, Batch Recall Records
- Warehouse Location Master, Zone Master, Bin Master, Bin Content
- Inventory Ledger Entries, Item Tracking Entries
- Channel Allocation Master, Channel Allocation Entries
- Inventory Aging Snapshots, Aging Action Records
- Cycle Count Schedule, Cycle Count Entries, Count Variance Records
- Barcode Label Templates, Scan Transaction Log

#### Integration Points

- Manufacturing module: production consumption and output
- Procurement module: purchase receipts
- Sales module: sales shipments and returns
- Quality module: quality status changes
- Finance module: inventory valuation and cost posting
- E-commerce module: real-time available-to-promise (ATP) quantities
- Power BI: inventory dashboards

#### User Roles

| Role | Access |
|------|--------|
| Warehouse Manager | Full access to all warehouse operations, zone/bin setup, allocation management |
| Warehouse Supervisor | Transaction processing, picking, packing, shipping, cycle counting |
| Warehouse Operator | Scan-based transactions: receiving, putaway, picking, packing |
| Inventory Planner | Channel allocation, reorder point management, aging analysis, ATP management |
| Inventory Analyst | Read-only access for reporting, aging review, variance investigation |

#### Dashboards and Reports

- Real-time inventory position by item, batch, location, zone, bin
- Inventory aging dashboard with aging band distribution by item category (RM, PM, FG)
- FIFO/FEFO compliance dashboard: deviations, overrides, reason analysis
- Channel allocation utilization dashboard
- Warehouse zone utilization heat map
- Cycle count accuracy trend by zone and item category
- Inventory turnover ratio by item category, brand, and channel
- Dead stock report with root cause analysis linkage
- Daily goods receipt summary
- Daily dispatch summary with order fulfillment rate
- Near-expiry alert report (items expiring within 90, 60, 30, 15 days)

---

### 3.3 Procurement & Vendor Management (PRC)

**Purpose:** Manage the entire procure-to-pay cycle from vendor qualification through purchase planning, ordering, receiving, inspection, and payment. Enforce vendor quality standards, manage approved vendor lists, optimize procurement costs, and ensure demand-linked purchasing that eliminates speculative or ad-hoc buying.

#### Functional Requirements

**Vendor Qualification & Approved Vendor List**

- PRC-001: Maintain an Approved Vendor List (AVL) for each item category, with vendors categorized by approval status: approved, conditionally approved (with conditions documented), under evaluation, suspended, blacklisted
- PRC-002: Implement vendor qualification workflow: vendor registration (self-service portal or manual entry) > document collection (GSTIN, PAN, MSME certificate, manufacturing license, ISO certificates, GMP certificate, FDA registration where applicable) > quality team evaluation (facility audit checklist, sample testing) > commercial evaluation (pricing, payment terms, lead time, MOQ) > approval committee decision (quality head + procurement head + finance head)
- PRC-003: Track vendor qualification documents with expiry dates: GMP certificate validity, ISO certification validity, manufacturing license validity, insurance policy validity. Generate alerts 60 days before expiry for renewal follow-up
- PRC-004: Maintain vendor performance scorecard updated quarterly: quality score (rejection rate, CoA compliance, OOS incidents), delivery score (on-time delivery rate, quantity accuracy), commercial score (price competitiveness, payment term compliance), responsiveness score (query response time, complaint resolution time)
- PRC-005: Automatically flag vendors whose performance score drops below threshold for review, and suspend vendors who fail two consecutive reviews
- PRC-006: Require minimum two approved vendors per critical raw material to prevent single-source dependency, with system alerting when an item has only one approved vendor
- PRC-007: Support vendor categorization: strategic (high value, critical supply), preferred (primary sourcing), alternate (backup), spot (one-time or emergency)

**Procurement Planning**

- PRC-008: Generate material requirement plans (MRP) from production forecasts: calculate gross requirements from planned production orders, net off against current inventory and pending purchase orders, generate planned purchase requisitions for shortfall quantities
- PRC-009: Support procurement lead time tracking per vendor per item, with system learning actual lead times from historical receipts
- PRC-010: Calculate economic order quantities considering: vendor MOQ (minimum order quantity), price break quantities, storage capacity constraints, shelf life constraints (do not order more than can be consumed within 80% of shelf life), cash flow considerations
- PRC-011: Generate consolidated procurement plan showing: what to order, from which vendor, in what quantity, by when, at what estimated cost, with total cash outflow projection by week and month
- PRC-012: Flag procurement requisitions that are not linked to a demand signal (production order, sales forecast, or replenishment trigger) for review and approval before conversion to purchase order

**Purchase Order Management**

- PRC-013: Create purchase orders from approved requisitions with configurable approval workflow based on PO value: up to Rs 50,000 (procurement officer), Rs 50,001 to Rs 5,00,000 (procurement manager), Rs 5,00,001 to Rs 25,00,000 (procurement head), above Rs 25,00,000 (COO/MD)
- PRC-014: Support purchase order types: standard PO, blanket PO (annual rate contract with scheduled releases), import PO (with LC/advance payment terms and incoterms), subcontracting PO (for outsourced processing), consignment PO
- PRC-015: Include on every PO: agreed specifications, CoA requirement (mandatory/not required), delivery schedule, penalty clauses for late delivery, quality rejection terms, payment terms, applicable taxes (GST with HSN)
- PRC-016: Support PO amendment workflow: amendment request > procurement manager approval > vendor acknowledgement tracking
- PRC-017: Track PO status lifecycle: draft > approved > sent to vendor > acknowledged by vendor > partially received > fully received > closed
- PRC-018: Generate PO in PDF format with company letterhead, terms & conditions, for email dispatch and vendor portal access

**Certificate of Analysis (CoA) Management**

- PRC-019: Define CoA parameter requirements per item: which parameters must be reported (e.g., pH, viscosity, microbial limits, heavy metals, assay/potency, appearance, odor, specific gravity), acceptable ranges for each parameter
- PRC-020: Require vendor CoA upload against each delivery before goods can be released from receiving quarantine
- PRC-021: Compare vendor CoA values against defined specifications automatically, flagging any out-of-spec parameters
- PRC-022: Store CoA documents linked to vendor, item, batch, and purchase receipt for audit traceability
- PRC-023: Track CoA compliance rate per vendor as an input to vendor performance scorecard

**Material Safety Data Sheet (MSDS/SDS) Management**

- PRC-024: Maintain current MSDS/SDS for every raw material and chemical ingredient in the system, linked to the item master
- PRC-025: Track MSDS revision dates and alert when an MSDS is older than 3 years for update request to vendor
- PRC-026: Make MSDS accessible to warehouse and production personnel via the system for any material they handle

**Import Management**

- PRC-027: Support import purchase workflow: import PO creation with incoterm (FOB, CIF, CFR, etc.) > LC opening request to finance > shipment tracking > customs clearance documentation (bill of entry, commercial invoice, packing list, CoO) > port receipt > inland transit > factory receipt
- PRC-028: Calculate import landed cost: CIF value + basic customs duty (rate per HS tariff code) + social welfare surcharge (10% on BCD) + IGST on imports + anti-dumping duty where applicable + clearing agent fee + port handling charges + container freight station charges + inland freight + insurance
- PRC-029: Track import shipment milestones: vessel departure, estimated arrival, port arrival, customs examination, out of charge, dispatch to factory, factory receipt
- PRC-030: Manage import documentation repository per shipment: commercial invoice, packing list, bill of lading, certificate of origin, insurance certificate, bill of entry, CoA, MSDS
- PRC-031: Handle foreign currency PO with automatic forex rate application at booking, receipt, and payment, with gain/loss computation

**MOQ & Price Management**

- PRC-032: Maintain vendor-item price lists with: base price, price break slabs (quantity-based discounts), validity period, currency, incoterm, payment term
- PRC-033: Support price comparison across approved vendors for the same item, with automatic vendor recommendation based on total landed cost
- PRC-034: Track MOQ per vendor per item and alert when requisition quantity is below MOQ, suggesting consolidation with other requirements
- PRC-035: Maintain price history per vendor per item for trend analysis and negotiation support
- PRC-036: Support annual rate contracts with locked pricing, volume commitments, and compliance tracking

#### Data Entities

- Vendor Master, Vendor Qualification Records, Vendor Performance Scores
- Item-Vendor Relationship (AVL), Vendor Price List, Vendor MOQ
- Purchase Requisition, Purchase Order, PO Line Items, PO Amendments
- CoA Master (specifications per item), CoA Document Repository, CoA Verification Log
- MSDS Repository
- Import Shipment Master, Import Cost Computation Worksheet, Import Document Repository
- Vendor Scorecard Parameters, Vendor Evaluation Records

#### Integration Points

- Inventory module: goods receipt posting, stock availability check
- Quality module: incoming inspection trigger, CoA verification
- Finance module: purchase invoice processing, vendor payment, TDS, GST
- Manufacturing module: production order-driven material requirements
- SCM module: demand forecasts driving procurement plan
- SharePoint: document storage for CoAs, MSDS, contracts, import documents

#### User Roles

| Role | Access |
|------|--------|
| Procurement Head | Full access, PO approval for high-value orders, vendor approval authority |
| Procurement Manager | PO creation and mid-value approval, vendor management, rate contract negotiation |
| Procurement Officer | Requisition processing, PO creation for low-value orders, order tracking |
| Import Specialist | Import PO management, shipment tracking, landed cost computation, customs documentation |
| Vendor Quality Coordinator | Vendor qualification, AVL management, CoA verification, vendor audit scheduling |

#### Dashboards and Reports

- Procurement pipeline dashboard: requisitions pending, POs in progress, deliveries expected this week/month
- Vendor performance scorecard dashboard by vendor and item category
- Purchase price variance report: standard cost vs actual purchase price by item and vendor
- Import shipment tracker: all active imports with milestone status
- MOQ compliance report: instances of sub-MOQ ordering and cost impact
- Vendor concentration risk report: spend distribution across vendors per item category
- Procurement savings report: negotiated savings vs prior period or standard cost
- Pending delivery report: overdue POs by vendor and item
- AVL coverage report: items with single-source vendors flagged
- CoA compliance rate by vendor

---

### 3.4 Process Manufacturing & Production (MFG)

**Purpose:** Manage process manufacturing (not discrete manufacturing) for skincare/cosmetic products, including formulation-based BOMs, batch processing with potency and concentration management, production scheduling, yield tracking, and batch costing. This module must handle the unique characteristics of cosmetic manufacturing: percentage-based formulations that scale, multi-stage processing (bulk preparation, filling, packaging), and ingredient potency variability.

#### Functional Requirements

**Formulation & Bill of Materials Management**

- MFG-001: Support process manufacturing BOMs (formulation-based), not discrete BOMs: formulations are defined as percentage-based recipes where each ingredient contributes a percentage of the total batch weight, and the recipe scales proportionally to any batch size
- MFG-002: Maintain multi-level BOMs with at least three levels: Level 1: finished good (packaged retail unit), Level 2: semi-finished good (bulk formulation, e.g., "DermaCult Vitamin C Serum Bulk"), Level 3: raw materials and packaging materials
- MFG-003: For bulk formulation BOMs (Level 2), define ingredients by: percentage composition (e.g., active ingredient at 2%, emulsifier at 5%, purified water QS to 100%), function/role in formula (active, emulsifier, preservative, fragrance, colorant, base), phase assignment for manufacturing sequence (oil phase, water phase, cool-down phase, final addition)
- MFG-004: For packaging BOMs (Level 1), define components by: quantity per finished unit (e.g., 1 bottle, 1 pump cap, 1 label, 1 carton, 1 shrink sleeve), scrap/waste percentage per component
- MFG-005: Support BOM version control: each BOM has a version number, effective date, and expiry date. When R&D modifies a formulation, a new version is created while the previous version remains accessible for historical production reference. Only one version can be active at any time per item
- MFG-006: Support yield and loss factors at BOM level: theoretical yield (batch size minus expected processing loss), actual yield recorded per batch for variance analysis
- MFG-007: Support alternate BOMs per item: when primary ingredients are unavailable, production can switch to a pre-approved alternate formulation with documented regulatory approval
- MFG-008: Support phantom BOMs (sub-assemblies that are not stocked independently): e.g., a pre-mix of actives that is prepared and immediately consumed in the main formulation without being independently inventoried
- MFG-009: Compute theoretical cost per kg of bulk formulation and per unit of finished good from the BOM, using current standard costs of components
- MFG-010: Support co-products and by-products from process manufacturing where applicable (e.g., if a process generates a usable secondary output)

**Production Planning & Scheduling**

- MFG-011: Generate production orders from: sales forecasts, confirmed sales orders, inventory replenishment triggers (stock below reorder point), or manual planning entry
- MFG-012: Support production order types: planned (tentative, not yet committed), firm planned (committed, materials being arranged), released (ready for production floor), started (in progress), finished (production complete, pending QC), closed (QC passed, costed, posted)
- MFG-013: Calculate material requirements per production order: from the BOM and batch size, compute required quantities of each raw material and packaging material, check availability against current inventory and pending purchase receipts, flag shortages
- MFG-014: Schedule production orders on manufacturing resources (mixing vessels, filling lines, packaging lines) considering: resource capacity (hours per shift per day), changeover time between products (especially important for fragrance and color changeovers), batch processing time (mixing time, holding time, filling time, packaging time), cleaning/sanitation requirements between batches, maintenance windows
- MFG-015: Support finite capacity scheduling: do not over-schedule a resource beyond its available capacity, highlight bottlenecks
- MFG-016: Generate production schedule Gantt chart showing: all planned and released production orders on a timeline, resource utilization, material availability alignment
- MFG-017: Support campaign production: schedule consecutive batches of the same product to minimize changeover time and cleaning between batches

**Batch Processing & Execution**

- MFG-018: Create batch production records (BPR) for each production order with: formulation reference, batch size, raw material allocation by batch (linking consumed RM batches to the production batch), processing instructions (temperature, time, speed per phase), in-process checkpoints, equipment assignment
- MFG-019: Record actual material consumption per batch against theoretical BOM quantities, computing usage variance (over-consumption or under-consumption of each ingredient)
- MFG-020: Support potency/concentration management for active ingredients: when an ingredient arrives at a potency different from the standard (e.g., Vitamin C at 98% instead of standard 100%), automatically adjust the batch formula to compensate and achieve the target concentration in the finished product
- MFG-021: Record batch processing parameters at each stage: mixing temperature, mixing time, mixing speed, holding time, filling temperature, room temperature and humidity during filling, and operator observations
- MFG-022: Record in-process quality checks at defined checkpoints: pH check after mixing, viscosity check, appearance check, weight check during filling, torque check during capping, label verification, batch coding verification
- MFG-023: Compute actual batch yield: actual finished quantity versus theoretical quantity, with yield percentage and loss analysis (processing loss, filling loss, spillage, sampling loss, testing loss)
- MFG-024: Support batch splitting post-production: if QC partially approves a batch (e.g., some units fail weight check), the batch can be split into approved and rejected sub-batches
- MFG-025: Record all equipment used per batch (reactor number, filling line number, packaging line number) for traceability
- MFG-026: Record all personnel involved in the batch (operators, supervisors, QC samplers) for GMP compliance

**Production Costing**

- MFG-027: Compute batch cost with full absorption: direct material (actual consumed quantities at actual batch-specific or standard cost), direct labor (actual hours at labor rate), variable manufacturing overhead (utility cost, consumables), fixed manufacturing overhead (depreciation, rent, insurance allocated per batch hour or machine hour)
- MFG-028: Compute cost per unit (per finished good unit) from batch total cost and actual yield quantity
- MFG-029: Track cost variance at batch level: material cost variance, labor cost variance, overhead variance, yield variance
- MFG-030: Roll up manufacturing cost into finished good standard cost for margin analysis

**Production Reporting**

- MFG-031: Generate production order completion report per batch: planned vs actual quantities, planned vs actual material consumption, planned vs actual time, yield percentage, variance summary
- MFG-032: Track OEE (Overall Equipment Effectiveness) per production line: availability (uptime vs scheduled time), performance (actual throughput vs rated capacity), quality (good units vs total units produced)
- MFG-033: Log production downtime per resource with reason codes: mechanical breakdown, electrical fault, changeover, cleaning, waiting for material, waiting for QC clearance, planned maintenance, no production order
- MFG-034: Track changeover time between products per production line to identify optimization opportunities
- MFG-035: Generate shift-wise production summary: products produced, quantities, batch numbers, downtime events, in-process rejections

**Subcontracting**

- MFG-036: Support subcontracting/toll manufacturing workflow: issue raw materials to subcontractor (tracked as inventory in transit to subcontractor location), track subcontractor processing, receive finished/semi-finished goods from subcontractor, reconcile issued material vs received goods with yield and loss
- MFG-037: Maintain subcontractor master with: facility details, GMP certification, approved product list, rate per unit/kg, quality requirements, audit schedule

#### Data Entities

- Formulation/BOM Master, BOM Versions, BOM Lines (ingredients/components), BOM Alternate, BOM Phantom
- Production Order Header, Production Order Lines (material, resource, capacity)
- Batch Production Record, Batch Material Consumption, Batch Processing Parameters
- Resource Master (machines, lines, vessels), Resource Calendar, Resource Capacity
- Production Order Routing (processing steps and times)
- Batch Cost Records, Cost Variance Entries
- Downtime Log, OEE Records
- Subcontractor Master, Subcontract Orders, Subcontract Material Issue/Receipt

#### Integration Points

- Inventory module: material consumption posting, production output posting, batch creation
- Procurement module: material shortage triggering purchase requisitions
- Quality module: in-process and finished goods inspection triggers
- SCM module: production plans driven by demand forecasts
- Finance module: production cost posting, WIP accounting
- MIS module: production dashboards and KPI feeds

#### User Roles

| Role | Access |
|------|--------|
| Production Head | Full access, production planning, capacity management, schedule approval |
| Production Planner | Production order creation, scheduling, MRP, campaign planning |
| Production Supervisor | Batch execution, material issue, processing parameter recording, output recording |
| Production Operator | Scan-based material issue and output recording, in-process checkpoint entry |
| Process Engineer | BOM/formulation creation and version management, yield analysis, process optimization |
| Production Cost Analyst | Batch costing, variance analysis, OEE analysis |

#### Dashboards and Reports

- Production schedule dashboard: today's production plan, in-progress batches, completed batches, upcoming schedule
- Batch status tracker: all active production orders with status, material availability, scheduled time
- Production vs plan dashboard: planned quantities vs actual production by day, week, month
- OEE dashboard by production line with availability, performance, quality components
- Yield analysis report: batch-wise yield percentage with trend over time by product
- Material consumption variance report: actual vs BOM standard by item and batch
- Batch cost report: cost breakdown per batch and per unit
- Downtime analysis: Pareto chart of downtime reasons by resource
- Resource utilization report: hours utilized vs available by resource
- Changeover time analysis: average changeover by product transition

---

### 3.5 Quality Control & Compliance (QC)

**Purpose:** Manage the entire quality lifecycle for a cosmetic manufacturing operation, from incoming raw material inspection through in-process controls to finished goods release. Enforce GMP compliance (IS 22716/ISO 22716, Schedule M-II), manage deviations and CAPA, track stability testing, and maintain the documentation required for regulatory audits.

#### Functional Requirements

**Inspection Plans**

- QC-001: Define inspection plans per item per inspection type (incoming, in-process, finished goods) specifying: parameters to test, test method reference, specification limits (min, max, target), sampling plan (sample size, acceptance criteria per IS 2500/ISO 2859), frequency, critical/major/minor classification of each parameter
- QC-002: Maintain inspection plan versions with effective dates; new versions require quality head approval before activation
- QC-003: Support skip-lot inspection: after a configurable number of consecutive passed lots from a vendor, reduce inspection frequency (e.g., from every lot to every third lot), with automatic reversion to full inspection on any failure
- QC-004: Define separate inspection plans for different product categories: facial treatments, serums, sunscreens, cleansers, creams, masks, hair care, body care

**Incoming Material Inspection**

- QC-005: Automatically trigger inspection order when goods are received against a purchase order, placing the received material in quarantine zone
- QC-006: Record inspection results per parameter: actual value, pass/fail against specification, instrument used, analyst name, date
- QC-007: Compare vendor CoA against in-house test results with configurable tolerance for acceptable difference
- QC-008: Support pass, conditional pass (with documented conditions and approvals), and fail outcomes per inspection lot
- QC-009: On pass: automatically release material from quarantine to approved zone, update batch status
- QC-010: On fail: generate rejection note, move to rejected zone, trigger vendor complaint and return process, update vendor quality score
- QC-011: On conditional pass: document the conditions under which material is accepted, track that conditions are fulfilled before unrestricted use

**In-Process Quality Control**

- QC-012: Define in-process control points per production step: after bulk mixing (pH, viscosity, appearance, homogeneity), during filling (fill weight/volume at defined intervals), during capping (torque test), during labeling (label correctness, batch coding, expiry date print verification), during secondary packaging (count accuracy, carton weight)
- QC-013: Record in-process test results linked to the production batch, step, and timestamp
- QC-014: Define control limits (specification limits plus tighter alert limits) and trigger real-time alerts when results approach alert limits before breaching specification
- QC-015: Support SPC (Statistical Process Control) charts for critical parameters (fill weight, pH) with automatic Xbar-R chart generation

**Finished Goods Inspection**

- QC-016: Trigger finished goods inspection upon production order completion, maintaining product in quarantine until QC release
- QC-017: Define finished goods test protocols per product: visual inspection, weight/volume verification, packaging integrity (leak test, drop test for selected samples), label and batch code verification, analytical testing (pH, viscosity, microbial limits, preservative efficacy where scheduled), sensory evaluation (appearance, odor, texture)
- QC-018: Support certificate of analysis (CoA) generation for each released batch: product name, batch number, manufacturing date, expiry date, all tested parameters with specifications and actual results, released by (quality head or designee), release date
- QC-019: Block shipment/dispatch of any finished goods batch that does not have QC release status in the system

**Out-of-Specification (OOS) Investigation**

- QC-020: When any test result falls outside specification, automatically initiate OOS investigation workflow
- QC-021: OOS investigation Phase 1 (laboratory investigation): verify sample integrity, check instrument calibration, review analyst technique, retest by same analyst, retest by different analyst. Document findings
- QC-022: OOS investigation Phase 2 (production investigation, if Phase 1 does not explain the OOS): review batch production record, check material inputs, review processing parameters, evaluate potential root causes in production
- QC-023: Require documented conclusion and corrective action for every OOS investigation before batch disposition decision
- QC-024: Track OOS investigation timeline with escalation if investigation exceeds 30 days

**Retain/Reference Samples**

- QC-025: For each production batch, retain reference samples per regulatory requirements: sufficient quantity for two complete re-tests, stored in conditions mimicking product shelf life
- QC-026: Track retain sample storage location, quantity, and destruction date (1 year beyond product expiry)
- QC-027: Log any access to retain samples (reason, quantity withdrawn, authorizing person)

**Stability Testing**

- QC-028: Define stability testing protocols per product, adapted from ICH guidelines for cosmetics: long-term stability (25 deg C/60% RH for shelf life duration), accelerated stability (40 deg C/75% RH for 6 months), in-use stability (testing after opening simulation)
- QC-029: Generate stability study plans: products to be tested, testing intervals (0, 1, 2, 3, 6, 9, 12, 18, 24, 36 months as applicable), parameters to test at each interval, number of samples required
- QC-030: Track stability sample inventory: samples in stability chambers, samples consumed at each interval, remaining samples
- QC-031: Record stability test results at each interval and compare against initial release results and specification limits
- QC-032: Automatically flag any stability failure and trigger evaluation for potential shelf life reduction, batch recall, or formulation review
- QC-033: Generate stability trend reports showing parameter changes over time for each product and batch

**Preservative Efficacy Testing (PET)**

- QC-034: Schedule preservative efficacy testing per USP <51> or ISO 11930 for each product formulation at defined frequencies: every new formulation, annually for existing products, after formulation change, after packaging change
- QC-035: Record PET results: challenge organisms, inoculation levels, log reduction at each time point, pass/fail against criteria

**Deviation & CAPA Management**

- QC-036: Capture deviations from approved procedures during any operation (production, quality, warehouse, procurement) with: deviation description, area/department, immediate action taken, impact assessment
- QC-037: Classify deviations by severity: critical (potential patient/consumer safety impact), major (significant quality impact), minor (procedural variance with no quality impact)
- QC-038: Route deviation for investigation based on severity: critical requires quality head and plant head; major requires department head and quality; minor can be handled by department supervisor
- QC-039: Track investigation findings, root cause (using tools like 5-Why, fishbone diagram documented in the system), and proposed CAPA
- QC-040: For each CAPA, track: corrective action (immediate fix), preventive action (system/process change to prevent recurrence), responsible person, target completion date, actual completion date, effectiveness check date and result
- QC-041: Escalate overdue CAPAs automatically: 7 days before due to responsible person, on due date to department head, 7 days overdue to quality head, 14 days overdue to plant head/COO

**Change Control**

- QC-042: Implement change control workflow for any change to: formulations, specifications, test methods, procedures, equipment, suppliers, packaging. Change request > impact assessment > multi-department review (quality, production, regulatory, R&D as applicable) > approval > implementation > post-implementation review
- QC-043: Track change control request lifecycle with status, assigned reviewers, review comments, approval history
- QC-044: Link change control records to affected items, BOMs, procedures, and documents for audit trail

**GMP Compliance (IS 22716/ISO 22716, Schedule M-II)**

- QC-045: Maintain personnel training records per employee: training courses completed, dates, trainer, assessment scores, certificate references. Track training due dates and alert when retraining is required
- QC-046: Maintain equipment calibration schedule: equipment ID, calibration parameters, calibration frequency, last calibration date, next calibration due date, calibration performed by (internal/external agency), calibration certificate reference
- QC-047: Block use of equipment past calibration due date in production recording (require calibration completion before equipment can be assigned to a production batch)
- QC-048: Maintain SOP master: SOP number, title, department, current version, effective date, next review date, document owner, approval history. Generate alerts 30 days before SOP review due date
- QC-049: Track SOP acknowledgment by personnel: each relevant employee must electronically acknowledge reading and understanding each SOP version applicable to their role
- QC-050: Maintain cleaning validation records per equipment: cleaning procedure reference, swab/rinse test results, acceptance criteria, validation date, revalidation schedule
- QC-051: Log environmental monitoring results: production area microbial counts (air and surface), temperature and humidity logs, clean room classification verification where applicable

#### Data Entities

- Inspection Plan Master, Inspection Plan Parameters, Sampling Plans
- Inspection Order, Inspection Results, Inspection Disposition Records
- OOS Investigation Records, Investigation Phases, Root Cause Records
- Stability Study Plan, Stability Test Schedule, Stability Test Results
- PET Schedule, PET Results
- Deviation Records, CAPA Records, CAPA Effectiveness Reviews
- Change Control Requests, Change Control Reviews, Change Control Implementation Records
- Training Master (courses), Training Records (per employee), Training Due Dates
- Equipment Master, Calibration Schedule, Calibration Records
- SOP Master, SOP Versions, SOP Acknowledgment Records
- Cleaning Validation Records, Environmental Monitoring Records
- Retain Sample Records
- CoA Templates, Generated CoAs

#### Integration Points

- Inventory module: quarantine hold/release, batch status changes
- Procurement module: incoming inspection trigger, vendor quality score update
- Manufacturing module: in-process and finished goods inspection triggers, batch release
- Regulatory module: stability data for registration files, GMP compliance evidence
- Finance module: quality cost tracking (testing costs, rejection costs, recall costs)
- SharePoint: document storage for SOPs, CoAs, calibration certificates, training records

#### User Roles

| Role | Access |
|------|--------|
| Quality Head | Full access, final approval for batch release, OOS closure, deviation closure, change control approval |
| QC Manager | Inspection management, stability study management, OOS investigation oversight |
| QC Analyst | Test result entry, CoA generation, sample management |
| QA Manager | Deviation management, CAPA management, change control, SOP management, training management, audit management |
| QA Officer | Deviation logging, CAPA tracking, training record maintenance, calibration tracking |
| Microbiologist | Microbial testing, environmental monitoring, PET testing |

#### Dashboards and Reports

- Quality release status dashboard: batches awaiting QC, in testing, released, rejected (today, this week)
- OOS investigation tracker: open investigations, aging, overdue
- CAPA tracker: open CAPAs by department, status, aging, overdue
- Vendor quality performance: rejection rate trend by vendor, CoA compliance rate
- Deviation trend: number of deviations by type, severity, department, month
- Stability study status: active studies, upcoming test points, any failures
- Calibration compliance: equipment due for calibration, overdue equipment
- Training compliance: employees with overdue training, training completion rate by department
- SOP review status: SOPs due for review, overdue reviews
- Inspection turnaround time: average time from sample receipt to result release
- Right first time rate: percentage of batches passing all inspections on first test

---

### 3.6 Sales & Distribution (SD)

**Purpose:** Manage the complete order-to-cash cycle across all five sales verticals (Salon B2B, Modern Trade, E-commerce, General Trade, Export), with channel-specific pricing, credit management, scheme management, logistics coordination, and return handling. Provide real-time visibility into sales performance against targets by vertical, territory, brand, and SKU.

#### Functional Requirements

**Customer Master & Segmentation**

- SD-001: Maintain a unified customer master with: legal name, trade name, GSTIN, PAN, billing address, multiple shipping addresses, contact persons (buyer, accounts, logistics), phone, email, customer category (salon, salon chain, distributor, sub-distributor, modern trade chain, e-commerce marketplace, institutional buyer, export buyer), sales territory, assigned sales representative, payment terms, credit limit, bank details
- SD-002: Support customer hierarchy: parent-child for chain customers (e.g., a salon chain with 50 outlets, each outlet is a child customer under the chain parent)
- SD-003: Segment customers by: channel (Salon B2B, Modern Trade, General Trade, E-commerce, Export), tier (platinum, gold, silver, bronze based on annual purchase value), geography (state, city, territory)
- SD-004: Maintain customer-specific pricing agreements and discount structures linked to the customer master
- SD-005: Track customer registration documents: GSTIN certificate, drug license (if applicable for certain products), FSSAI license (if applicable), export-import code (for export customers), signed distributor agreement

**Pricing & Discount Management**

- SD-006: Maintain base price list (MRP) per finished good SKU
- SD-007: Support channel-wise price lists: salon B2B price list, modern trade price list, general trade price list, export price list (currency-specific: INR, USD, AED, CAD, NPR, BDT)
- SD-008: Support tiered pricing within channels: volume-based price breaks (buy 100-499 units at price X, 500-999 at price Y, 1000+ at price Z), customer-tier-based pricing (platinum customers get additional X% off channel price)
- SD-009: Support promotional pricing with: promotion code, start date, end date, applicable products, applicable customer segments, discount type (percentage off, flat amount off, special price), approval workflow
- SD-010: Calculate net price per order line applying pricing waterfall: base price > channel discount > tier discount > promotional discount > special negotiated price (if applicable), showing the full pricing cascade on the order and invoice
- SD-011: Support minimum price enforcement: system prevents order entry below a floor price per product per channel without finance head override
- SD-012: Support free goods schemes: buy X get Y free (same product), buy X get Z free (different product), with scheme code, validity period, and applicable customer segments

**Scheme Management**

- SD-013: Define and manage trade schemes with: scheme code, scheme name, description, type (quantity discount, value discount, free goods, cash discount, loyalty points), applicable products, applicable customer segments, applicable territories, start date, end date, scheme budget, approval status
- SD-014: Automatically apply eligible schemes during order entry based on order contents and customer profile
- SD-015: Track scheme utilization: number of orders, quantities, discount value given, remaining budget
- SD-016: Block scheme application after scheme budget is exhausted or end date is passed
- SD-017: Report scheme ROI: incremental revenue generated during scheme period versus scheme cost

**Order Management**

- SD-018: Support order entry from multiple sources: sales representative mobile app, B2B portal (customer self-service), EDI from modern trade chains, e-commerce marketplace integration, manual entry by sales coordinator
- SD-019: Validate orders at entry: customer credit check, product availability (against channel allocation), minimum order quantity check, pricing validation, shipping address validation, GSTIN validation
- SD-020: Support order types: standard sales order, sample order (free goods for customer trial), replacement order (against quality complaint), export order (with additional fields: incoterm, port of shipment, country of destination, export documentation requirements)
- SD-021: Manage order lifecycle: draft > confirmed > allocated (inventory reserved) > released for picking > picked > packed > shipped > delivered > invoiced
- SD-022: Support partial shipment against orders: ship available quantity, backorder remaining, with customer notification
- SD-023: Support order hold for: credit issue, pricing dispute, product quality hold, regulatory hold, with reason code and release workflow
- SD-024: Calculate order value, taxes (GST with HSN, for export: IGST at 0% under LUT or with payment of tax), freight charges, and total amount
- SD-025: Generate order confirmation document with all commercial details for customer communication

**Dispatch & Logistics**

- SD-026: Generate pick lists from released orders, optimized by warehouse zone and bin location, following FEFO/FIFO
- SD-027: Support packing operations: record packing details (carton count, carton dimensions, gross weight, net weight), generate packing list
- SD-028: Generate shipping documents: delivery challan, tax invoice, e-way bill (auto-generated via GST portal integration for shipments exceeding Rs 50,000), packing list, lorry receipt / transporter details
- SD-029: Support transporter management: transporter master with vehicle details, rate contracts, GPS tracking integration readiness
- SD-030: Record proof of delivery (POD) with: delivery date, receiver name, receiver signature (digital capture), condition of goods at delivery, any shortage or damage noted
- SD-031: Track shipment status: dispatched > in transit > out for delivery > delivered > POD received
- SD-032: Support dispatch planning: consolidate multiple orders for the same route/destination into single shipments to optimize freight cost
- SD-033: Generate freight cost computation per shipment and allocate to orders for landed cost analysis

**Export Sales**

- SD-034: Support export order processing with: proforma invoice, commercial invoice in foreign currency, packing list, shipping bill, certificate of origin, GMP certificate copy, product registration certificate (per destination country), letter of undertaking (LUT) for GST-free export
- SD-035: Track export documentation workflow: order confirmation > production allocation > QC release > pre-shipment inspection (where required) > customs documentation > shipping > bill of lading/airway bill > bank document submission (for LC shipments) > delivery at destination
- SD-036: Support export pricing in multiple currencies: USD, AED, CAD, NPR, BDT, EUR, with exchange rate management
- SD-037: Track export incentives: duty drawback claims, RODTEP benefits, advance authorization utilization
- SD-038: Maintain country-specific regulatory requirements per product per destination (Nepal, Bangladesh, UAE, Mauritius, Canada) and block export if product registration is not current

**Returns Management**

- SD-039: Process sales returns with: return authorization (RA) workflow, reason code (quality issue, expiry, damage in transit, wrong product shipped, customer order cancellation, shelf life rejection by retailer), return receipt inspection, credit note generation, inventory re-entry (if product is resaleable) or scrap
- SD-040: Track return rates by customer, product, channel, and reason code for trend analysis
- SD-041: Link returns to original invoice and batch for traceability and root cause investigation
- SD-042: For marketplace returns (Amazon, Nykaa): reconcile returned items against marketplace return data, process inventory receipt and grading (resaleable, refurbishable, unsaleable)

**Sales Territory & Representative Management**

- SD-043: Define sales territory hierarchy: zone > region > area > territory, with geographic mapping
- SD-044: Assign sales representatives to territories with target assignment per month, quarter, and year
- SD-045: Track sales representative performance: actual sales vs target, customer coverage (visits planned vs executed), new customer acquisition, collection performance
- SD-046: Support sales representative mobile app integration for: order entry, customer visit logging, photo documentation (store/salon displays), collection recording, beat plan compliance

#### Data Entities

- Customer Master, Customer Hierarchy, Customer Addresses, Customer Contacts
- Price List Master, Price List Lines, Customer-Specific Prices
- Scheme Master, Scheme Rules, Scheme Utilization Records
- Sales Order Header, Sales Order Lines, Sales Order Status Log
- Pick List, Packing List, Shipment Records, POD Records
- Sales Invoice, Credit Note, Debit Note
- Sales Return Authorization, Return Receipt, Return Inspection
- Territory Master, Territory-Representative Assignment, Sales Target Master
- Export Order Documentation, Export Shipment Tracking
- Transporter Master, Freight Rate Master, Freight Cost Records

#### Integration Points

- Inventory module: availability check, reservation, shipment posting, return receipt
- Finance module: invoice posting, credit note, AR, GST, e-invoicing, e-way bill
- E-commerce module: marketplace order import, fulfillment update
- B2B module: salon portal orders, distributor orders
- CRM module: customer relationship data, visit records
- WMS: pick/pack/ship operations
- Power BI: sales dashboards

#### User Roles

| Role | Access |
|------|--------|
| Sales Head | Full access, pricing approval, scheme approval, territory management |
| Regional Sales Manager | Order management and reporting for assigned regions, target monitoring |
| Area Sales Manager | Order management for assigned area, customer management, visit tracking |
| Sales Representative | Order entry (own customers), visit logging, collection recording |
| Sales Coordinator | Order processing, dispatch coordination, documentation |
| Export Manager | Export order management, documentation, pricing, regulatory compliance |
| Sales Analyst | Reporting, target tracking, scheme analysis |

#### Dashboards and Reports

- Daily sales dashboard: orders received, orders shipped, orders pending, value by channel
- Sales vs target: by representative, territory, region, channel, brand, category (daily, MTD, QTD, YTD)
- Order fulfillment rate: orders shipped on time and in full (OTIF) by channel
- Top customers: by revenue, by growth, by margin contribution
- SKU performance: by revenue, by volume, by margin, by channel
- Returns analysis: return rate by customer, product, reason, channel
- Scheme effectiveness: scheme-wise revenue, cost, ROI
- Sales representative scorecard: sales, collection, coverage, new customers
- Export shipment tracker: active export orders with documentation and shipping status
- Channel-wise revenue and margin: daily/weekly/monthly trend
- Dispatch performance: orders dispatched vs target, transit time analysis
- Credit exposure: customers approaching credit limit, overdue receivables by territory

---

### 3.7 E-Commerce Integration (ECOM)

**Purpose:** Integrate all e-commerce channels (Shopify D2C store, Amazon, Nykaa, Blinkit, Zepto, and future marketplaces) with the ERP for real-time inventory synchronization, automated order flow, fulfillment tracking, return processing, and payment reconciliation. Ensure channel-wise inventory allocation is respected and marketplace-specific business rules are handled.

#### Functional Requirements

**Shopify D2C Integration**

- ECOM-001: Bidirectional product sync between ERP and Shopify: product master, pricing, images, descriptions, variants, inventory levels. ERP is the master for pricing and inventory; Shopify is the master for product content and marketing descriptions
- ECOM-002: Real-time inventory push from ERP to Shopify: when available-to-promise (ATP) quantity changes for any D2C-allocated SKU, update Shopify inventory within 5 minutes
- ECOM-003: Order import from Shopify to ERP: upon order creation in Shopify, automatically create sales order in ERP with: customer details, shipping address, line items, pricing, discount codes applied, payment status, shipping method selected
- ECOM-004: Fulfillment update from ERP to Shopify: when order is shipped from warehouse, push tracking number, carrier details, and shipped status to Shopify, triggering customer notification
- ECOM-005: Return/refund processing: when a return is initiated on Shopify, create return authorization in ERP, process return receipt and inspection, trigger refund on Shopify upon approval
- ECOM-006: Payment reconciliation: match Shopify payments (received via Razorpay/Cashfree/Shopify Payments) against orders, accounting for payment gateway commissions, TCS deductions, and net settlement amounts
- ECOM-007: Sync discount codes and promotional pricing from ERP scheme management to Shopify discount engine
- ECOM-008: Handle Cash on Delivery (COD) orders: track COD amount collection by delivery partner, reconcile COD remittance against orders

**Amazon SP-API Integration**

- ECOM-009: Product listing sync: push product data from ERP to Amazon catalog via SP-API, maintaining ASIN mapping, pricing, and inventory
- ECOM-010: Inventory sync to Amazon: update FBM (Fulfilled by Merchant) inventory levels in real time based on ERP Amazon channel allocation; for FBA (Fulfilled by Amazon), track inventory levels at Amazon fulfillment centers via SP-API reports
- ECOM-011: Order import: automatically pull Amazon orders (both FBM and FBA) into ERP as sales orders with: Amazon order ID, buyer details, ASIN-to-SKU mapping, pricing after Amazon fees, shipping details
- ECOM-012: FBM fulfillment: generate pick/pack/ship in ERP, push tracking to Amazon via SP-API for buyer notification
- ECOM-013: FBA inventory management: track inbound shipments to Amazon FCs, reconcile FBA inventory (sellable, unfulfillable, reserved) against ERP records, process FBA return and disposition reports
- ECOM-014: Amazon fee reconciliation: import Amazon settlement reports, reconcile referral fees, FBA fees, storage fees, advertising spend, TCS, and net payout against order-level revenue
- ECOM-015: Handle Amazon deductions: A-to-Z claims, return processing fees, long-term storage fees, reconcile and post to appropriate GL accounts

**Nykaa Integration**

- ECOM-016: Product catalog sync: push product data and inventory levels to Nykaa via their vendor/seller portal API or EDI
- ECOM-017: Order import: pull Nykaa orders into ERP with: Nykaa order ID, buyer details, SKU mapping, pricing, shipping requirements
- ECOM-018: Fulfillment update: push shipment and tracking details to Nykaa upon dispatch
- ECOM-019: Payment reconciliation: reconcile Nykaa settlement reports against orders, accounting for Nykaa commissions, TCS, return deductions, and promotional cost sharing
- ECOM-020: Return processing: import Nykaa return data, process in ERP return workflow, reconcile return credits

**Quick Commerce Integration (Blinkit, Zepto)**

- ECOM-021: Maintain inventory at dark store/warehouse level: push available inventory for quick commerce fulfillment locations
- ECOM-022: Order import: pull orders from Blinkit and Zepto into ERP with channel-specific order details
- ECOM-023: Fulfillment tracking: update dispatch and delivery status
- ECOM-024: Payment reconciliation: reconcile quick commerce platform settlements against orders, accounting for platform commissions, delivery charges, return deductions
- ECOM-025: Manage stock replenishment to quick commerce partner warehouses/dark stores as inter-warehouse transfers in ERP

**Cross-Channel E-Commerce Management**

- ECOM-026: Unified e-commerce order dashboard showing: orders by channel (Shopify, Amazon, Nykaa, Blinkit, Zepto), order status, fulfillment status, payment status, return status
- ECOM-027: Channel-wise inventory allocation enforcement: prevent overselling by limiting available-to-promise per channel to allocated quantity, with safety buffer
- ECOM-028: Centralized product information management (PIM) capability: maintain product content (descriptions, features, ingredients, images) in ERP or connected PIM, syndicate to all channels
- ECOM-029: Handle marketplace-specific GST compliance: TCS collected by marketplaces (Amazon, Nykaa, Flipkart) tracked in ERP, reconciled against GSTR-8 filed by marketplace operators
- ECOM-030: Support marketplace advertising spend tracking: import advertising cost data per campaign per marketplace for profitability analysis
- ECOM-031: Calculate marketplace-specific unit economics per SKU: selling price less marketplace commission less fulfillment fee less return provision less advertising cost per unit less payment gateway fee less TCS, yielding net realization per unit per marketplace

#### Data Entities

- E-Commerce Channel Master, Channel Configuration (API credentials, sync settings)
- Product-Channel Mapping (ERP SKU to marketplace SKU/ASIN/listing ID)
- E-Commerce Order Header, Order Lines, Order Status Log
- Marketplace Settlement Records, Fee Breakdown Records
- Marketplace Return Records, Return Reconciliation
- Channel Inventory Allocation, Inventory Sync Log
- Marketplace Advertising Spend Records

#### Integration Points

- Shopify: REST Admin API and webhooks
- Amazon: SP-API (Selling Partner API) for orders, inventory, fulfillment, financial reports
- Nykaa: vendor portal API or EDI
- Blinkit/Zepto: API or EDI as provided by platforms
- Inventory module: ATP quantities, channel allocation
- Sales module: order creation, fulfillment, returns
- Finance module: payment reconciliation, commission accounting, TCS

#### User Roles

| Role | Access |
|------|--------|
| E-Commerce Head | Full access, channel strategy, pricing decisions, marketplace relationship |
| Marketplace Manager | Order management, listing management, payment reconciliation per marketplace |
| E-Commerce Operations | Order processing, fulfillment coordination, return handling |
| E-Commerce Analyst | Reporting, unit economics analysis, advertising ROI analysis |

#### Dashboards and Reports

- E-commerce orders dashboard: orders by channel, status, value (real-time)
- Marketplace payment reconciliation status: settled vs pending vs disputed by channel
- Channel-wise unit economics report: net realization per SKU per marketplace
- E-commerce return rate analysis: by channel, by product, by reason
- Inventory sync status: last sync time per channel, any sync errors or discrepancies
- Marketplace advertising spend and ROAS (return on ad spend) by product and channel
- FBA inventory reconciliation: ERP vs Amazon inventory discrepancy report
- D2C (Shopify) customer analytics: new vs returning, average order value, customer lifetime value
- Channel revenue comparison: daily/weekly/monthly revenue by marketplace with trend

---

### 3.8 Salon B2B & Channel Management (B2B)

**Purpose:** Manage the salon B2B channel (50,000+ salons) which is the company's core distribution strength. Handle tiered pricing, credit management, scheme management, sales representative territory management, salon portal, and the unique requirements of professional skincare distribution where product education and service support are integral to the sales relationship.

#### Functional Requirements

**Salon Onboarding & Classification**

- B2B-001: Salon onboarding workflow: sales representative lead entry > salon verification (address, existence, license) > credit assessment > tier assignment > pricing assignment > account activation
- B2B-002: Classify salons by tier based on annual purchase value, location category, and service level: Platinum (top 5% by value), Gold (next 15%), Silver (next 30%), Bronze (remaining 50%)
- B2B-003: Maintain salon profile: salon name, owner name, address, contact details, number of chairs/beds, services offered, brands currently stocked (including competitor), average monthly purchase value, payment history, credit utilization
- B2B-004: Track salon lifecycle: prospect > onboarded > active > dormant (no purchase in 90 days) > inactive (no purchase in 180 days) > churned (no purchase in 365 days), with automated alerts to sales representative at each stage transition
- B2B-005: Support salon chain management: group multiple salon outlets under a single chain entity for consolidated pricing, billing, and reporting

**B2B Portal**

- B2B-006: Provide web-based self-service portal for salon owners/managers with: product catalog browsing, price visibility (tier-specific), order placement, order tracking, invoice and statement download, payment history, scheme/offer visibility, new product announcements
- B2B-007: Portal order workflow: salon places order > order validated against credit and availability > auto-confirmed if within limits > or routed to sales representative for approval if outside limits > order fulfillment by warehouse
- B2B-008: Display salon-specific product recommendations based on purchase history and salon category
- B2B-009: Support portal-based grievance and complaint logging with ticket tracking
- B2B-010: Provide salon-wise purchase analytics on the portal: monthly purchase trend, top products purchased, scheme utilization

**Credit Management for Salons**

- B2B-011: Assign credit limit per salon based on tier and payment history: Platinum up to Rs 5,00,000, Gold up to Rs 2,00,000, Silver up to Rs 75,000, Bronze: COD or advance payment only (configurable thresholds)
- B2B-012: System enforces credit limit at order confirmation: if order would cause outstanding balance to exceed credit limit, order is held for approval by regional sales manager or finance
- B2B-013: Track salon-wise credit utilization: current outstanding, overdue amount, overdue days, available credit
- B2B-014: Support dynamic credit limit adjustment: automatic increase for consistently on-time payers (after 6 months of perfect payment), automatic decrease or hold for chronic late payers
- B2B-015: Generate salon credit aging report in standard aging buckets (current, 30, 60, 90, 120+ days)
- B2B-016: Support credit insurance integration readiness for high-value salon chain accounts

**Salon Scheme & Promotion Management**

- B2B-017: Define salon-specific schemes: introductory kit offers for new salons, loyalty schemes based on cumulative purchase, festival/seasonal schemes, product launch schemes, counter display incentives
- B2B-018: Support salon loyalty program: accumulate loyalty points based on purchase value, redeem points for products or services (training sessions, salon branding materials)
- B2B-019: Manage salon demo kits and sample allocation: track demo kit issue per salon, replenishment schedule, return tracking
- B2B-020: Calculate scheme profitability per salon: incremental revenue generated versus scheme cost

**Sales Territory & Beat Management**

- B2B-021: Define beat plans for sales representatives: daily route listing which salons to visit on which day, optimized for geography
- B2B-022: Track beat plan compliance: planned visits vs actual visits, with GPS-based check-in validation from mobile app
- B2B-023: Record visit outcomes: order placed, product training delivered, complaint addressed, competitive intelligence gathered, display/merchandising checked
- B2B-024: Target management: set monthly and quarterly targets per sales representative by: total revenue, new salon acquisition count, dormant salon reactivation, product-wise targets (push specific focus products), collection targets
- B2B-025: Commission and incentive calculation based on: revenue achievement percentage, collection percentage, new salon acquisition, focus product sales, with tiered incentive slabs

**Training & Professional Support**

- B2B-026: Track salon-level product training: which products/treatments the salon staff has been trained on, training date, trainer name, training type (in-salon, batch training at O3+ center, digital/video)
- B2B-027: Link training completion to product eligibility: certain professional-only products require salon staff training certification before the salon can order those products
- B2B-028: Schedule and track salon training events: planned, completed, upcoming, with attendance records

#### Data Entities

- Salon Master (extended Customer Master), Salon Tier, Salon Lifecycle Status
- Salon Chain Master, Chain-Salon Hierarchy
- Beat Plan Master, Beat Plan Schedule, Visit Records
- Salon Credit Profile, Credit Limit History, Credit Aging Records
- Salon Scheme Master, Scheme Eligibility Rules, Scheme Utilization
- Salon Loyalty Points Ledger
- Demo Kit Issue/Return Records
- Sales Representative Target Master, Commission Calculation Records
- Training Schedule, Training Records per Salon, Training Certification

#### Integration Points

- Sales module: order processing, invoicing, returns
- Finance module: AR, credit management, collection tracking
- Inventory module: channel allocation for salon B2B
- CRM module: salon relationship management, complaint tracking
- Mobile App: sales representative order entry, visit logging, GPS check-in
- B2B Portal: web-based salon self-service

#### User Roles

| Role | Access |
|------|--------|
| Salon Business Head | Full access, channel strategy, pricing policy, scheme approval, target setting |
| Regional Sales Manager | Territory management for region, target monitoring, credit approval, scheme approval |
| Area Sales Manager | Beat plan management for area, daily visit monitoring, order management |
| Sales Representative | Order entry, visit logging, salon profile update, collection recording |
| Salon Support Coordinator | Portal management, complaint handling, training scheduling |
| B2B Analyst | Reporting, target tracking, scheme analysis, credit analysis |

#### Dashboards and Reports

- Salon channel revenue dashboard: daily/weekly/monthly revenue, orders, average order value
- Sales representative performance scorecard: revenue vs target, visits vs plan, collection vs target, new salons
- Beat compliance dashboard: planned vs actual visits by representative and territory
- Salon tier distribution: number and revenue by tier, tier migration trend
- Credit exposure dashboard: total credit outstanding, overdue analysis by territory and tier
- Salon lifecycle dashboard: active, dormant, inactive, churned counts with trend
- Scheme effectiveness report: per scheme revenue impact, cost, ROI
- Top salons by revenue, by growth, by consistency
- Dormant salon reactivation report: salons approaching dormancy, reactivation attempts and outcomes
- Training coverage: percentage of salons trained on each product category
- Demo kit utilization: kits issued vs orders generated

---

### 3.9 Human Resources & Performance Management (HR)

**Purpose:** Manage organizational structure, employee lifecycle, JD/KRA/KPI framework, performance evaluation, task sheet management, training identification, and hiring planning. While the primary HRMS platform is Darwinbox, certain workforce planning and performance data must integrate with the ERP for cost allocation and business planning.

#### Functional Requirements

**Organizational Structure**

- HR-001: Maintain organizational hierarchy: company > division > department > sub-department > team > individual, mapped to the business plan structure
- HR-002: Define departments aligned with O3+ operations: Sales (Salon B2B, Modern Trade, E-commerce, General Trade, Export), Marketing, Brand Management (per brand), R&D/Formulation, Supply Chain (procurement, planning, logistics), Production, Quality (QC + QA), Finance & Accounts, Human Resources, IT, Administration
- HR-003: Maintain position master with: position title, department, sub-department, reporting line, grade/band, salary range, JD, KRA, KPI, required competencies, headcount (budgeted positions vs filled positions)
- HR-004: Visually map the organizational structure showing filled and vacant positions at each level

**JD/KRA/KPI Framework**

- HR-005: For every position, maintain a detailed Job Description tied to the business plan: not generic HR templates, but specific descriptions explaining what this role delivers in the context of O3+'s targets. Example: "Brand Manager, DermaCult" JD specifies DermaCult's channel mix, pricing strategy, and target consumer, not a generic brand management JD
- HR-006: Define 3-5 KRAs (Key Result Areas) per position, each directly linked to departmental targets which cascade from the 3-year business plan
- HR-007: Define measurable KPIs per KRA with: metric definition, measurement frequency (daily, weekly, monthly), data source (ERP report, MIS dashboard, manual input), target value, threshold values (exceeds expectation, meets expectation, below expectation, unacceptable)
- HR-008: Track KPI actuals against targets with automated data pull where possible (e.g., sales KPIs pulled from ERP sales data, production KPIs pulled from manufacturing module, fill rate from warehouse data)
- HR-009: Support KRA/KPI revision process: annual review during budget cycle, with change approval from department head and HR head

**Task Sheet System**

- HR-010: From the business plan, generate department-level task sheets: monthly and quarterly deliverables for each department, broken down from the annual plan
- HR-011: From department task sheets, derive individual weekly task sheets: specific tasks assigned to each team member, with: task description, expected output, due date, priority, linkage to KRA
- HR-012: Track task completion: employees mark tasks as complete with evidence/deliverable attachment, supervisors verify and approve
- HR-013: AI scan layer (see AI/ML section) continuously monitors task sheet completion rates, quality of deliverables, and timeliness across all employees
- HR-014: Generate weekly task sheet compliance report per team and individual: tasks assigned, completed on time, completed late, incomplete, quality of completion

**Performance Evaluation & Review**

- HR-015: Implement quarterly performance review cycle in Darwinbox: self-assessment > manager assessment > skip-level review > calibration session > final rating
- HR-016: Performance rating based on: KPI achievement (weighted 60%), behavioral competencies (weighted 20%), peer feedback (weighted 10%), task sheet compliance (weighted 10%)
- HR-017: Categorize employees based on performance: high performer (top 15%), solid performer (next 55%), needs improvement (next 20%), underperformer (bottom 10%)
- HR-018: For "needs improvement" employees: automatically generate training plan and improvement timeline (60-90 days)
- HR-019: For "underperformer" employees after intervention: if no improvement in two consecutive review cycles with documented support and training, trigger performance improvement plan (PIP) with clear exit criteria
- HR-020: Generate nine-box grid mapping (performance vs potential) for leadership pipeline planning

**Training & Development**

- HR-021: Identify training needs from three sources: performance gaps flagged in reviews, new skill requirements from business plan initiatives, mandatory compliance training (GMP, quality, safety)
- HR-022: Maintain training calendar: internal training sessions, external workshops, online courses, vendor-conducted product training, GMP training
- HR-023: Track training completion per employee with: course name, date, duration, trainer, assessment score, certificate reference
- HR-024: Measure training effectiveness: pre-training and post-training assessment scores, performance KPI improvement post-training
- HR-025: Link training completion to competency matrix: track which competencies each employee has been trained on and certified for

**Hiring Planning**

- HR-026: Derive hiring requirements from: budgeted positions minus current headcount, approved new positions from business plan expansion, attrition replacement (based on historical attrition rate by department)
- HR-027: Track hiring pipeline per position: vacancy created > job posted > applications received > shortlisted > interview scheduled > offer made > offer accepted > joining date > onboarded
- HR-028: Calculate hiring cost per position: recruitment agency fees, job posting costs, interview expenses, onboarding costs
- HR-029: Generate monthly hiring dashboard: open positions, pipeline status, time-to-fill by department, cost-to-fill

**Attendance, Leave & Payroll** (in Darwinbox, integrated with ERP)

- HR-030: Track attendance across all locations (Parwanoo plant, Noida HQ, field sales) with biometric/geo-fencing integration
- HR-031: Manage leave types per Indian labor law and company policy: casual leave, sick leave, earned leave, maternity/paternity leave, compensatory off, public holidays (state-specific for HP and UP)
- HR-032: Process monthly payroll with all statutory components: basic, HRA, special allowance, PF (employee + employer), ESI (where applicable), professional tax (state-specific), TDS per Section 192 with tax planning declarations, gratuity accrual
- HR-033: Generate payslips, Form 16, PF ECR, ESI contribution statements

**Integration with ERP for Cost Allocation**

- HR-034: Push department-wise and cost-center-wise employee costs from Darwinbox/payroll to ERP finance module monthly for accurate departmental P&L
- HR-035: Allocate labor cost to production batches based on actual hours logged by production personnel, feeding into batch costing

#### Data Entities

- Organization Hierarchy, Department Master, Sub-Department Master
- Position Master, JD/KRA/KPI Records
- Employee Master (in Darwinbox), Employee-Position Assignment
- Task Sheet Templates, Individual Task Assignments, Task Completion Records
- Performance Review Records, Rating History, PIP Records
- Training Master (courses), Training Calendar, Training Records, Competency Matrix
- Hiring Requisition, Hiring Pipeline Records
- Attendance Records, Leave Records, Payroll Records

#### Integration Points

- Darwinbox: primary HRMS platform for employee lifecycle, attendance, payroll, reviews
- ERP Finance module: employee cost allocation, labor cost posting
- ERP Manufacturing module: labor hours for batch costing
- AI/ML layer: task sheet scanning, performance pattern analysis
- Power BI: HR dashboards

#### User Roles

| Role | Access |
|------|--------|
| HR Head | Full access to all HR functions, policy management, compensation decisions |
| HR Manager | Employee lifecycle management, training administration, hiring coordination |
| Department Head | View team JD/KRA/KPI, approve task sheets, conduct reviews, raise hiring requests |
| Employee (Self-Service) | View own profile, mark tasks, self-assessment, leave application, payslip download |
| HR Analyst | Reporting, headcount analysis, attrition analysis, training effectiveness analysis |

#### Dashboards and Reports

- Organizational structure dashboard: total headcount, department-wise, filled vs budgeted positions
- Performance distribution: rating distribution by department, nine-box grid
- Task sheet compliance: weekly completion rate by team and individual
- Training calendar and completion tracker
- Hiring pipeline status: open positions, time-to-fill, cost-to-fill
- Attrition dashboard: monthly attrition rate by department, tenure band, reason analysis
- Employee cost analysis: department-wise, cost-center-wise, month-over-month trend
- KPI achievement heatmap: department and individual level KPI status (green/amber/red)

---

### 3.10 Product Lifecycle Management (PLM)

**Purpose:** Manage the end-to-end product lifecycle from concept through development, approval, launch, active life, and eventual discontinuation. Integrate brand management, R&D, regulatory, and commercial functions into a structured product development workflow that ensures every new product has a validated business case, approved formulation, regulatory clearance, and go-to-market plan before entering production.

#### Functional Requirements

**Product Concept & Business Case**

- PLM-001: Create product concept records with: brand assignment, product category, target segment, proposed positioning, competitive analysis findings, market survey data, target consumer profile, proposed price architecture (MRP, channel-wise pricing), margin projection per channel
- PLM-002: Require brand manager sign-off on product concept with documented business case before R&D development begins
- PLM-003: Define the master product and satellite product family strategy per segment: identify the anchor SKU (differentiated, highly consumable) and plan the supporting push-pull products that create the product family
- PLM-004: Assess cannibalization risk: does the new product overlap with existing SKUs in the same channel, and if so, what is the net revenue impact

**Formulation Development**

- PLM-005: Track R&D formulation development stages: brief received > lab-scale formulation > stability assessment > pilot batch > scale-up trial > commercial batch approval
- PLM-006: Record multiple formulation options per product concept with: ingredient list, percentage composition, active ingredient USPs, cost estimate, regulatory status of each ingredient
- PLM-007: Track ingredient regulatory status per target market: DCGI/BIS approved for India, FDA/MoCRA compliant for US, EU 1223/2009 compliant for EU, and per destination country for exports
- PLM-008: Manage formulation approval workflow: R&D recommends > quality assesses (stability, micro, regulatory) > brand management assesses (sensory, packaging compatibility, cost alignment) > management approval
- PLM-009: Link approved formulation to BOM creation in manufacturing module

**Packaging Development**

- PLM-010: Track packaging development stages: brief > design concepts > artwork development > regulatory text review > pre-press > prototype > testing (compatibility, drop, leak) > approval > tooling/die development > production-ready
- PLM-011: Design packaging for multi-use potential: identify where bottle formats, tube sizes, jar specifications, and carton dimensions can serve across multiple products or brands to gain procurement leverage
- PLM-012: Maintain artwork version control: every label/carton artwork has a version number, approval date, and link to the regulatory text approval
- PLM-013: Ensure packaging carries all mandatory declarations per Drugs & Cosmetics Rules and Legal Metrology Act: ingredient list (INCI nomenclature), net content, MRP, manufacturing date, expiry/best before, manufacturer name and address, batch number, country of origin, allergen warnings

**Regulatory Clearance**

- PLM-014: Track product registration requirements per market: BIS certification (IS 4707 Part 1 for skin cosmetics, Part 2 for hair cosmetics), CDSCO notification for cosmetics containing specified ingredients, PETA/cruelty-free certification, FDA/MoCRA registration for US market, EU CPNP notification for EU market
- PLM-015: Manage product registration applications: application filed > under review > queries received > queries responded > approved > registration certificate issued
- PLM-016: Track registration certificate validity and renewal timelines per product per market
- PLM-017: Block production or export of a product to any market where registration is not current

**Product Launch Planning**

- PLM-018: Create product launch plans with: target launch date, initial production quantity, initial distribution plan (which channels, which territories), marketing plan (digital, offline, trade marketing), sales training schedule, sample kit requirements, listing plan for each marketplace
- PLM-019: Track launch readiness checklist: formulation approved, stability data sufficient, BOM created, packaging approved and available, regulatory clearance obtained, pricing finalized, marketing materials ready, sales team trained, marketplace listings prepared, initial stock produced and QC-released
- PLM-020: Support launch delay management: when any checklist item is not ready by target date, automatically alert the product launch team and project stakeholders

**SKU Rationalization**

- PLM-021: Report on SKU performance: revenue contribution, margin contribution, inventory turns, channel penetration, year-over-year trend for every active SKU
- PLM-022: Flag underperforming SKUs based on configurable criteria: bottom 10% by revenue, negative margin, less than X units sold in 12 months, declining trend for 3+ consecutive quarters
- PLM-023: SKU discontinuation workflow: flagged for review > commercial assessment > impact analysis (salon/customer dependency, inventory clearance plan) > discontinuation approval > clearance period > discontinued status > removed from active catalog

**Product Pricing Management**

- PLM-024: Maintain pricing architecture per SKU: MRP, recommended retail price, channel-wise trade price, cost-to-company, margin at each level (manufacturer margin, distributor margin, retailer margin)
- PLM-025: Support pricing simulation: model margin impact of price changes across channels before implementation
- PLM-026: Track competitive pricing: maintain competitor product pricing data for benchmarking
- PLM-027: Generate cross-brand formulation overlap report: identify where two or more brands share similar base formulations (common ingredient profiles above configurable similarity threshold), flag opportunities for base formulation standardization to consolidate raw material procurement and reduce production changeover time
- PLM-028: Track formulation standardization initiatives: when a common base formulation is adopted across brands, record the original per-brand formulations, the standardized version, projected procurement savings, and actual savings realized

#### Data Entities

- Product Concept Records, Business Case Documents
- Formulation Development Records, Formulation Options, Formulation Approvals
- Packaging Development Records, Artwork Versions, Packaging Test Results
- Product Registration Records (per market), Registration Certificates
- Product Launch Plans, Launch Checklists, Launch Status Logs
- SKU Performance Records, SKU Rationalization Reviews
- Pricing Architecture Records, Pricing Simulation Records, Competitive Pricing Data

#### Integration Points

- Manufacturing module: BOM creation from approved formulations
- Quality module: stability testing linkage, regulatory compliance data
- Regulatory module: registration tracking
- Sales module: pricing, product catalog
- Marketing: launch plans, promotional strategy
- SCM module: demand planning for new product launch quantities

#### User Roles

| Role | Access |
|------|--------|
| Brand Head | Full access to brand-specific products, concept approval, pricing decisions |
| Brand Manager | Product concept creation, business case development, launch planning |
| R&D Head | Formulation management, formulation approval, ingredient management |
| R&D Scientist | Formulation development, lab records, stability evaluation |
| Packaging Development Manager | Packaging design management, artwork approval, tooling tracking |
| Regulatory Affairs Manager | Registration management, regulatory clearance tracking |
| Product Analyst | SKU performance analysis, rationalization recommendations, pricing analysis |

#### Dashboards and Reports

- Product development pipeline: products at each stage from concept to launch
- New product launch tracker: upcoming launches with readiness status
- SKU rationalization dashboard: flagged underperformers with key metrics
- Product portfolio analysis: revenue/margin contribution by brand, category, product family
- Formulation cost analysis: cost per kg by product category with trend
- Registration status tracker: products pending registration per market
- Competitive pricing comparison: O3+ vs competition by category and channel

---

### 3.11 Supply Chain Planning & Forecasting (SCM)

**Purpose:** Create an integrated planning layer that connects sales forecasts to production plans to procurement plans to logistics plans, ensuring the entire supply chain operates on a single agreed demand signal rather than fragmented departmental estimates. This module eliminates ad-hoc procurement and production that currently leads to excess inventory and aging issues.

#### Functional Requirements

**Demand Forecasting**

- SCM-001: Generate demand forecasts by SKU, by month, for a rolling 12-month horizon, combining: historical sales data (weighted moving average of last 12 months with seasonal adjustment), sales team input (bottom-up territory-wise estimates), marketing calendar (planned promotions, new product launches, scheme periods), channel-specific trends (e-commerce growth rates, salon penetration targets, modern trade listing plans)
- SCM-002: Support forecast adjustments with audit trail: original statistical forecast > sales team adjustment > marketing adjustment > final consensus forecast, with each adjustment documented with reason
- SCM-003: Calculate forecast accuracy metrics: MAPE (Mean Absolute Percentage Error), bias (systematic over or under forecasting), and track accuracy trend by SKU, category, and channel
- SCM-004: Flag products with poor forecast accuracy (MAPE above 40%) for review and root cause analysis
- SCM-005: Support new product forecast with no history: use proxy product analogy (similar product launch performance), marketing team estimate, and initial order pipeline

**Supply Planning**

- SCM-006: Convert demand forecast into production plan: net the demand forecast against current finished goods inventory, in-transit stock, and committed production orders to calculate net production requirement by SKU by period
- SCM-007: Generate master production schedule (MPS) from net requirements, considering: production capacity (per manufacturing line per shift), batch size constraints (minimum batch size, batch size multiples), product sequencing for changeover optimization, campaign production grouping
- SCM-008: From MPS, generate material requirements plan (MRP): explode production orders through BOMs to calculate gross material requirements, net against current material inventory and pending purchase orders, generate planned purchase requisitions with required-by dates accounting for procurement lead times
- SCM-009: Run MRP at least weekly; support on-demand MRP for urgent requirement changes
- SCM-010: Highlight supply risks: materials with insufficient inventory and no pending PO to cover requirements within lead time, single-source materials, materials with quality holds

**Inventory Planning**

- SCM-011: Calculate and maintain optimal inventory levels per SKU per location: safety stock (based on demand variability and lead time variability), reorder point, maximum stock level, economic order quantity
- SCM-012: Generate inventory health report showing: days of stock on hand per SKU, slow-moving items (inventory exceeding X days of demand), fast-moving items at risk of stockout (inventory below safety stock)
- SCM-013: Plan inventory positioning across locations for multi-channel fulfillment: how much FG inventory should be held at Parwanoo, at dispatch warehouse, at Amazon FBA, at quick commerce partner locations

**Capacity Planning**

- SCM-014: Maintain manufacturing capacity master: available capacity per production line per shift per day (in units or kg), number of shifts, working calendar, planned maintenance windows
- SCM-015: Generate capacity utilization report: planned production load vs available capacity by production line by week, flagging over-capacity periods that need overtime, additional shift, or outsourcing
- SCM-016: Support what-if capacity scenarios: model impact of adding a shift, adding a production line, or subcontracting specific products

**Distribution Planning**

- SCM-017: Plan distribution of finished goods from manufacturing to fulfillment points: dispatch warehouse, Amazon FBA warehouse, quick commerce partner locations, regional transit warehouses (if any)
- SCM-018: Optimize dispatch scheduling to minimize freight cost while meeting delivery commitments: consolidate shipments, optimize truck loading, plan routes

**Sales & Operations Planning (S&OP)**

- SCM-019: Support monthly S&OP review process with structured data: demand plan, supply plan, inventory plan, financial reconciliation (planned revenue, cost, margin), exception items requiring management decision
- SCM-020: Generate S&OP meeting pack automatically from system data: demand vs supply alignment, production plan adherence last month, inventory health, capacity utilization, procurement status, new product launch status
- SCM-021: Track S&OP decision log: decisions made in each monthly review, action items assigned, completion tracking
- SCM-022: Support phased facility consolidation towards Himachal: maintain multiple warehouse/plant locations during the transition period, track inter-facility transfer volumes and costs, monitor consolidation milestones (which product lines have been moved, which remain), generate consolidation progress dashboard showing: percentage of production consolidated, cost savings realized vs projected, remaining timeline, and any dependencies blocking the next consolidation phase
- SCM-023: Provide facility decommissioning workflow: when a facility or warehouse location is being consolidated out, manage the wind-down process (inventory drawdown plan, asset transfer, personnel reallocation tracking, lease/contract termination dates), prevent new inventory from being allocated to decommissioning locations, and archive the location's historical data

#### Data Entities

- Demand Forecast Master, Forecast Versions, Forecast Adjustments
- Master Production Schedule, MPS Line Items
- Material Requirements Plan, MRP Output (planned requisitions)
- Inventory Planning Parameters (safety stock, reorder point, EOQ per item per location)
- Capacity Master, Capacity Calendar, Capacity Load Records
- S&OP Meeting Records, Decision Logs, Action Items

#### Integration Points

- Sales module: historical sales data, order pipeline
- Manufacturing module: production capacity, production orders, BOM explosion
- Procurement module: purchase requisition generation, lead times, pending POs
- Inventory module: current stock positions, channel allocations
- Finance module: plan-to-P&L reconciliation
- Power BI: planning dashboards

#### User Roles

| Role | Access |
|------|--------|
| Supply Chain Head | Full access, S&OP facilitation, planning parameter decisions |
| Demand Planner | Forecast creation and adjustment, accuracy tracking, consensus process |
| Supply Planner | MPS creation, MRP execution, capacity planning, supply risk management |
| Inventory Planner | Inventory parameter management, stock health monitoring, allocation planning |
| S&OP Coordinator | Meeting pack preparation, decision log maintenance, action tracking |

#### Dashboards and Reports

- Demand forecast dashboard: forecast by SKU, brand, channel with trend and accuracy metrics
- Supply plan dashboard: production plan vs demand, supply gaps highlighted
- Inventory health dashboard: days of supply by SKU, aging inventory, stockout risk
- Capacity utilization: planned load vs available capacity by line by week
- MRP status: planned requisitions pending conversion to PO, material availability risks
- Forecast accuracy trend: MAPE by product category and channel, monthly trend
- S&OP scorecard: plan adherence, inventory turns, order fill rate, forecast accuracy

---

### 3.12 Regulatory & Compliance Management (REG)

**Purpose:** Track all regulatory requirements applicable to O3+ as a cosmetic manufacturer and exporter, manage product registrations across markets, ensure labeling compliance, and maintain audit-ready documentation for all regulatory bodies.

#### Functional Requirements

**Indian Regulatory Compliance**

- REG-001: Track BIS (Bureau of Indian Standards) compliance for cosmetics under IS 4707 (Part 1: Skin Cosmetics, Part 2: Hair Cosmetics): maintain list of products requiring BIS certification, track certification status, certificate number, validity period, renewal timelines
- REG-002: Track CDSCO (Central Drugs Standard Control Organisation) compliance under the Drugs & Cosmetics Act 1940 and Rules 1945: product-wise classification (cosmetic vs cosmetic-drug), manufacturing license status (Form 32), import registration for imported ingredients where applicable
- REG-003: Maintain manufacturing license records: Form 32 license number, validity, renewal timeline, conditions, inspections
- REG-004: Track compliance with Legal Metrology Act for product labeling: MRP declaration, net content, manufacturer details, customer care details
- REG-005: Maintain FSSAI registration/license status if any products fall under food category (lip care products potentially)

**Export Market Regulatory Compliance**

- REG-006: Track FDA/MoCRA (Modernization of Cosmetics Regulation Act) compliance for US exports: FDA establishment registration, product listing, adverse event reporting readiness, GMP compliance documentation, fragrance allergen disclosure
- REG-007: Track EU 1223/2009 compliance for EU/UK exports: responsible person designation, CPNP (Cosmetic Products Notification Portal) notification per product, Product Information File (PIF) maintenance, Safety Assessment report per product, CPSR (Cosmetic Product Safety Report) availability
- REG-008: Track country-specific registration for current export markets: Nepal (DDA registration), Bangladesh (DGDA registration), UAE (Emirates Authority for Standardization and Metrology), Mauritius (Ministry of Health), Canada (Health Canada, Cosmetic Notification Form)
- REG-009: Maintain country-specific labeling requirements: language requirements, ingredient naming conventions, import markings, barcode requirements (per country)
- REG-010: Block export shipment creation for any product-country combination where registration is expired or not obtained

**Cruelty-Free & Sustainability Compliance**

- REG-011: Track PETA cruelty-free certification status: certification date, renewal timeline, compliance evidence (no animal testing declaration, supplier animal testing status)
- REG-012: Maintain supplier-level cruelty-free declarations: collect and store animal testing status declarations from all raw material and ingredient suppliers
- REG-013: Track any other sustainability certifications: vegan certification, organic certification (where applicable), environmental compliance

**Ingredient Regulatory Database**

- REG-014: Maintain a regulatory database for every ingredient used in O3+ formulations with: CAS number, INCI name, regulatory status in each target market (India, US, EU, UAE, Nepal, Bangladesh, Mauritius, Canada), maximum permitted concentration, restricted/prohibited status, required warnings, allergen classification
- REG-015: Automatically flag during formulation development if a proposed ingredient is restricted or prohibited in any target market
- REG-016: Track changes in ingredient regulations across markets and alert R&D and regulatory when regulations change that affect current formulations

**Audit & Inspection Management**

- REG-017: Maintain audit calendar: scheduled audits by regulatory bodies (BIS, CDSCO, FDA, state drug inspectors), customer audits, internal audits, ISO audits, GMP audits
- REG-018: Track audit findings, observations, non-conformances with response tracking and closure verification
- REG-019: Maintain audit-readiness checklist per regulatory body with continuous compliance status monitoring

#### Data Entities

- Regulatory Body Master, Regulation Master
- Product Registration Records (per product per market), Registration Certificates, Renewal Schedules
- Manufacturing License Records, Facility Registration Records
- Ingredient Regulatory Database, Ingredient-Market Compliance Matrix
- Cruelty-Free Declarations (supplier level), Certification Records
- Audit Calendar, Audit Records, Audit Findings, Corrective Actions
- Country-Specific Labeling Requirements Master

#### Integration Points

- PLM module: formulation ingredient checking against regulatory database
- Quality module: GMP compliance evidence, deviation/CAPA records for audits
- Sales module: export shipment blocking for unregistered products
- Manufacturing module: manufacturing license compliance
- Procurement module: supplier cruelty-free declaration tracking

#### User Roles

| Role | Access |
|------|--------|
| Regulatory Affairs Head | Full access, registration strategy, regulatory body liaison, audit management |
| Regulatory Affairs Manager | Registration application management, compliance tracking, audit preparation |
| Regulatory Affairs Officer | Document maintenance, ingredient database updates, labeling compliance checks |

#### Dashboards and Reports

- Registration status dashboard: all products across all markets, registration validity, upcoming renewals
- Compliance calendar: upcoming deadlines for renewals, filings, audits
- Ingredient regulatory alert: new restrictions or changes affecting current formulations
- Audit tracker: upcoming audits, open findings, overdue corrective actions
- Export compliance matrix: product-country registration status heat map
- License and certification validity tracker: manufacturing license, BIS, PETA, ISO, GMP

---

### 3.13 MIS & Business Intelligence (MIS)

**Purpose:** Deliver real-time and scheduled management information across all business functions through dashboards and reports. Enable leadership (Vineet, Vidur, COO, department heads) to see exactly where things stand at any point without waiting for manual compilation. This is the visibility layer that makes the entire system useful for decision-making.

#### Functional Requirements

**Dashboard Infrastructure**

- MIS-001: Implement Power BI Premium as the BI platform, embedded within the D365 Business Central environment and accessible via web browser and mobile app
- MIS-002: Configure automated data refresh from ERP to Power BI: critical dashboards (production, dispatch, inventory, sales, cash) refresh every 15 minutes during working hours; detailed analytical reports refresh daily at 6:00 AM IST
- MIS-003: Implement role-based dashboard access: MD/Director level sees consolidated cross-functional KPIs; COO sees operational detail across all departments; department heads see deep drill-down within their function
- MIS-004: Support dashboard drill-down: from summary KPI to detailed transaction-level data in maximum 3 clicks
- MIS-005: Enable dashboard access on mobile devices (iOS and Android) via Power BI mobile app for leadership who need to monitor on the go
- MIS-006: Configure automated email delivery of key daily and weekly reports to designated recipients in PDF format

**Daily MIS Reports**

- MIS-007: Daily Production Report: products produced today, batch numbers, quantities, yield percentages, variance from plan, cumulative MTD production vs plan, downtime events, quality holds
- MIS-008: Daily Dispatch Report: orders dispatched today, quantities, destinations, transporters, cumulative MTD dispatches vs plan, pending orders backlog count and value, orders delayed beyond committed delivery date
- MIS-009: Daily Sales Report: orders received today by channel (Salon B2B, Modern Trade, E-commerce by marketplace, General Trade, Export), value, top products sold, comparison with same day last week and last year
- MIS-010: Daily Inventory Position: current stock of finished goods by SKU (in units and value), stock days remaining (current stock divided by average daily demand), items below safety stock flagged in red, items in quarantine (pending QC)
- MIS-011: Daily Cash Position: opening bank balance, receipts (customer collections, marketplace settlements), payments (vendor payments, salaries, other expenses), closing balance, projected cash position for next 7 days
- MIS-012: Daily E-Commerce Dashboard: orders by marketplace, GMV (Gross Merchandise Value), returns processed, payment settlements received, inventory sync status

**Weekly MIS Reports**

- MIS-013: Weekly Planning vs Actual: production plan vs actual (SKU-wise), procurement plan vs actual (delivery receipts vs expected), dispatch plan vs actual, sales forecast vs actual orders
- MIS-014: Weekly Quality Report: batches tested, pass/fail rate, open OOS investigations, open deviations, CAPA status, incoming material acceptance rate
- MIS-015: Weekly Order Fulfillment: OTIF (On Time In Full) rate by channel, backorder aging, reasons for non-fulfillment (stock unavailability, QC hold, credit hold, logistics delay)
- MIS-016: Weekly AR Aging Report: total receivables, aging distribution, overdue amount, top 20 overdue customers, collection performance by territory
- MIS-017: Weekly Sales Representative Performance: orders booked, revenue generated, visits completed, collection achieved, new salons onboarded, against weekly targets
- MIS-018: Weekly Inventory Aging Report: inventory distribution across aging bands (0-6m, 6-12m, 12-24m, 24m+) for RM, PM, and FG separately, with comparison to previous week and trend

**Monthly MIS Reports**

- MIS-019: Monthly P&L by Channel: revenue, COGS, gross margin, direct channel costs (commissions, marketplace fees, freight, schemes), channel contribution margin, indirect cost allocation, channel operating profit for each channel: Salon B2B, Modern Trade, E-commerce (split by marketplace), General Trade, Export
- MIS-020: Monthly P&L by Brand: revenue, COGS, gross margin, brand-specific costs (marketing, brand management, product development amortization), brand contribution margin for each brand: O3+ Professional, DermaCult, Sara Beauty, Agelock, Biozoma, O3+ x Mijoo, Laamis
- MIS-021: Monthly P&L by Category: revenue, COGS, gross margin by product category (facial kits, serums, sunscreens, cleansers, creams, masks, toners, body care, hair care)
- MIS-022: Monthly SKU Profitability Report: for every active SKU, show: revenue, units sold, manufacturing cost, landed cost (channel-wise), margin (channel-wise), inventory turns, channel mix
- MIS-023: Monthly Inventory Turns Report: inventory turnover ratio by item category (RM, PM, FG), by brand, by product category, benchmarked against targets
- MIS-024: Monthly Procurement Report: PO value by vendor category, purchase price variance, vendor delivery performance, import shipment status, procurement spend vs budget
- MIS-025: Monthly Manufacturing Report: production volume vs plan, capacity utilization, OEE by line, yield trends, downtime analysis, batch cost variance, work-in-process value
- MIS-026: Monthly Cash Flow Report: operating cash flow (collections minus payments), investing cash flow (capex, deposits), financing cash flow (loan disbursement/repayment), net cash flow, closing balance, 3-month cash flow forecast
- MIS-027: Monthly GST Reconciliation Report: output tax liability, ITC claimed, net GST paid, GSTR-2A/2B reconciliation status (matched, unmatched, excess ITC claimed)
- MIS-028: Monthly HR Dashboard: headcount by department, new hires, separations, attrition rate, training hours per employee, performance distribution, open positions

**Executive Scorecards**

- MIS-029: MD/Director Monthly Scorecard (single page): revenue (actual vs budget vs last year), gross margin %, EBITDA %, net profit %, inventory days, receivable days, payable days, cash balance, top 5 action items from S&OP, key risks flagged
- MIS-030: COO Weekly Operational Scorecard: production OTIF, dispatch OTIF, quality right-first-time %, procurement lead time adherence, warehouse FIFO compliance, MIS data accuracy score, top 5 operational issues and actions

**Custom Report Builder**

- MIS-031: Provide a self-service report builder in Power BI for finance and analyst users to create ad-hoc reports without IT intervention
- MIS-032: Maintain a report catalog (see Appendix) with: report name, description, frequency, data source, recipients, delivery method
- MIS-033: Provide a visual A2 department integration map as a live system dashboard: every department displayed with its defined inputs and outputs, connected to adjacent departments. Each node shows current status (on-track, delayed, blocked). Handoff delays are auto-flagged when a department's output is overdue against the next department's expected input timeline. This is the system-level representation of the A2 layout used in organizational process design.

#### Data Entities

- Dashboard Configuration, Report Catalog, Report Schedule
- Data Refresh Log, Data Quality Metrics
- Report Distribution Lists, Subscription Records

#### Integration Points

- All ERP modules: as data sources for dashboards and reports
- Darwinbox: HR data feed for workforce dashboards
- E-commerce platforms: marketplace data for e-commerce dashboards
- Azure AI: anomaly detection alerts feeding into dashboard notifications

#### User Roles

| Role | Access |
|------|--------|
| MD/Director | Executive scorecards, all summary dashboards, drill-down to any module |
| COO | Operational scorecards, all detailed operational dashboards |
| Department Head | Full access to departmental dashboards, summary access to cross-functional |
| Analyst | Report builder access, detailed data exploration within designated modules |
| Report Administrator | Dashboard configuration, report scheduling, distribution management |

#### Dashboards and Reports

All dashboards and reports specified in requirements MIS-007 through MIS-032 above. The complete report catalog is detailed in Appendix Section 9.

---

### 3.14 CRM & Customer Management (CRM)

**Purpose:** Manage customer relationships across all channels, with particular emphasis on the salon B2B channel where personal relationship management by sales representatives is critical. Track customer interactions, manage complaints and service requests, and provide customer 360-degree views for informed selling.

#### Functional Requirements

- CRM-001: Maintain customer 360-degree view: customer profile, purchase history (last 12 months with trend), payment history, outstanding balance, complaints/returns history, last interaction date and type, assigned sales representative, salon tier/classification
- CRM-002: Track customer interactions: sales visits, phone calls, emails, B2B portal activity, complaint submissions, training sessions attended, event participation
- CRM-003: Manage complaint workflow: complaint registration (source: phone, email, portal, field visit) > categorization (product quality, delivery delay, damaged goods, wrong product, pricing dispute, service issue) > investigation > resolution > customer communication > closure > satisfaction feedback
- CRM-004: Track complaint resolution SLAs: acknowledgment within 4 hours, initial response within 24 hours, resolution within 7 working days for standard complaints, 48 hours for critical quality complaints
- CRM-005: Link product quality complaints to batch numbers in the quality module for batch-level complaint trending
- CRM-006: Generate customer churn risk alerts based on: declining purchase frequency, declining order values, increasing complaints, delayed payments
- CRM-007: Track new customer acquisition pipeline: lead source, conversion stage, estimated value, probability, assigned representative
- CRM-008: Support marketing campaign tracking: campaign definition, target customer list, execution tracking, response tracking, campaign ROI measurement
- CRM-009: Maintain competitor activity log per territory: competitor products spotted, competitor pricing, competitive wins/losses
- CRM-010: Generate customer satisfaction scores through periodic surveys (quarterly for top-tier salons, annually for all active customers)

#### Data Entities

- Customer 360 View (composite from CRM + ERP data)
- Interaction Records, Visit Records, Call Records
- Complaint Master, Complaint Investigation Records, Complaint Resolution Records
- Lead Master, Lead Pipeline Records
- Campaign Master, Campaign Responses
- Competitor Activity Log
- Customer Satisfaction Survey Records

#### Integration Points

- Sales module: customer master, order history, invoice history
- Finance module: payment history, outstanding balance, credit status
- Quality module: complaint to batch linkage, batch quality data
- B2B module: salon profile, visit records, portal activity
- Mobile App: field sales interaction logging
- Email/SMS: customer communication automation

#### User Roles

| Role | Access |
|------|--------|
| CRM Manager | Full access, complaint escalation management, campaign management |
| Sales Representative | Customer view for own accounts, interaction logging, complaint registration |
| Customer Service Executive | Complaint handling, resolution tracking, customer communication |
| CRM Analyst | Reporting, customer analytics, churn analysis, campaign effectiveness |

#### Dashboards and Reports

- Customer 360 dashboard: complete customer profile accessible by sales team
- Complaint tracker: open complaints by category, aging, SLA compliance
- Customer acquisition pipeline: leads by stage, conversion rate, expected value
- Customer retention dashboard: active, dormant, inactive, churned trends
- Campaign effectiveness: response rate, conversion rate, ROI by campaign
- Territory customer health: customer count, revenue, growth, complaints by territory

---

## 4. AI/ML Layer Specifications

### 4.1 Purpose

Layer AI/ML capabilities on top of the clean, structured data generated by the ERP to automate analysis, generate recommendations, and enable predictive decision-making. This layer is deployed after the ERP data foundation is stable (Phase 3 of implementation), building on the principle that AI on dirty data produces bad outcomes.

### 4.2 AI/ML Use Cases

**AI-MIS-001: MIS Automation & Anomaly Detection**

- Automatically generate daily MIS commentary: instead of raw numbers, provide natural language summaries highlighting what is notable ("Production was 12% below plan today, driven by a 3-hour downtime on Filling Line 2 due to mechanical fault")
- Detect anomalies in daily data: unusual spikes or drops in sales, inventory discrepancies, production yield deviations, cash flow irregularities
- Generate weekly trend alerts: identify patterns that are changing (e.g., "DermaCult Vitamin C Serum sales have declined 15% over the last 4 weeks across Salon B2B, investigate")
- Priority: High. Timeline: Month 7-9

**AI-MIS-002: Demand Forecasting Enhancement**

- Build ML-based demand forecasting models using: historical sales data, promotional calendars, seasonal patterns, market trends, weather data (relevant for sunscreen/seasonal products), economic indicators
- Improve forecast accuracy beyond statistical methods, targeting MAPE below 25% for A-class SKUs
- Automatically identify forecast bias and adjust
- Priority: High. Timeline: Month 9-12

**AI-MIS-003: Skin Prescription & Recommendation Engine**

- Build a consumer-facing skin analysis and product recommendation engine that takes: skin type (dry, oily, combination, sensitive), primary concern (acne, aging, pigmentation, dullness, dehydration), secondary concerns, climate/geography, age group, current routine, ingredient sensitivities/allergies
- Map these inputs against the O3+ product catalog to recommend: specific products, usage sequence (morning/evening routine), complementary products (building the product family basket)
- Deploy as: API service consumed by Shopify D2C store, B2B portal (for salon professionals to recommend to clients), mobile app
- Train on: dermatological guidelines, ingredient efficacy data, product formulation data, customer feedback/reviews
- Priority: Medium. Timeline: Month 10-14

**AI-MIS-004: Performance Management AI Scanner**

- Integrate with the task sheet system (HR module): scan task completion rates, quality of deliverables (where measurable), timeliness patterns
- Identify performance patterns: consistently high performers, declining performance trends, skill gaps correlating with specific task types
- Generate automated performance alerts to department heads: "Employee X has missed 3 out of 5 weekly task deadlines in the last month, down from 90% compliance previously"
- Input data: task sheet records, KPI actuals from ERP, attendance data, training completion data
- Priority: Medium. Timeline: Month 8-11

**AI-MIS-005: Pricing Optimization**

- Analyze SKU-level elasticity: how do sales volume and margin respond to price changes, discounts, and schemes
- Recommend optimal pricing per channel per SKU considering: competitor pricing, customer willingness to pay, margin targets, volume targets
- Model promotional effectiveness: predict the incremental revenue impact of proposed schemes before launch
- Priority: Low (Phase 3). Timeline: Month 12-18

**AI-MIS-006: Quality Prediction**

- Analyze historical batch quality data to predict: which incoming raw materials are likely to fail inspection (based on vendor, season, lot characteristics), which production conditions correlate with quality deviations
- Recommend preventive actions: "Vitamin C batches from Vendor X have shown increasing instability in the last 3 deliveries; recommend enhanced testing or alternate vendor"
- Priority: Low (Phase 3). Timeline: Month 14-18

### 4.3 Technical Architecture for AI/ML

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Data Lake | Azure Data Lake Storage Gen2 | Centralized storage for all structured and semi-structured data from ERP and external sources |
| Data Pipeline | Azure Data Factory | ETL/ELT pipelines from ERP, Darwinbox, e-commerce platforms to Data Lake |
| ML Platform | Azure Machine Learning | Model training, deployment, and monitoring |
| Cognitive Services | Azure OpenAI Service (GPT-4) | Natural language MIS commentary, anomaly explanation, recommendation text generation |
| API Layer | Azure Functions + API Management | Serving ML model predictions to consuming applications (Shopify, B2B portal, mobile app) |
| Monitoring | Azure ML Model Monitoring | Track model performance, detect drift, trigger retraining |

### 4.4 Data Requirements for AI/ML

- Minimum 12 months of clean ERP data before demand forecasting models can be reliably trained
- Minimum 6 months of task sheet data before performance pattern detection is meaningful
- Skin recommendation engine requires: complete product ingredient database, dermatological ingredient-skin type mapping (partially externally sourced), customer skin profiling data (collected through D2C store quizzes and salon consultations)
- All AI models must be validated with business stakeholders before deployment, not just data scientists

---

## 5. Integration Specifications

### 5.1 Integration Summary

| Integration | Source System | Target System | Direction | Protocol | Frequency | Priority |
|------------|--------------|---------------|-----------|----------|-----------|----------|
| INT-001: Shopify Orders | Shopify | D365 BC | Shopify to ERP | REST API + Webhooks | Real-time (webhook on order create) | Phase 1 |
| INT-002: Shopify Inventory | D365 BC | Shopify | ERP to Shopify | REST API | Every 5 minutes | Phase 1 |
| INT-003: Shopify Fulfillment | D365 BC | Shopify | ERP to Shopify | REST API | On shipment posting | Phase 1 |
| INT-004: Amazon Orders | Amazon SP-API | D365 BC | Amazon to ERP | REST API (polling) | Every 15 minutes | Phase 1 |
| INT-005: Amazon Inventory | D365 BC | Amazon SP-API | ERP to Amazon | REST API | Every 15 minutes | Phase 1 |
| INT-006: Amazon Fulfillment | D365 BC | Amazon SP-API | ERP to Amazon | REST API | On shipment posting | Phase 1 |
| INT-007: Amazon Settlements | Amazon SP-API | D365 BC | Amazon to ERP | REST API (daily pull) | Daily | Phase 1 |
| INT-008: Nykaa Orders | Nykaa API | D365 BC | Nykaa to ERP | API/EDI | Every 30 minutes | Phase 2 |
| INT-009: Nykaa Inventory | D365 BC | Nykaa API | ERP to Nykaa | API/EDI | Every 30 minutes | Phase 2 |
| INT-010: Blinkit Orders | Blinkit API | D365 BC | Blinkit to ERP | API | On order create | Phase 2 |
| INT-011: Zepto Orders | Zepto API | D365 BC | Zepto to ERP | API | On order create | Phase 2 |
| INT-012: GST E-Invoice | D365 BC | NIC Portal | ERP to NIC | REST API | On invoice posting | Phase 1 |
| INT-013: GST E-Way Bill | D365 BC | NIC Portal | ERP to NIC | REST API | On shipment posting | Phase 1 |
| INT-014: Bank Statement Import | Bank Portal | D365 BC | Bank to ERP | File-based (MT940/CSV) | Daily | Phase 1 |
| INT-015: Bank Payment File | D365 BC | Bank Portal | ERP to Bank | File-based (bank format) | On payment batch | Phase 1 |
| INT-016: Darwinbox HR Data | Darwinbox | D365 BC | Darwinbox to ERP | REST API | Daily | Phase 2 |
| INT-017: Darwinbox Payroll Cost | Darwinbox | D365 BC | Darwinbox to ERP | REST API | Monthly | Phase 2 |
| INT-018: Power BI Data Feed | D365 BC | Power BI | ERP to BI | DirectQuery + Import | 15-min / Daily | Phase 1 |
| INT-019: B2B Portal | B2B Portal | D365 BC | Bidirectional | REST API | Real-time | Phase 2 |
| INT-020: Mobile Sales App | Mobile App | D365 BC | Bidirectional | REST API | Real-time | Phase 2 |
| INT-021: Azure AI Data Feed | D365 BC | Azure Data Lake | ERP to AI | Azure Data Factory | Daily batch + CDC | Phase 3 |
| INT-022: AI Recommendations | Azure ML | Shopify / B2B Portal | AI to Apps | REST API | On-demand | Phase 3 |
| INT-023: Payment Gateway (Razorpay/Cashfree) | Payment Gateway | D365 BC | Gateway to ERP | Webhooks + API | Real-time | Phase 1 |

### 5.2 Integration Design Principles

- All integrations route through Azure API Management for: centralized logging, rate limiting, retry logic, circuit breaker, and monitoring
- Webhook-based integrations (Shopify, payment gateways) have a queue buffer (Azure Service Bus) to handle burst loads and ensure no message loss
- File-based integrations (bank) have file validation and error handling before import
- All integrations have configurable retry: 3 retries with exponential backoff (30s, 60s, 120s), then dead-letter queue for manual review
- Integration monitoring dashboard shows: sync status per integration, last successful sync, error count, messages in dead-letter queue

### 5.3 Error Handling

- INT-ERR-001: Every integration has an error handling workflow: on error > log error with full payload > retry per policy > if all retries fail > push to dead-letter queue > notify integration team via email and Teams alert
- INT-ERR-002: Dead-letter queue items must be reviewed and resolved within 4 hours during business hours
- INT-ERR-003: Integration error dashboard visible to IT team and operations leadership showing: total errors today, errors by integration, unresolved dead-letter items

---

## 6. Data Migration Plan

### 6.1 Migration Scope

Migrate from Microsoft Dynamics NAV (Navision) to Dynamics 365 Business Central. Given the low utilization of Navision, a significant portion of operational data currently resides in Excel spreadsheets, which must also be migrated.

### 6.2 Migration Strategy

**Approach:** Phased migration with parallel run period.

| Phase | Data Category | Source | Approach |
|-------|--------------|--------|----------|
| Phase 1A: Master Data | Item Master (all 237+ SKUs with attributes, shelf life, HSN, UoM) | Navision + Excel | Clean, validate, and load |
| Phase 1A: Master Data | Customer Master (50,000+ salons + distributors + MT chains + export buyers) | Navision + Excel | Clean, de-duplicate, validate GSTIN, and load |
| Phase 1A: Master Data | Vendor Master (all raw material, packaging, service vendors) | Navision + Excel | Clean, validate GSTIN/PAN, and load |
| Phase 1A: Master Data | Chart of Accounts (restructured for new dimensional analysis) | Navision | Map old CoA to new CoA, validate balances |
| Phase 1A: Master Data | BOM/Formulation Master | R&D records (manual/Excel) | Enter fresh in new system with R&D validation |
| Phase 1B: Opening Balances | GL Opening Balances | Navision | Extract trial balance as of cutover date, post as opening entry |
| Phase 1B: Opening Balances | Customer Opening Balances (outstanding invoices) | Navision | Extract open customer ledger entries, load as opening balances |
| Phase 1B: Opening Balances | Vendor Opening Balances (outstanding invoices) | Navision | Extract open vendor ledger entries, load as opening balances |
| Phase 1B: Opening Balances | Inventory Opening Balances (item-wise, batch-wise, location-wise) | Navision + physical count | Conduct physical inventory count at cutover, load actual counted quantities with batch details |
| Phase 1B: Opening Balances | Fixed Asset Register | Navision + Excel | Extract asset master with accumulated depreciation, validate, load |
| Phase 2: Transaction History | Sales history (12-24 months) | Navision | Extract for reporting/analysis purposes in Power BI data warehouse, not in BC transaction tables |
| Phase 2: Transaction History | Purchase history (12-24 months) | Navision | Same approach: to data warehouse for analysis |
| Phase 2: Transaction History | Production history | Manual records/Excel | Digitize key records for reference, not full migration |
| Phase 3: Documents | SOPs, CoAs, MSDS, regulatory certificates | File servers, email, physical files | Organize and upload to SharePoint with metadata tagging |

### 6.3 Data Cleansing Rules

- MIG-001: All master data records must have mandatory fields populated before migration: items (name, category, HSN, UoM, GST rate), customers (legal name, GSTIN, address, category, territory), vendors (legal name, GSTIN/PAN, category)
- MIG-002: De-duplicate customer master: identify potential duplicates by name similarity, GSTIN match, address match. Merge duplicates with consolidated transaction history
- MIG-003: De-duplicate vendor master: same approach
- MIG-004: Validate GSTIN format and status (active/suspended) for all customers and vendors via GST portal API lookup
- MIG-005: Standardize item names, descriptions, and categories across all SKUs
- MIG-006: Validate BOM accuracy: R&D must review and sign off on every migrated formulation before it goes live
- MIG-007: Reconcile opening inventory balances from physical count against Navision book stock, with variance investigation and write-off approval for discrepancies

### 6.4 Migration Timeline

| Activity | Duration | Dependencies |
|----------|----------|-------------|
| Data extraction from Navision | 2 weeks | Navision admin access |
| Data profiling and quality assessment | 1 week | Extraction complete |
| Data cleansing and transformation | 3 weeks | Quality assessment complete, cleansing rules defined |
| Master data load (trial migration 1) | 1 week | Cleansing complete, BC environment ready |
| Validation and correction | 2 weeks | Trial migration 1 complete, business user validation |
| Master data load (trial migration 2) | 3 days | Corrections applied |
| Final validation | 1 week | Trial migration 2 complete |
| Physical inventory count | 3 days | Planned for cutover weekend |
| Opening balance load | 2 days | Physical count complete, GL balances reconciled |
| Cutover and go-live | 1 day (weekend) | All validations complete, management sign-off |
| Post-go-live data validation | 2 weeks | Go-live complete |
| Historical data load to data warehouse | 2 weeks (parallel with post-go-live) | Go-live complete |

### 6.5 Cutover Strategy

- Cutover happens over a weekend to minimize business disruption
- Navision is frozen for data entry 48 hours before cutover
- Final delta extraction captures any transactions between last full extraction and freeze
- Opening balances reflect the position as of cutover date
- Physical inventory count conducted on Friday evening and Saturday
- All opening balances loaded and reconciled on Saturday
- Go-live on Monday morning
- Parallel run for 30 days: key transactions verified against expected outcomes
- Navision remains accessible (read-only) for 90 days post-go-live for reference

---

## 7. Non-Functional Requirements

### 7.1 Performance

- NFR-001: Page load time for any BC screen: less than 3 seconds on standard broadband (10 Mbps)
- NFR-002: Report generation for standard reports (up to 10,000 rows): less than 10 seconds
- NFR-003: Report generation for large analytical reports (100,000+ rows): less than 60 seconds
- NFR-004: E-commerce order import processing: less than 5 seconds per order
- NFR-005: Inventory availability check response: less than 2 seconds
- NFR-006: Dashboard refresh (Power BI): incremental refresh completes within 5 minutes for critical dashboards
- NFR-007: System must handle concurrent users: 50 concurrent ERP users, 200 concurrent B2B portal users, 500 concurrent D2C website users (order processing)
- NFR-008: Batch processing (MRP, MIS report generation, data sync): must complete within 2-hour overnight window (12 AM to 2 AM IST)
- NFR-009: Barcode scan response time in warehouse: less than 1 second for transaction validation and confirmation

### 7.2 Availability

- NFR-010: System availability: 99.5% uptime during business hours (8 AM to 10 PM IST, Monday to Saturday), measured monthly
- NFR-011: Planned maintenance window: Sunday 2 AM to 6 AM IST (automatic BC SaaS updates managed by Microsoft occur within this window)
- NFR-012: Maximum unplanned downtime: 4 hours per incident, with recovery target (RTO) of 1 hour for critical functions (order processing, production recording, dispatch)
- NFR-013: Data backup: continuous backup with point-in-time recovery capability for last 30 days (Azure-managed for BC SaaS)

### 7.3 Scalability

- NFR-014: System must scale to support projected business growth: Rs 780 crore revenue (FY29), estimated 500+ SKUs, 100,000+ salon customers, 100+ concurrent ERP users
- NFR-015: E-commerce integration must handle peak loads during sale events: 10x normal order volume for Shopify, 5x for Amazon (Big Billion Days, Prime Day equivalents)
- NFR-016: Data storage must accommodate 5 years of transactional data within the ERP and 7 years in the data warehouse for compliance

### 7.4 Security

- NFR-017: Authentication: Azure Active Directory single sign-on for all Microsoft stack applications (BC, Power BI, SharePoint). Multi-factor authentication (MFA) mandatory for all users
- NFR-018: Authorization: role-based access control (RBAC) with principle of least privilege. Users see only data relevant to their function, department, and geography
- NFR-019: Data encryption: at rest (Azure-managed encryption for BC SaaS, AES-256) and in transit (TLS 1.2 minimum for all API communications)
- NFR-020: API security: OAuth 2.0 for all external API integrations, API keys rotated quarterly, IP whitelisting for critical integrations (GST portal, bank APIs)
- NFR-021: Audit logging: all data creation, modification, and deletion logged with: timestamp, user, field changed, old value, new value. Audit logs retained for 7 years
- NFR-022: Sensitive data handling: PAN numbers, bank account details, salary data encrypted with field-level encryption beyond database-level encryption
- NFR-023: Data access segregation: production data cannot be accessed from development/sandbox environments. Test environments use masked/anonymized data
- NFR-024: Vulnerability management: quarterly security assessments, annual penetration testing, timely patching (BC SaaS patches managed by Microsoft)
- NFR-025: Data residency: all data stored within India (Azure India regions) to comply with potential data localization requirements
- NFR-026: Session management: automatic session timeout after 30 minutes of inactivity, concurrent session limit of 2 per user

### 7.5 Compliance

- NFR-027: The system must maintain audit trails sufficient for: statutory audit (Companies Act 2013), tax audit (Income Tax Act), GST audit, internal audit, regulatory audit (CDSCO, BIS, FDA)
- NFR-028: Electronic records must comply with Information Technology Act 2000 requirements for electronic records and digital signatures where applicable
- NFR-029: Data retention per Indian regulatory requirements: financial records 8 years, batch records 3 years beyond product expiry, quality records 5 years, employee records 5 years beyond separation

### 7.6 Disaster Recovery

- NFR-030: Recovery Point Objective (RPO): maximum 5 minutes of data loss (achieved through continuous Azure backup)
- NFR-031: Recovery Time Objective (RTO): 1 hour for critical functions (finance, inventory, production, order processing), 4 hours for secondary functions (reporting, analytics, document management)
- NFR-032: DR testing: annual DR drill simulating primary region failure and failover to secondary Azure region
- NFR-033: Business continuity plan: documented procedures for operating with degraded capability if ERP is unavailable (manual dispatch process, offline order capture, etc.)

---

## 8. Implementation Phasing

### 8.1 Phase Overview

The implementation is structured in three major phases aligned with the 90-day transformation plan and subsequent quarters. The approach prioritizes getting the "single command system" operational first (inventory, production, dispatch visibility), then expanding to advanced commercial and planning functions, and finally deploying AI/ML capabilities.

### 8.2 Phase 1: Foundation (Days 31-120 / Month 2-4)

**Scope:** Core ERP modules to achieve basic operational visibility and control

| Workstream | Modules/Functions | Milestone |
|------------|------------------|-----------|
| ERP Platform Setup | D365 Business Central SaaS provisioning, environment setup, user licensing | Month 2, Week 1 |
| Implementation Partner Onboarding | Partner selection (Indian FMCG experience required), SOW finalization, team mobilization | Month 2, Week 2 |
| Finance (Core) | Chart of Accounts, GL, AR, AP, bank management, basic GST (e-invoicing, e-way bill) | Month 4 go-live |
| Inventory (Core) | Item master, batch tracking, warehouse zones/bins, goods receipt, goods issue, FIFO/FEFO enforcement, inventory aging tracking | Month 4 go-live |
| Procurement (Core) | Vendor master, purchase orders, goods receipt with QC trigger, three-way matching | Month 4 go-live |
| Manufacturing (Core) | BOM/formulation, production orders, material consumption, output recording, batch costing | Month 4 go-live |
| Quality (Core) | Incoming inspection, finished goods inspection, batch release, CoA management | Month 4 go-live |
| Sales (Core) | Customer master, sales orders, pricing (basic: channel-wise), dispatch, invoicing, basic returns | Month 4 go-live |
| GST Compliance | Full e-invoicing, e-way bill, GSTR-1/3B preparation, ITC reconciliation | Month 4 go-live |
| Data Migration | Master data cleansing and load, opening balance migration, physical inventory count | Month 3-4 |
| Power BI (Core) | Daily production report, daily dispatch report, daily inventory position, daily sales report, daily cash position | Month 4 go-live |
| Shopify Integration | Order import, inventory sync, fulfillment update, payment reconciliation | Month 4 go-live |
| Amazon Integration | Order import (FBM), inventory sync, fulfillment update, settlement reconciliation | Month 4 go-live |
| Training | Core user training for finance, warehouse, procurement, production, QC, sales teams | Month 3-4 |

**Go-Live Criteria for Phase 1:**
- All master data migrated and validated by business owners
- Opening balances reconciled between Navision and BC
- Physical inventory count completed and loaded
- Core transactions tested end-to-end: procure-to-pay, produce-to-stock, order-to-cash
- E-invoicing and e-way bill tested against NIC sandbox
- Minimum 20 power users trained and certified
- Daily MIS reports producing accurate data
- Rollback plan documented and tested

### 8.3 Phase 2: Expansion (Month 5-8)

**Scope:** Advanced commercial functions, all channel integrations, planning module, HRMS integration

| Workstream | Modules/Functions | Milestone |
|------------|------------------|-----------|
| Sales Advanced | Tiered pricing, scheme management, credit management, salon B2B portal, territory management, commission calculation | Month 6 |
| E-Commerce (Full) | Nykaa integration, Blinkit/Zepto integration, FBA inventory management, full marketplace reconciliation, channel-wise inventory allocation | Month 7 |
| B2B Portal | Salon self-service portal: catalog, order placement, order tracking, statements, scheme visibility | Month 7 |
| Mobile App | Sales rep mobile app: order entry, visit logging, GPS check-in, collection recording | Month 7 |
| Supply Chain Planning | Demand forecasting, MPS, MRP, S&OP process support | Month 8 |
| Quality Advanced | Stability testing, deviation/CAPA, change control, GMP compliance tracking (training, calibration, SOPs) | Month 7 |
| PLM | Product development workflow, SKU rationalization reporting, packaging management | Month 8 |
| Regulatory | Registration tracking, ingredient database, export compliance, audit management | Month 7 |
| Cost Accounting Advanced | Full absorption batch costing, channel-wise landed cost, variance analysis, SKU profitability | Month 6 |
| CRM | Customer 360, complaint management, campaign tracking | Month 7 |
| Darwinbox Integration | Employee cost feed to ERP, labor cost allocation to batches | Month 6 |
| HR/Performance Integration | Task sheet system integrated with ERP KPI data feeds | Month 8 |
| Power BI (Full) | Weekly reports, monthly reports, all dashboards as specified in MIS module, executive scorecards | Month 8 |

### 8.4 Phase 3: Intelligence (Month 9-18)

**Scope:** AI/ML layer, advanced analytics, process optimization

| Workstream | Modules/Functions | Milestone |
|------------|------------------|-----------|
| Data Lake Setup | Azure Data Lake provisioning, data pipeline from ERP/Darwinbox/e-commerce, data quality framework | Month 9 |
| AI: MIS Automation | Anomaly detection, natural language MIS commentary, automated trend alerts | Month 10 |
| AI: Performance Scanner | Task sheet compliance scanning, performance pattern detection, automated alerts | Month 11 |
| AI: Demand Forecasting | ML-based demand forecasting models, accuracy benchmarking against statistical methods | Month 12 |
| AI: Skin Recommendation | Consumer skin profiling, product matching engine, deployment on Shopify and B2B portal | Month 14 |
| AI: Pricing Optimization | Price elasticity analysis, scheme effectiveness prediction, channel pricing recommendation | Month 16 |
| AI: Quality Prediction | Incoming quality prediction, process deviation prediction | Month 18 |
| Advanced Analytics | Self-service analytics for business users, advanced dashboard customization | Month 12 |
| Process Optimization | Based on 6+ months of system data: identify and implement process improvements in procurement, production, logistics | Ongoing from Month 10 |

### 8.5 Resource Requirements for Implementation

| Role | Phase 1 (4 months) | Phase 2 (4 months) | Phase 3 (10 months) |
|------|-------|-------|-------|
| Project Manager (O3+ internal) | 1 full-time | 1 full-time | 1 part-time |
| Implementation Partner Team (external) | 6-8 consultants | 4-6 consultants | 2-3 consultants |
| BC Functional Consultants (partner) | 4 | 3 | 1 |
| BC Technical/AL Developer (partner) | 2 | 2 | 1 |
| Integration Developer (partner or internal) | 1 | 2 | 1 |
| Power BI Developer (partner or internal) | 1 | 1 | 1 |
| AI/ML Engineer (internal or contracted) | 0 | 0 | 2 |
| Data Migration Specialist (partner) | 1 | 0 | 0 |
| O3+ Business Process Owners (internal, part-time) | 6-8 (one per module area) | 6-8 | 3-4 |
| O3+ Super Users/Testers (internal, part-time during UAT) | 15-20 | 15-20 | 5-10 |
| Change Management/Training Lead (internal or partner) | 1 | 1 | 0.5 |

### 8.6 Budget Estimate (Indicative)

| Component | Year 1 (Phase 1 + Phase 2 start) | Year 2 (Phase 2 + Phase 3) | Annual Recurring |
|-----------|--------|--------|-----------------|
| D365 Business Central Licenses (50 users, various license types) | Rs 60-75 lakh | Rs 60-75 lakh | Rs 60-75 lakh |
| ISV Extensions (Process Manufacturing, WMS, Quality) | Rs 15-25 lakh | Rs 10-15 lakh | Rs 10-15 lakh |
| Implementation Partner Services | Rs 1.0-1.5 crore | Rs 60-80 lakh | Rs 20-30 lakh (support) |
| Azure Infrastructure (API Management, Data Lake, AI services) | Rs 15-20 lakh | Rs 25-35 lakh | Rs 30-40 lakh |
| Power BI Premium Licenses | Rs 8-12 lakh | Rs 8-12 lakh | Rs 8-12 lakh |
| Integration Development (e-commerce, GST, bank) | Rs 20-30 lakh | Rs 15-25 lakh | Rs 5-10 lakh (maintenance) |
| B2B Portal Development | Included in Phase 2 | Rs 15-25 lakh | Rs 5-8 lakh |
| Mobile App Development | Included in Phase 2 | Rs 10-15 lakh | Rs 3-5 lakh |
| AI/ML Development | 0 | Rs 30-50 lakh | Rs 15-25 lakh |
| Training and Change Management | Rs 10-15 lakh | Rs 8-12 lakh | Rs 5-8 lakh |
| Contingency (15%) | Rs 22-30 lakh | Rs 20-30 lakh | -- |
| **Total Estimated** | **Rs 1.7-2.3 crore** | **Rs 1.9-2.7 crore** | **Rs 1.6-2.3 crore** |

Note: These are indicative estimates. Final costs depend on implementation partner selection, license negotiation, customization complexity, and scope refinement during discovery.

---

## 9. Appendix: MIS Report Catalog

### 9.1 Daily Reports

| Report ID | Report Name | Description | Data Source | Primary Recipients | Delivery |
|-----------|-------------|-------------|-------------|-------------------|----------|
| RPT-D-001 | Daily Production Summary | Products produced, batch numbers, quantities, yield, variance from plan, MTD cumulative | Manufacturing module | COO, Production Head, Planning Head | Auto-email 7 AM + Dashboard |
| RPT-D-002 | Daily Dispatch Summary | Orders dispatched, quantities, destinations, MTD cumulative, pending backlog, delayed orders | Sales/Inventory module | COO, Sales Head, Logistics Head | Auto-email 7 AM + Dashboard |
| RPT-D-003 | Daily Sales Summary | Orders received by channel, value, top products, comparison with same period prior year | Sales module | MD, COO, Sales Head, E-commerce Head | Auto-email 8 AM + Dashboard |
| RPT-D-004 | Daily Inventory Position | FG stock by SKU (units and value), stock days, items below safety stock, items in quarantine | Inventory module | COO, Planning Head, Sales Head | Dashboard (real-time) |
| RPT-D-005 | Daily Cash Position | Bank balances, receipts, payments, closing balance, 7-day projection | Finance module | MD, COO, Finance Head | Auto-email 10 AM + Dashboard |
| RPT-D-006 | Daily E-Commerce Orders | Orders by marketplace, GMV, returns, settlements, inventory sync status | E-commerce module | E-commerce Head, Finance Head | Auto-email 9 AM + Dashboard |
| RPT-D-007 | Daily Production Quality | Batches tested, pass/fail, batches awaiting QC, in-process rejections | Quality module | Quality Head, Production Head | Auto-email 7 AM |
| RPT-D-008 | Daily Goods Receipt | Materials received today, vendor, quantity, quality status | Procurement/Inventory module | Procurement Head, Warehouse Manager | Auto-email 7 AM |
| RPT-D-009 | Daily Customer Collections | Collections received by territory, overdue collections pending | Finance module | Finance Head, Sales Head | Auto-email 10 AM |
| RPT-D-010 | Daily Picking/Packing Status | Orders in picking queue, picked, packed, awaiting dispatch, dispatch exceptions | WMS module | Warehouse Manager, Sales Coordinator | Dashboard (real-time) |

### 9.2 Weekly Reports

| Report ID | Report Name | Description | Data Source | Primary Recipients | Delivery |
|-----------|-------------|-------------|-------------|-------------------|----------|
| RPT-W-001 | Production Plan vs Actual | SKU-wise planned vs actual production, variance analysis, reason for shortfall | Manufacturing module | COO, Production Head | Auto-email Monday 8 AM |
| RPT-W-002 | Procurement Status | Pending POs, expected deliveries this week, overdue deliveries, material shortages for next 2 weeks | Procurement module | COO, Procurement Head, Planning Head | Auto-email Monday 8 AM |
| RPT-W-003 | Quality Weekly Summary | Batches tested, rejection rate, open OOS, open deviations, CAPA status, incoming acceptance rate | Quality module | COO, Quality Head | Auto-email Monday 8 AM |
| RPT-W-004 | Order Fulfillment OTIF | On-time in-full rate by channel, backorder analysis, reasons for non-fulfillment | Sales/Inventory module | COO, Sales Head, Planning Head | Auto-email Monday 8 AM |
| RPT-W-005 | AR Aging Summary | Total receivables, aging distribution, overdue amount, top 20 overdue, collection performance by territory | Finance module | Finance Head, Sales Head | Auto-email Monday 8 AM |
| RPT-W-006 | Inventory Aging | Aging band distribution (0-6m, 6-12m, 12-24m, 24m+) for RM, PM, FG with week-over-week change | Inventory module | COO, Planning Head, Sales Head | Auto-email Monday 8 AM |
| RPT-W-007 | Sales Rep Performance | Revenue vs target, visits vs plan, collection vs target, new salons, by representative | Sales/B2B module | Sales Head, Regional Managers | Auto-email Monday 8 AM |
| RPT-W-008 | E-Commerce Performance | Weekly GMV by marketplace, order count, return rate, average order value, marketplace payment status | E-commerce module | E-commerce Head, Finance Head | Auto-email Monday 8 AM |
| RPT-W-009 | Dispatch Logistics | Shipments in transit, delivery SLA compliance by transporter, freight cost per shipment | Sales/Logistics module | Logistics Head, Finance Head | Auto-email Monday 8 AM |
| RPT-W-010 | FIFO/FEFO Compliance | Picks out of FEFO/FIFO sequence, override reasons, compliance percentage by zone | WMS module | Warehouse Manager, COO | Auto-email Monday 8 AM |

### 9.3 Monthly Reports

| Report ID | Report Name | Description | Data Source | Primary Recipients | Delivery |
|-----------|-------------|-------------|-------------|-------------------|----------|
| RPT-M-001 | Channel P&L | Revenue, COGS, gross margin, channel costs, contribution margin, operating profit per channel | Finance/Sales module | MD, COO, Finance Head, Sales Head | 5th working day + Dashboard |
| RPT-M-002 | Brand P&L | Revenue, COGS, gross margin, brand costs, contribution margin per brand | Finance/Sales module | MD, COO, Brand Heads | 5th working day + Dashboard |
| RPT-M-003 | Category P&L | Revenue, COGS, gross margin by product category | Finance/Sales module | MD, COO, Brand Heads | 5th working day |
| RPT-M-004 | SKU Profitability | Revenue, units, manufacturing cost, landed cost, margin per SKU per channel | Finance/Manufacturing/Sales | COO, Finance Head, Brand Heads | 5th working day |
| RPT-M-005 | Inventory Turnover | Inventory turns by item category, brand, product category, with target comparison | Inventory module | COO, Finance Head, Planning Head | 5th working day |
| RPT-M-006 | Procurement Analysis | Spend by vendor category, price variance, vendor delivery score, import status, spend vs budget | Procurement module | COO, Procurement Head, Finance Head | 5th working day |
| RPT-M-007 | Manufacturing Performance | Production volume vs plan, capacity utilization, OEE, yield trends, downtime analysis, batch cost variance | Manufacturing module | COO, Production Head | 5th working day |
| RPT-M-008 | Cash Flow Statement | Operating, investing, financing cash flows, net cash, 3-month forecast | Finance module | MD, COO, Finance Head | 5th working day |
| RPT-M-009 | GST Reconciliation | Output liability, ITC claimed, net GST, GSTR-2A/2B match rate, unmatched invoices | Finance module | Finance Head, GST Officer | 5th working day |
| RPT-M-010 | HR Dashboard | Headcount, hires, separations, attrition rate, training hours, performance distribution, open positions | Darwinbox/HR module | MD, COO, HR Head | 5th working day |
| RPT-M-011 | Budget vs Actual | Revenue, cost, margin: budget vs actual by department, brand, channel | Finance module | MD, COO, Finance Head, All Dept Heads | 5th working day |
| RPT-M-012 | Export Performance | Export revenue by country, order status, documentation status, regulatory compliance, realization | Sales/Export module | MD, COO, Export Head | 5th working day |
| RPT-M-013 | Customer Health | Active vs dormant vs churned customers, acquisition rate, churn rate, tier migration, complaint trend | CRM/Sales module | COO, Sales Head | 5th working day |
| RPT-M-014 | Vendor Scorecard | Quality, delivery, commercial, responsiveness scores per vendor, trend | Procurement/Quality module | Procurement Head, Quality Head | 5th working day |
| RPT-M-015 | Scheme Effectiveness | Per scheme: revenue impact, cost, ROI, utilization rate | Sales module | Sales Head, Finance Head | 5th working day |

### 9.4 Quarterly Reports

| Report ID | Report Name | Description | Data Source | Primary Recipients | Delivery |
|-----------|-------------|-------------|-------------|-------------------|----------|
| RPT-Q-001 | Business Performance Review | Comprehensive quarterly review: revenue, margin, growth, market position, operational KPIs, against 3-year plan | All modules | MD, Board | 10th working day of quarter |
| RPT-Q-002 | SKU Rationalization Review | Bottom-performing SKUs, cannibalization analysis, discontinuation recommendations | Sales/Finance/Inventory | MD, COO, Brand Heads | 10th working day of quarter |
| RPT-Q-003 | Supply Chain Scorecard | Forecast accuracy, production plan adherence, procurement performance, inventory health, logistics performance | SCM/Manufacturing/Procurement | COO, SCM Head | 10th working day of quarter |
| RPT-Q-004 | Regulatory Compliance Status | Registration status per product per market, upcoming renewals, audit findings status | Regulatory module | MD, Regulatory Head | 10th working day of quarter |
| RPT-Q-005 | Performance Management Summary | Organization performance distribution, department-wise KPI achievement, training effectiveness, hiring status | HR module/Darwinbox | MD, COO, HR Head | 10th working day of quarter |

### 9.5 Ad-Hoc / On-Demand Reports

| Report ID | Report Name | Description | Trigger |
|-----------|-------------|-------------|---------|
| RPT-A-001 | Batch Traceability Report | Forward/backward traceability for a specific batch: from raw materials through production to customer shipments | Batch number input |
| RPT-A-002 | Customer Statement | Account statement for a specific customer showing all invoices, payments, credits, and balance | Customer selection |
| RPT-A-003 | Vendor Statement | Account statement for a specific vendor | Vendor selection |
| RPT-A-004 | Batch Recall Impact Assessment | Given a batch number, identify all affected downstream batches and customers, with contact details and quantities | Batch recall initiation |
| RPT-A-005 | Product Costing Drill-Down | Detailed cost breakdown for a product: formulation cost, packaging cost, labor, overhead, by batch or standard | Product selection |
| RPT-A-006 | Inventory Valuation Report | Current inventory value by item, batch, location, with aging and standard/actual cost | On demand |
| RPT-A-007 | Price Comparison Report | Compare vendor prices for a specific item across all approved vendors | Item selection |
| RPT-A-008 | Channel Margin Waterfall | Detailed margin waterfall for a specific product in a specific channel: MRP to net realization | Product + channel selection |
| RPT-A-009 | Sales Representative Route Analysis | Actual visit locations vs planned beat, time at each stop, orders placed | Representative + date range |
| RPT-A-010 | Capacity Planning What-If | Model production capacity scenarios: additional shift, new line, outsourcing | Planning parameters input |

---

**Document Control**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 12 April 2026 | Vipul Udbhav | Initial version |

---

*This document is confidential and intended for internal use by O3+ (Visage Beauty & Health Care Pvt Ltd) and authorized implementation partners only. Distribution outside these parties requires written approval from the MD or COO.*
