---
description: A unified collection of APIs for trading, proof verification, and integration.
---

# API Suite

The Twilight API suite gives developers, integrators, and operators direct programmatic access to the core components of the Twilight ecosystem infrastructure.

These APIs enable you to connect trading systems, automate strategies, run infrastructure, and interact with Twilight’s zero-knowledge services.



Twilight exposes two primary API layers:

***

### Relayer API

The Relayer API powers the trading layer of Twilight.

It provides endpoints for:

* Market data retrieval (tickers, order books, trades)
* Order placement and management
* Real-time WebSocket streams for execution and price updates



This API is ideal for:

* Programmatic traders and market-makers
* Wallets or dashboards needing live market data
* Integrators building execution or analytics services



→ See Relayer API for detailed REST and WebSocket specifications.

***

### ZkOS RPC API



The ZkOS (Zero-Knowledge Operating System) API connects to Twilight’s cryptographic verification layer.

It enables developers and operators to:

* Submit and verify zero-knowledge proofs
* Query commitments, nullifiers, and proof statuses



This interface is mainly used by:

* Node operators and relayers
* Developers building privacy-preserving applications
* Services that require on-chain proof validation



→ See zkOS RPC API for usage examples and RPC structure.

***

### Developer Resources

All Twilight APIs follow standard REST or JSON-RPC 2.0 conventions and are available in testnet and production environments.

For SDKs, code samples, and integration helpers, visit the SDK Integration section.
