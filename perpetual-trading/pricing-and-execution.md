# Pricing & Execution

> Audience: Traders, Market Makers, and integrators interested in how orders are priced, triggered, and executed within the Twilight perpetual DEX.

### 1. Overview

Twilight’s trading engine uses an oracle-priced execution model rather than an order book or AMM.

All trades are executed directly against the pooled BTC collateral at an externally sourced spot price — ensuring deterministic fills, zero AMM slippage, and complete transparency of execution logic.

The price integrity of every position rests on a single principle: execution follows the oracle, not trader interactions.

This creates a uniform reference for all participants while preserving the privacy and finality benefits of on-chain settlement.

***

### 2. Oracle Pricing

The Twilight Relayer sources its real-time BTC/USD price feed directly from Binance Spot Market mid-price through a continuous web stream.

| Parameter        | Description                                                                                                            |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Feed Source      | Binance Spot mid-price (direct websocket)                                                                              |
| Update Frequency | Real-time; identical to Binance tick rate                                                                              |
| Aggregation      | Mid-price = (Best Bid + Best Ask) / 2                                                                                  |
| Freshness        | No ignore filters; last valid tick is used if feed stalls                                                              |
| Future Index     | Multi-source weighted oracle combining Binance, OKX, Coinbase, Kraken, and optionally on-chain feeds (Chainlink, Pyth) |

The current testnet uses Binance mid-price exclusively; production deployments will introduce a weighted index with confidence bands and latency checks to further strengthen oracle robustness.

***

### 3. Execution Model

Twilight has no on-platform price discovery.

All fills occur at the current oracle mark against the pool, which acts as the universal counterparty.

This ensures uniform execution and consistent accounting across all participants.

| Concept           | Behavior                                                                                  |
| ----------------- | ----------------------------------------------------------------------------------------- |
| Order Type        | Market or Limit (oracle-triggered)                                                        |
| Execution Trigger | Limit Buy fills when oracle\_mid ≤ limit; Limit Sell when oracle\_mid ≥ limit             |
| Matching Logic    | Direct execution vs. pool; no user-to-user matching                                       |
| Order Resting     | None — orders execute fully or remain pending until trigger                               |
| Partial Fills     | Not supported; every fill is atomic                                                       |
| Settlement Level  | Block-confirmed — execution completes at next block once UTXO state updates               |
| Privacy Layer     | Each order is wrapped in a shielded transaction via zkOS to ensure margin confidentiality |

This design guarantees that all participants receive identical pricing and eliminates competition or slippage between traders.

***

### 4. Trade Finality and Sequencing

Each order submitted to the relayer receives a trusted timestamp upon arrival.

The relayer sequences all inbound instructions and ensures fair ordering before forwarding shielded transactions to the validator layer.

Validators then verify sequencing integrity and confirm execution at the next block, updating both trader UTXOs and pool state.

This hybrid model preserves low-latency execution while maintaining verifiable fairness and auditability on-chain.

***

### 5. Handling Oracle Latency and Failures

If the oracle feed temporarily freezes, Twilight automatically falls back to the last known valid tick until new data arrives.

Trading continues seamlessly at that frozen mark, preventing inconsistent fills across participants.

Future versions will introduce oracle freshness checks and fail-safe halts when data latency exceeds configured limits.

***

### 6. Pricing Implications

* No Slippage: All trades execute at the oracle mark regardless of trade size.
* No Spread Manipulation: The oracle defines the sole reference price; traders cannot move it.
* Deterministic Fills: Identical inputs always produce identical outcomes.
* Block-Level Finality: Once confirmed on Nyks, trade settlement is irreversible.
* Uniform PnL Basis: Unrealized and realized PnL are computed from the same oracle mark (see PnL & Settlement).

***

### 7. Future Roadmap

Upcoming improvements to the pricing and execution layer include:

* Multi-source Oracle Index with exchange weightings and volatility filtering.
* Confidence Bands to reject stale or outlier ticks.
* Maker Stability Program integrating oracle latency and execution quality scoring.
* Adaptive Order Types such as conditional stops and time-in-force triggers.

***

### 8. Summary

Twilight’s oracle-priced execution system replaces the traditional order book with a transparent, deterministic, and privacy-preserving trade model.

Every order settles at the same globally verifiable mark, eliminating front-running, slippage, and interest costs — while preserving Bitcoin as the native unit of margin, PnL, and liquidity.

***

See also:

[Pool Mechanics](twilight-pool-mechanics.md) · [Funding](fees-and-funding.md) · [PnL & Settlement](pnl-and-settlement.md) · [Liquidations](margin-and-liquidations.md)
