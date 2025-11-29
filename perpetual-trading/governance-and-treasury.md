---
description: >-
  This page describes how the protocol is administered today and how the
  treasury/insurance will function on mainnet.
---

# Governance & Treasury

***

### 1) Governance (Current)

* **Testnet governance:** centrally managed by core contributors (ops, parameters, upgrades).
* **Parameter changes:** announced in release notes; reflected in relayer/config as needed.
* **Community input:** via GitHub issues and public discussions; no on-chain voting at this stage.

There is **no governance token** and **no DAO transition plan** at this time.

***

### 2) Treasury & Insurance (Mainnet Plan)

To improve resilience during volatility and tail events, **mainnet** will introduce a **protocol treasury/insurance fund** with the following goals:

* **Shock absorption:** cover deficits when extreme moves push trader losses beyond seized margin
* **Stability incentives:** fund programs that reward capital residency and rapid top-ups at high utilization
* **Operational safety:** support oracle/index redundancy and incident response

#### 2.1 Funding Sources (Illustrative)

Exact splits will be published pre-launch; examples include:

* A configurable share of **fees**
* A configurable share of **net funding residuals**
* Occasional **protocol allocations** (e.g., bootstrap grants)

#### 2.2 Withdrawal & Usage Policy

* Treasury usage is **programmatic and rule-bound** (e.g., deficit coverage after liquidation events)
* Transparent accounting via periodic reports and on-chain attestations where applicable
* Change management via published governance processes (still core-managed initially)

***

### 3) Parameter Stewardship

Initial mainnet will keep parameter control **with core maintainers**, including:

* Funding curve scalar `ψ`, utilization cap `U_cap`
* Fee schedule (maker/taker)
* Oracle/index composition and guard-bands
* Risk guards (per-order margin caps, throttles)

Changes will be documented with clear activation epochs and migration steps.

***

### 4) Community & Transparency

* **Specs & change logs:** consolidated in the docs site and GitHub repos
* **Issue tracking:** please open an issue in the appropriate GitHub repository for proposals or concerns
* **No token, no DAO:** the focus is operational excellence and stability first; governance decentralization is not planned at this time

***
