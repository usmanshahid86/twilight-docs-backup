# Risk Parameters

This page summarizes the guard-rails that keep trading stable and the pool solvent. Values shown for testnet are indicative; mainnet parameters will be announced prior to launch.

***

### 1) Position & Leverage

* **Contract type:** BTC-USD inverse perpetual
* **Leverage:** up to **50×** (testnet; mainnet target same)
* **Margin model:** **isolated** per position (no cross margin, no post-open top-ups)
* **Per-order margin cap (testnet guard):** soft limit ≈ **0.10 BTC** equivalent (relayer policy)

***

### 2) Maintenance & Liquidation

**Maintenance margin** is computed by the protocol/SDK per position:

```
MaintenanceMargin = (0.004 × PositionSize)
                    + (fee% × BankruptcyValue)
	•	    + (funding% × BankruptcyValue)
```

* **Liquidation trigger:** when equity falls to the computed MaintenanceMargin
* **Liquidation action:** **full close** at oracle mark; **full margin forfeiture**
* **No partial liquidation tiers** (testnet)

> See also: _PnL & Settlement_ and _Fees & Funding_ for how fee/funding terms contribute to the formula.

***

### 3) Pool Utilization & Capacity

Twilight is a single-asset (BTC) collateral pool. Capacity is governed by utilization:

* **TVL** — total LP deposits (BTC)
* **TTM** — total trader margin locked (BTC)
* **Utilization** `U = TTM / TVL`
* **Utilization cap (`U_cap`)**: **90%** (testnet policy)
  * On breach, the relayer may throttle new opens until `U < U_cap`.

***

### 4) Pricing & Oracle Assumptions

* **Execution price:** external spot **Binance mid** (0.5s tick)
* **No AMM curve, no on-venue orderbook price discovery**
* **Roadmap:** multi-venue index (incl. on-chain oracles like Chainlink/Pyth), freshness bands, circuit breakers

> Current testnet has **no oracle circuit breaker** active; the relayer falls back to last valid tick if the feed stalls.

***

### 5) Funding (Skew Compensation)

* **Interval:** hourly
* **Purpose:** discourage one-sided positioning; compensate pool for directional inventory
* **Sign:** long-heavy → longs pay; short-heavy → shorts pay
* **Residual:** payer/receiver imbalance flows to **Pool NAV**

> Full formula and examples: _Fees & Funding_.

***

### 6) Withdrawals (LP)

* **Testnet:** withdrawals are **instant** after block settlement
* **Mainnet (preview):** subject to treasury/insurance design and possible safety delays during stress

***

### 7) What’s Enforced Where?

| Control                     | Enforced by               | Notes                                  |
| --------------------------- | ------------------------- | -------------------------------------- |
| Max leverage, margin rules  | Relayer + Validator check | Position-level; verified at settlement |
| Liquidation threshold       | Protocol/SDK computation  | Full close on breach                   |
| Utilization cap throttling  | Relayer policy            | Protects pool solvency                 |
| Oracle selection & fallback | Relayer + infra           | Index/circuit breakers on roadmap      |

***

**Related:**

* [_Pricing & Execution_ ](pricing-and-execution.md)— how orders are priced and sequenced
* [_PnL & Settlement_ ](pnl-and-settlement.md)— how BTC PnL settles and updates Pool NAV

