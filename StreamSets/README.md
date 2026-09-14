[← Back to all labs](../README.md)

# Real-Time FX Rate Analytics with IBM watsonx.data Integration StreamSets

> **Lab Level:** Intermediate &nbsp;|&nbsp; **Estimated Duration:** 3–4 hours &nbsp;|&nbsp; **Platform:** IBM watsonx.data Integration (StreamSets) + Confluent Kafka

---

## Welcome

Welcome to this hands-on lab. Over the next few hours you will build an end-to-end, real-time data pipeline that ingests simulated multi-currency exchange rates, filters them in-flight to isolate EUR/USD readings, persists them in a query-ready lakehouse table, and visualises the result in an IBM Data Integration project — all without writing a single line of application code.

By the time you finish you will have worked through every layer of the modern data stack that IBM watsonx.data Integration is designed to connect: **streaming ingest → pipeline transformation → lakehouse storage → governed analytics**.

---

## The Story

### Meet Isabelle

Isabelle Mercier is a senior treasury analyst at **Horizon Asset Management**, a mid-size French investment firm that manages cross-border equity portfolios on behalf of institutional clients.

The firm holds significant USD-denominated assets while reporting in EUR. Every day Isabelle's desk receives dozens of ad-hoc requests:   

- *"What was the EUR/USD rate around noon today?"*, 
- *"Can you pull the EUR/USD rate history for the last trading week?"* 

Her current answer is always the same: she exports a spreadsheet from the Reuters terminal, massages it in Excel, and emails a PNG chart. The whole cycle takes two hours — by which time the rate has moved and hedging decisions have already been delayed.

Isabelle's CIO has just signed a contract for IBM watsonx.data Integration and Confluent Cloud. The mandate: **make EUR/USD rate data available to treasury analysts within minutes of each fix, stored in an open lakehouse that the whole organisation can query.**

You are the data engineer assigned to build the first production pipeline.

---

## What You Will Build

```
Datagen Connector          Confluent Kafka          StreamSets Pipeline
(10 × 10 currencies) ───►   topic: fx_rates    ───►  Kafka Origin
  1 tick / 30 sec            Avro schema                  │
                                                          ▼
                                                     Expression Evaluator
                                                     (kafkaTimestamp → /timestamp)
                                                          │
                                                          ▼
                                                     Stream Selector
                                                     (from_currency=EUR
                                                      AND to_currency=USD)
                                                          │
                                              ┌───────────┤
                                              │           │
                                           Matched    Unmatched
                                              │           │
                                              ▼           ▼
                                      watsonx.data      Trash
                                      Presto table
                                      fx_eurusd_<YOUR_INITIALS>
                                              │
                                              ▼
                                   IBM Data Integration Project
                                              │
                                              ▼
                                   Line chart: EUR/USD rate over time
```

The visualisation you will produce at the end of Module 2 looks like this:

![Final visualisation — EUR/USD rate in IBM Data Integration](images/screenshots/m2-17-visualization-line-selected.png)

### Key learning objectives

| # | Objective |
|---|-----------|
| 1 | Build a StreamSets DataOps pipeline that reads from Kafka, adds a timestamp field, and filters records by currency pair |
| 2 | Write filtered results to an Apache Iceberg table in watsonx.data |
| 3 | Import the table into an IBM Data Integration project and build a line chart visualisation |

---

## Lab Modules

| Module | Title | Who | Duration |
|--------|-------|-----|----------|
| **Setup** | [Confluent Cluster and watsonx.data Environment Preparation](setup/README.md) | Tutor / Pre-lab | ~30 min |
| **Module 1** | [Build the StreamSets Kafka-to-Presto Pipeline](module1/README.md) | Participant | ~90 min |
| **Module 2** | [Import the Table & Visualise in IBM Data Integration](module2/README.md) | Participant | ~45 min |

> **Note for participants:** The tutor will have completed the **Setup** module before the lab session starts. You will be given the Confluent bootstrap URL, Schema Registry endpoint and credentials before beginning Module 1.

---

## Prerequisites

### For participants
- An IBM ID with access to the lab watsonx.data Integration environment
- Access to the IBM Data Integration instance at `ca-tor.dai.cloud.ibm.com` 
- A web browser (Chrome or Firefox recommended)

---

## Data Schema

Each message published to the Kafka topic represents a single exchange rate reading between two currencies:

| Field | Type | Description |
|-------|------|-------------|
| `from_currency` | string | Source currency — one of EUR, GBP, CHF, CAD, AUD, NZD, SEK, NOK, DKK, SGD |
| `to_currency` | string | Target currency — one of USD, JPY, CNY, INR, BRL, MXN, ZAR, HKD, KRW, TRY |
| `rate` | double | Exchange rate: 1 unit of `from_currency` expressed in `to_currency` |

The Kafka topic carries rates for all currency combinations. The StreamSets pipeline **filters** this stream and writes only EUR/USD records to the lakehouse table:

| Field | Type | Description |
|-------|------|-------------|
| `from_currency` | string | Source currency — always `"EUR"` |
| `to_currency` | string | Target currency — always `"USD"` |
| `timestamp` | datetime | Kafka broker timestamp promoted to a record field |
| `rate` | double | EUR/USD exchange rate at the time of the reading |

---

## Lab Structure

```
StreamSets/
├── README.md                       ← You are here
├── setup/
│   ├── README.md              ← Step-by-step tutor guide
│   └── alphavantage-generator.json ← Connector config for multi-currency feed
├── module1/
│   └── README.md                  ← Participant guide: StreamSets pipeline build
└── module2/
    └── README.md                  ← Participant guide: data import & visualisation
```

---

## Ready to Start?

**Tutors** → open [`setup/README.md`](setup/README.md) and follow every step before participants arrive.

**Participants** → wait for your tutor to hand out the connection details sheet, then open **[Module 1](module1/README.md)** and follow the steps in order.

---


<div align="center">
<sub>IBM watsonx.data Integration Lab &nbsp;·&nbsp; Real-Time FX Rate Analytics &nbsp;·&nbsp; EUR/USD</sub>
<br/><br/>
<sub>Guide maintained by the IBM France Client Engineering team</sub>
</div>
