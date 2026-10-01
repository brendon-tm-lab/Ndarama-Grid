1. Who experiences this problem and what evidence exists?
Primary affected groups:

Artisanal and small-scale (ASM) miners and cooperatives — a large share of Zimbabwe's mining workforce — who work in pits and shafts with zero continuous safety monitoring, sell mineral parcels with no verifiable provenance, and are historically underpaid by middlemen because they cannot prove origin or quality.
Small-scale formal operators, who cannot afford dedicated compliance consultants for EMA and Ministry of Mines reporting, yet face statutory obligations (e.g. the mandatory 2-hour incident notification window under the Environmental Management Act).
Buyers and exporters, who have no trusted way to verify a parcel's origin, exposing them to responsible-sourcing and export-compliance risk.
Regulators (EMA, Ministry of Mines), who lack continuous field data and rely on periodic manual inspection.
Mining communities, who bear the human cost — occupational lung disease from silica dust is a leading occupational illness in the sector, and gas/rockfall incidents are discovered after the fact rather than prevented.
Evidence the problem exists:

The structural nature of the sector itself: ASM sites are remote and typically outside cellular coverage, which is why cloud-only tools have failed to penetrate them — connectivity deserts and safety blind spots are the same geography.
Occupational disease data: silica dust exposure is the leading cause of occupational lung disease among miners — a direct consequence of unmonitored particulate levels.
Market behaviour: persistent price spreads between middleman offers and fair reference value (e.g. middleman pricing below verified fair value across gold, lithium, PGEs and coal) demonstrate the provenance-information gap is actively exploited.
Regulatory reality: EMA and Ministry of Mines reporting requirements are well documented in statute, yet compliance remains concentrated among large operators — evidence of the affordability gap.
2. Describe the innovation / proposed solution and how it works
Ndarama Grid is a modular, offline-first platform applying AI and low-power sensing to safety, provenance, connectivity and compliance at once. It ships as four modules sharing one data backbone, so a site can adopt one and grow into the rest:

How it works — hub-and-spoke flow:

Field layer generates data. PitGuard sensor nodes (ESP32-based, solar + battery) continuously measure gas (CO, CH4), silica dust (PM2.5/PM10) and seismic vibration; fixed and drone cameras run on-device computer vision for PPE compliance and restricted-zone intrusion; the solar survey drone performs periodic aerial passes compared via photogrammetry for pit-wall and tailings stability. OreChain scan points tag every mineral parcel with QR/NFC linked to GPS, timestamp and registered miner.
Mesh carries it out. Low-power LoRa nodes form a self-healing mesh relaying data hop-by-hop to a gateway, with store-and-forward queuing — safety alerts are priority-queued ahead of routine ledger sync so an urgent signal is never stuck behind bulk traffic. Only the gateway needs backhaul (cellular or satellite).
The Hub persists it. A FastAPI + PostgreSQL system of record (Docker-orchestrated) with one shared schema and authentication layer across all modules.
Copilot turns it into decisions. A multi-LLM router grounded in the Mines and Minerals Act and EMA requirements drafts compliance reports for human sign-off, suggests beneficiation pathways from parcel grade data, and answers plain-language questions.
Everyone accesses it at their level. Web dashboards for operators and regulators; USSD/SMS gateway so a feature-phone miner receives the same gas warning as a supervisor with a smartphone. OreChain's hash-chained, cryptographically signed custody ledger gives buyers a verifiable provenance chain, letting miners negotiate premium fair-value pricing.
3. Why the solution is technically feasible
Proven building blocks, no exotic hardware:

Commodity, locally serviceable components: ESP32 microcontrollers, electrochemical/MOS gas sensors, low-cost laser particle counters, MEMS accelerometers and LoRa 868/915 MHz ISM radios — all off-the-shelf, low-power, long-range, and within ASM-cooperative budgets.
The hard architectural problem is already solved by design choice: the offline-first, Docker-orchestrated hub-and-spoke pattern (FastAPI + PostgreSQL + Alembic) is not a novel bet — it is the same proven architecture pattern already used in a production EHR platform built for clinics under identical connectivity constraints, adapted here to mining.
The integrity mechanism is already implemented: cryptographic hash-chained signing is a pattern the applicant has already designed and shipped across five prior prototypes — applied here to mineral custody events instead of software licensing.
AI layer is vendor-resilient and grounded: Copilot runs on a multi-LLM router (OpenAI/Gemini/Anthropic) — a routing pattern already used in production — so no single vendor's pricing or uptime blocks it, and every output is a human-review artefact rather than an autonomous filing.
Edge AI is realistic at this scale: on-device vision inference for PPE detection runs on Jetson Nano-class boards, requiring no live cloud connection — exactly what offline sites demand.
Scalability is structural, not aspirational: a new site is additional field nodes plus a Mesh gateway — no backend re-architecture, because all four modules already write to one shared Hub schema.
In short: every risky component of Ndarama Grid substitutes a pattern with a working precedent, and the six-month incubation roadmap (PitGuard pilot → Mesh → OreChain scan points → Copilot online) sequences delivery so feasibility is demonstrated incrementally, not all at once.
