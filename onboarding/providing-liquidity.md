# Providing Liquidity

Liquidity providers are the backbone of the **Twilight Pool**, the settlement layer that powers all perpetual trades on the Twilight network.\
By depositing BTC or NYKS test tokens into the pool, LPs supply collateral that traders borrow against — earning yield from trading activity, fees, and liquidation profits.

***

### What is the Twilight Pool?

Unlike order book exchanges, Twilight uses a **single-sided liquidity pool**.\
All trades settle directly against this pool, meaning there are no counterparties or order matches — only traders and the protocol.

When a trader opens a position:

* The pool acts as the counterparty.
* Profitable trades are paid out from the pool.
* Losses and liquidations flow back into the pool, increasing its value.

This design enables continuous liquidity and deterministic execution without slippage or front-running.

***

### How LPs Earn

Liquidity providers earn yield through three main sources:

| Source                       | Description                                                                           |
| ---------------------------- | ------------------------------------------------------------------------------------- |
| **Trading Fees**             | Every position opened or closed generates protocol fees distributed to LPs.           |
| **Liquidation Profits**      | When traders are liquidated, the collateral above debt value is credited to the pool. |
| **Funding/Skew Adjustments** | The pool periodically rebalances exposure; positive skew returns may accrue to LPs.   |

All yields are denominated in **BTC**, giving LPs direct bitcoin-denominated returns.

***

### 🪙 Becoming a Liquidity Provider

#### Step-by-Step

1. **Navigate to the Lend Page**\
   [https://frontend.twilight.rest/lend](https://frontend.twilight.rest/lend)
2. **Connect Your Keplr Wallet**\
   Ensure your wallet is connected to the **Twilight Testnet** and funded with NYKS and BTC test tokens.
3. **Select Asset and Amount**\
   Choose the token (BTC or NYKS) you wish to deposit.\
   The available balance will appear automatically.
4. **Confirm Deposit**\
   Click **Provide Liquidity** and approve the transaction in Keplr.\
   Once confirmed, your assets are added to the Twilight Pool.
5. **Track Pool Share**\
   After deposit, you receive **LP tokens** representing your share of the pool.\
   These can be viewed and redeemed anytime from the **Lend Dashboard**.

***

### Withdrawing Liquidity

To withdraw:

1. Go to the **Lend** page.
2. Click **Withdraw** next to your active position.
3. Enter the amount to withdraw and confirm in Keplr.

Withdrawals return your principal plus accumulated yield (if any) directly to your **Funding Account**.

> ⚠️ Liquidity withdrawals are subject to cooldown periods during extreme volatility to protect the pool’s solvency.

***

### Risk & Rewards

Providing liquidity involves exposure to trading outcomes:

* If traders profit overall, pool value may decrease temporarily.
* If traders lose (liquidations or net negative PnL), the pool accrues gains.

Over time, LPs earn sustainable yield driven by trading volume and risk distribution — similar to market-making returns in traditional derivatives markets.

***

### Transparency & Verification

All pool transactions and yield calculations are **publicly verifiable** on the Nyks chain.\
However, individual trader positions remain encrypted on zkOS, ensuring privacy for users while preserving full transparency for LP accounting.

***

### Key Takeaways

* LPs supply liquidity to the Twilight Pool, enabling zero-cost leverage for traders.
* Returns are generated from trading fees, liquidations, and pool skew adjustments.
* LP positions are non-custodial, withdrawable, and verifiable through on-chain data.
* All yield is denominated in BTC — providing true bitcoin yield exposure.

***
