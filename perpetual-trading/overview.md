# Overview

## Perpetual Trading Overview

Twilight introduces a new standard for Bitcoin perpetuals — combining privacy, capital efficiency, and on-chain verifiability within a single decentralized protocol.

Unlike conventional perp DEXs that rely on funding rate arbitrage or order book depth, Twilight uses a spot-priced liquidity pool architecture to maintain balance between longs and shorts — enabling interest-free leverage and basis-neutral trading.

***

### Core Concept

In most perpetual exchanges, funding rates are used to align the perp price with spot markets.

Twilight redefines this mechanism into what it calls hourly market funding — a pool-based system that redistributes value between longs and shorts based on market imbalance, not price deviation.

The rate is computed each hour from the relative open interest across the pool:

```mathml
skew_raw = (L_usd - S_usd) / (L_usd + S_usd)   # Range: -1 to +1
rate_hr  = (skew_raw * skew_raw) / (ψ * 8)     # Unsigned rate
rate_hr  = rate_hr * sign(skew_raw)            # Directional funding
```

* L\_usd: total long position size
* S\_usd: total short position size
* ψ: pool stability parameter

A positive rate means longs pay shorts; a negative rate means shorts pay longs.

This ensures market balance without introducing interest costs or external spot dependencies.

### What This Means for Traders

* Funding redistributes exposure, not debt.
* No borrowing costs → leverage is inherently interest-free.
* Pool incentives remain symmetric, rewarding contrarian liquidity that reduces skew.

All funding flows ultimately settle into the Twilight Liquidity Pool, increasing returns for liquidity providers.

***

### System Design at a Glance

Twilight’s trading layer is composed of three coordinated modules:

| Component    | Function                                                                                               |
| ------------ | ------------------------------------------------------------------------------------------------------ |
| Nyks Chain   | Publishes verifiable trade metadata and pool states using the Cosmos SDK and Tendermint BFT consensus. |
| Relayer Core | Executes trades against the pool, handles margin updates, and applies hourly market funding.           |
| zkOS         | Processes encrypted balances, ensuring all position data and PnL remain private yet provable on-chain. |

Together they deliver a trustless, privacy-preserving exchange with deterministic on-chain settlement.

***

### User Roles

| Role                                     | Description                                                                                                                   |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Trader                                   | Opens long or short positions using SATS as margin collateral; pays or earns hourly funding depending on market direction.    |
| Liquidity Provider (LP)                  | Supplies BTC or SATS liquidity to the pool and earns yield from trading fees, liquidations, and net funding inflows.          |
| Market Maker / Institutional Participant | Connects via the Relayer API for automated hedging or arbitrage; benefits from zero interest cost and programmable execution. |

***

### Key Advantages

* Interest-Free Leverage → No margin borrowing or financing costs.
* Hourly Market Funding → Dynamic rate balances open interest while rewarding contrarian liquidity.
* Encrypted Execution → All balances, positions, and PnL are shielded through zkOS.
* Bitcoin-Denominated Settlement → All margin and yield accounted in SATS.
* Basis-Neutral Design → Removes dependency on spot-perp price convergence.
* Unified Pool Architecture → All positions share a common liquidity source for deterministic pricing and instant execution.

***

### Lifecycle of a Trade

1. Position Opening → Trader deposits SATS as collateral; zkOS generates a proof of position.
2. Execution → Relayer executes trade directly against the Twilight Pool.
3. Funding Accrual → Hourly market funding is applied to margin continuously.
4. Settlement → On position close or liquidation, net PnL and accrued funding settle to the trader’s account and pool balance.
5. On-Chain Publication → An anonymized record of the trade and funding adjustment is published to Nyks for public verification.

***

### Next Steps

* [Twilight Pool Mechanics](twilight-pool-mechanics.md) → How liquidity is structured and returns are generated.
* [Pricing & Oracle Design ](pricing-and-execution.md)→ Learn how Twilight maintains spot-based pricing without order books.
* [Risk & Liquidation](risk-parameters.md) → Understand margin safety, liquidation triggers, and LP protection mechanisms.

***
