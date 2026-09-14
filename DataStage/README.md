[← Back to all labs](../README.md)

# Batch Integration with watsonx.data integration DataStage

> **Lab Level:** Beginner &nbsp;|&nbsp; **Estimated Duration:** 1 hour &nbsp;|&nbsp; **Platform:** IBM watsonx.data Integration (DataStage)

---

## Welcome

Welcome to this hands-on lab. Over the next hour you will build reusable batch ETL/ELT pipelines using IBM DataStage within watsonx.data integration. You will ingest data from multiple external sources — including Db2 Warehouse, a PostgreSQL database, and a MongoDB database — transform and enrich it, and deliver the result to a single output file.

By the time you finish you will have experienced the full lifecycle of modern batch data integration: **multi-source ingestion → transformation → enrichment → parameterized output**.

---

## The Story

### Meet the Golden Bank Team

Golden Bank needs to adhere to a new regulation where it cannot lend to underqualified loan applicants. As a data engineer at Golden Bank, you use DataStage to aggregate anonymized mortgage application data with applicants' personally identifiable information.

Your lenders use this information to decide whether to approve or deny mortgage applications. Your leadership has added risk analysts who calculate daily what interest rate they recommend for borrowers in each credit score range. You need to integrate this information into a spreadsheet shared with the lenders — including credit score data, total debt, and an interest-rate lookup table — and load it into a target output CSV file.

---

## What You Will Build

```
Db2 Warehouse              PostgreSQL DB           MongoDB DB
(Applicant & Application   (Credit Score data)     (Interest Rate data)
 data)
        │                        │                        │
        ▼                        ▼                        ▼
   Db2 Warehouse          PostgreSQL               MongoDB
   Connector              Connector                Connector
        │                        │                        │
        └────────────────────────┘                        │
                     │                                    │
                     ▼                                    │
               Join Stage                                 │
          (applicant + credit score)                      │
                     │                                    │
                     ▼                                    │
            Transformer Stage                             │
          (calculate total debt)                          │
                     │                                    │
                     └────────────────────────────────────┘
                                  │
                                  ▼
                           Lookup Stage
                      (interest rate per applicant)
                                  │
                                  ▼
                      Sequential File (CSV output)
                      parameterized via Parameter Sets
```

### Key learning objectives

| # | Objective |
|---|-----------|
| 1 | Run and explore an existing DataStage batch flow in a watsonx.data integration project |
| 2 | Edit the flow by adding stages to join, transform, and enrich data from multiple sources |
| 3 | Create reusable parameter sets and value sets to make pipelines configurable at runtime |

---

## Lab Modules

| Module | Title | Who | Duration |
|--------|-------|-----|----------|
| **Module 1** | [Author Reusable Batch ETL Pipelines with DataStage](module1/README.md) | Participant | ~60 min |

> **Note for participants:** Your tutor will provide credentials and environment access details before the lab session begins.

---

## Prerequisites

### For participants
- An IBM ID with access to the lab watsonx.data integration environment
- Access to the IBM watsonx Data Fabric at `dataplatform.cloud.ibm.com`
- A web browser (Chrome or Firefox recommended)
- Lab project ZIP file (provided by the tutor or downloaded as instructed in Module 1)

---

## Use Case Overview

**Golden Bank** aggregates mortgage application data from three sources:

| Source | Type | Content |
|--------|------|---------|
| Db2 Warehouse database | Relational DB | Applicant & application records |
| PostgreSQL database | Relational DB | Credit score data per applicant |
| MongoDB database | NoSQL DB | Daily recommended interest rates per credit score range |

The DataStage flow joins and enriches these sources, calculates total debt, looks up applicable interest rates, and writes the result to a target CSV file for lenders.

---

## Lab Structure

```
DataStage/
├── README.md              ← You are here
└── module1/
    └── README.md         ← Participant guide: build the pipeline
```

---

## Ready to Start?

**Participants** → wait for your tutor to hand out the connection details and credentials, then open **[Module 1](module1/README.md)** and follow the steps in order.

---

<div align="center">
<sub>IBM watsonx.data Integration Lab &nbsp;·&nbsp; Batch Integration with DataStage &nbsp;·&nbsp; Golden Bank Use Case</sub>
<br/><br/>
<sub>Guide maintained by the IBM France Client Engineering team</sub>
</div>
