# Twilight Pool Mechanics

> Audience: Liquidity Providers (LPs), Market Makers, and institutional participants interested in understanding how capital flows and earns yield in Twilight’s perpetual DEX.

***

### 1. Overview

The Twilight Liquidity Pool is the backbone of the protocol’s trading engine — a single BTC-denominated capital base that powers all leveraged trading on the platform.

Every position opened on Twilight, whether long or short, is executed against this shared pool, not against another trader or AMM curve.

By aggregating all deposits into one collateral base, Twilight eliminates AMM slippage, impermanent loss, and borrowing costs. Instead, the pool earns revenue from trading activity — fees, funding differentials, and liquidation proceeds — while absorbing the directional risk of trader performance.

This model allows Twilight to operate as a basis-free, interest-free perpetual exchange where BTC remains the sole collateral, and liquidity availability is determined by the pool’s utilization.

***

### 2. Pool Composition and Accounting

All deposits and settlements on the exchange are denominated in SATS — the BTC test asset used on Twilight Testnet.

There are no transferable LP tokens in this phase; deposits are tracked internally, and balances can be withdrawn instantly when utilization allows.

| Metric                      | Symbol                         | Description                                  |
| --------------------------- | ------------------------------ | -------------------------------------------- |
| Total Pool Value (TVL)      | Σ deposits\_btc                | Total BTC collateral supplied by LPs.        |
| Total Trader Margin (TTM)   | Σ trader\_margin\_locked\_btc  | BTC margin locked for active positions.      |
| Total Amount Borrowed (TTM) | Σtrader\_margin\_borrowed\_btc | Total amount of BTC borrowed from the pool.  |
| Free Liquidity              | max(TVL − TTM, 0)              | BTC available for new trades or withdrawals. |
| Utilization (U)             | TAB / TVL                      | Ratio of active exposure to total pool size. |

When utilization approaches the configured cap (U\_cap = 90%), new position openings are throttled until additional capital is added.

This ensures that trader exposure always remains fully collateralized by BTC on the pool.

Withdrawals are processed instantly as long as free liquidity remains. No lockup periods or cooldowns are active during the testnet.

***

### 3. Economic Flows

All economic activity from trading is consolidated into the pool’s Net Asset Value (NAV).

There is no protocol treasury, insurance fund, or DAO fee split in the testnet — 100% of proceeds accrue to LPs.

#### 3.1 Trading Fees

Each trade on Twilight contributes to the pool’s NAV through standard maker and taker fees:

* Taker fee: 0.04% of trade notional.
*   Maker fee: 0.02% of trade notional.

    Both are denominated in BTC (SATS) and credited directly to the pool.

#### 3.2 Funding Transfers

Funding compensates the pool for directional skew created by trader positioning.

It redistributes value between long and short traders and sends any remaining difference to the pool.

Funding is calculated hourly based on open interest imbalance.

See Funding for the detailed formula and application logic.

#### 3.3 Liquidations

When a trader’s margin falls below maintenance thresholds, the position is immediately liquidated at the current oracle mark price.

The trader’s entire margin is absorbed into the pool NAV.

If unrealized losses exceed posted margin, the deficit is borne by the pool.

***

### 4. Risk Controls and Guardrails

| Parameter                | Testnet Value | Description                                                  |
| ------------------------ | ------------- | ------------------------------------------------------------ |
| Initial Deposit          | 0.5 BTC       | Minimum LP contribution to join the pool.                    |
| Utilization Cap (U\_cap) | 90%           | New opens throttle above this level.                         |
| Top-Up SLA               | 5 min         | LPs must add collateral or reduce exposure after breach.     |
| Capital Uptime Target    | ≥ 99.5%       | Informational metric for reliability tracking.               |
| Withdrawal Policy        | Instant       | No enforced delay; 24 h notice recommended for mainnet.      |
| Haircut Reserve          | Inactive      | Placeholder for future risk buffers; not applied in testnet. |

These parameters maintain capital solvency and ensure that all trading activity remains backed by BTC collateral.

***

### 5. LP Lifecycle

1. Deposit — Add SATS to the pool via frontend or SDK.
2. Monitor — Track pool utilization, NAV, and funding activity via /api/lend\_pool\_info.
3. Earn — Collect passive yield from trading volume and funding flow.
4. Top-Up — Add more collateral if utilization exceeds the cap.
5. Withdraw — Instantly remove capital when liquidity allows.

Participation is permissionless; any funded wallet can act as an LP.

***

### 6. Key Metrics

| Metric       | Definition                         | Purpose                                      |
| ------------ | ---------------------------------- | -------------------------------------------- |
| TVL          | Total BTC collateral deposited     | Pool depth and size indicator.               |
| TTM          | Trader margin currently locked     | Exposure backed by pool collateral.          |
| U            | TTM / TVL                          | Liquidity utilization ratio.                 |
| Funding Rate | Skew-based hourly transfer         | Incentive to maintain balance between sides. |
| NAV Change   | Δ from fees, funding, liquidations | Reflects pool earnings and losses.           |

These metrics are publicly queryable through the Twilight REST API and refresh in near real-time.

***

### 7. Summary

The Twilight Pool consolidates BTC collateral into a unified liquidity layer that powers perpetual trading without intermediaries, interest costs, or AMM mechanics.

Its health defines the solvency, depth, and yield potential of the entire protocol — making it the foundation upon which all trading activity in Twilight is built.

***

See also:

[Funding](../perpetual-trading/fees-and-funding.md) · [Fees](../perpetual-trading/fees-and-funding.md) · [Liquidations](../perpetual-trading/margin-and-liquidations.md) · [Risk Parameters](../perpetual-trading/risk-parameters.md)
