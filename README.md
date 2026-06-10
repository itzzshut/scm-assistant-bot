# SCM Supply Chain RAG Assistant

A Retrieval-Augmented Generation (RAG) chatbot built using Flowise Cloud to answer questions about a supplier network using structured supplier performance data and supply chain governance policies.

---

## Public Chatbot URL

Deployment link could not be generated successfully due to Flowise Cloud deployment limitations encountered during submission.

Screenshots demonstrating the completed workflow, retrieval testing, and chatbot execution are included in the `/screenshots` folder.

---

## Models Used

### LLM

* Google Gemini 2.5 Flash

### Embeddings

* Google Gemini Embedding (`gemini-embedding-001`)

### Vector Store

* Pinecone

---

## Dataset Used

The chatbot uses the following data sources:

### 1. supplier_performance_data.csv

* 2,000 purchase orders
* 116 suppliers
* 27 attributes including:

  * OTD (On-Time Delivery) Rate
  * Defect Rate
  * Compliance Score
  * Risk Level
  * Disruption Flags
  * PO Value
  * Sustainability Metrics
  * Lead Times

### 2. SupplyChain_Governance_Policy_v3.2.pdf

A 10-section governance document containing:

* Tier thresholds
* Service Level Agreements (SLAs)
* Audit procedures
* Penalty rules
* Supplier watch-list policies
* Disruption response procedures
* Escalation guidelines

---

## Architecture

User Query

↓

Google Gemini 2.5 Flash

↓

Multi Retrieval QA Chain

↓

Vector Store Retriever

↓

Pinecone Vector Database

↓

Google Gemini Embeddings

↓

Retrieved Context

↓

Final Response

---

## Chunk Configurations Tried

### Configuration 1

* Splitter: Recursive Character Text Splitter
* Chunk Size: 1000
* Chunk Overlap: 0

**Result:**

* Faster retrieval
* Better contextual coherence
* Selected for the final implementation

---

### Configuration 2

* Splitter: Recursive Character Text Splitter
* Chunk Size: 800
* Chunk Overlap: 100

**Result:**

* Improved context continuity
* Slight increase in retrieval latency
* Produced similar retrieval quality

---

## Validation Questions & Answers

### Q1. Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer:**

11 Tier-3 suppliers: Dravex Components India, Plataforma Metales SA, Maghreb Castworks, Helios Pack Greece, Cerromax Mineria, Orinoco Pack SAPI, Quetzal Textiles, Sibertek Molding, Archipelago PCB Corp, Varna Electronics EAD, Deltaforge Vietnam.

All are High Risk with an active flag → Level 3 Activate per Policy §9 (CPO escalation + alternate supplier at minimum 40% volume).

---

### Q2. Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**Answer:**

19 suppliers qualify: Borealis Composites, Crestline Chemical Supply, Fenwick Alloy Solutions, Hanguk Circuit Works, Hokkaido Alloy Tech, Krauss-Polymex GmbH, Lakeshore Components, Lumivex Semiconductor NL, Maplewood Polymer Corp, Norbec Alloy Works, Nordloom Finland Oy, Orrentek Precision Mfg, Ostwind Composites AG, PrecisionForge Taiyuan, Solveig Eco Packaging, Straits Packaging Hub, Tasman Circuit Boards, Toreval Electronics, Valdoro Special Alloys.

Criteria (Policy §4.2): Tier-1 + OTD ≥ 93% + Defect < 0.5% + Sustainability Score ≥ 85.

---

### Q3. Which region has the highest total PO value, and does it breach the concentration limit?

**Answer:**

EMEA at $193,987,179.91 — approximately 48.5% of total spend ($399,563,494.10).

This breaches the 45% regional concentration cap (Policy §5.3), requiring a Diversification Plan within 60 days.

---

### Q4. Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer:**

11 suppliers (Compliance Score < 60):

Deltaforge Vietnam, Maghreb Castworks, Helios Pack Greece, Cerromax Mineria, Orinoco Pack SAPI, Varna Electronics EAD, Quetzal Textiles, Plataforma Metales SA, Archipelago PCB Corp, Dravex Components India, Sibertek Molding.

SWL restricts new PO issuance to 20% of prior quarter volume (Policy §3.4).

---

### Q5. Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer:**

Mechanical Components — average 2.12% across 360 POs.

Below the Tier-2 ceiling of 2.50% (Policy §3.2), so no breach — but approaching the limit.

---

## Screenshots

The repository includes screenshots demonstrating the implementation process:

* Document Store setup and data loading
* Retrieval Playground testing
* Flowise Chatflow canvas
* Chatbot interaction examples

All screenshots are available in the `/screenshots` directory.

---

## Future Improvements

Given additional time, the following enhancements would be implemented:

* Deploy and maintain a publicly accessible chatbot URL.
* Improve retrieval quality using hybrid search and reranking techniques.
* Add conversational memory for multi-turn interactions.
* Display source citations alongside generated responses.
* Introduce monitoring, analytics, and evaluation metrics.
* Implement role-based access control and user authentication.

---

## Repository Structure

```
scm-assistant-bot/
├── README.md
├── .gitignore
├── scm_assistant.json
└── screenshots/
    ├── 01_document_store.png
    ├── 02_retrieval_playground.png
    ├── 03_flowise_canvas.png
    └── 04_chatbot_testing.png
```

---

## Disclaimer

API keys and sensitive credentials have not been included in this repository. The implementation was developed solely for the Trinamix Junior AI Engineer hiring task.
