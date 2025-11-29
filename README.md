# Introduction

Bitcoin’s evolution into a multi-trillion-dollar asset class has outgrown the infrastructure that serves it. Existing trading venues either sacrifice privacy for liquidity or replicate the custodial, interest-bearing models of traditional finance.

Twilight was built to change that — to make Bitcoin the foundation of a secure, privacy-preserving, and fully on-chain derivatives market.

Twilight is a Bitcoin-first encrypted perpetual DEX designed to merge the efficiency of perpetual futures with the integrity of self-custodied settlement. It introduces a verifiable yet private market layer where traders, liquidity providers, and validators coordinate through cryptography rather than intermediaries.

The Twilight platform can be accessed via:

[https://frontend.twilight.rest/](https://staging-frontend.twilight.rest/)

***

### Why Twilight Exists

Modern derivatives markets revolve around leveraged perpetual contracts. They power everything from delta-neutral yield strategies to institutional hedging — yet, most of them:

* expose every position publicly on-chain,
* charge continuous funding or interest costs,
* and remain vulnerable to liquidation cascades and MEV attacks.

These inefficiencies penalize both traders and liquidity providers while amplifying systemic risk.

Twilight re-architects this model from first principles.

It allows participants to hedge, leverage, and earn yield without paying margin interest or revealing their exposure. The exchange’s privacy layer (zkOS) encrypts position data using Bulletproofs, while still proving every trade’s validity on-chain.

The result is a new kind of perpetual DEX: funding remains transparent, PnL remains private, and execution remains verifiable.

***

### The Core Architecture

Twilight operates on three interlocking systems:

1. Nyks Chain (Cosmos SDK) – A Tendermint-based consensus chain that records verifiable exchange data and publishes state updates for all shielded accounts.
2. Relayer Core – The off-chain execution and settlement engine that prices trades directly against the Twilight Pool, manages utilization and funding, and coordinates validator verification.
3. zkOS (Zero-Knowledge Operating System) – A UTXO-based privacy layer using Bulletproofs to keep margin, leverage, and balance data confidential while ensuring correctness through on-chain proof verification.

Together, these components form a publicly auditable yet privacy-preserving infrastructure for BTC-denominated perpetuals.

***

### A New Kind of Market Structure



Twilight replaces order-book matching with a single-asset collateral pool where all trades settle directly against pooled BTC liquidity.

* The pool profits from trader losses, liquidation events, and funding differentials.
* Liquidity providers (LPs) earn BTC-denominated yield without impermanent loss.
* Pricing is derived from external spot markets (currently Binance mid-price, expanding to multi-venue oracles).
* Funding rates, computed hourly, adjust for open-interest skew — not to peg price, but to reward market balance.

This model removes the need for AMM curves, interest-bearing borrowing, or centralized margin systems — reducing friction for both retail and institutional participants.

***

### Privacy as Infrastructure

Twilight’s privacy layer is not an add-on — it’s the foundation.

Every position, transfer, and settlement is wrapped in a zero-knowledge proof ensuring that:

* The trade was valid.
* The trader had sufficient margin.
* The pool’s accounting remained correct.
*   Yet, no on-chain observer can infer who traded, how much leverage they used, or what PnL they hold.

    At the same time, all aggregate metrics — utilization, funding rate, pool NAV — remain fully transparent for verification.

### For Traders, Developers, and Institutions

* For Traders: Hedge exposure and manage leverage at zero ongoing cost while preserving privacy.
* For Institutions: Deploy delta-neutral and basis strategies without public exposure or capital inefficiency.
* For Developers: Build on a modular infrastructure — from zkOS RPCs to SDK integrations — to extend Twilight’s ecosystem.
* For Validators and Operators: Run the core infrastructure that secures encrypted settlement and keeps markets decentralized.

### Looking Ahead

Twilight launches its public testnet in November 2025, with a mainnet rollout planned for Q1 2026.

This testnet phase invites node operators, relayers, and developers to participate in building the encrypted exchange layer that will anchor Bitcoin’s next chapter — a chapter defined not by speculation, but by privacy, stability, and true decentralization.
