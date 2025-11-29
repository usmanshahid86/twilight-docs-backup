---
description: >-
  Learn how to build, integrate, and operate within the Twilight ecosystem —
  from SDK integration to raw API access.
---

# Developer Overview

Welcome to the Twilight Developer Portal — your technical entry point for building and integrating with the Twilight network.



This section provides the foundational resources and SDK guides needed to interact with Twilight’s APIs, privacy modules, and client interfaces. Whether you’re connecting a trading platform, developing analytical tools, or integrating shielded transactions, this documentation will help you navigate Twilight’s architecture with confidence.

***

### Architectural Overview



Twilight combines transparent execution with private settlement through three core layers:

| Layer            | Description                                                                                                                           |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Exchange Layer   | Handles perpetual markets, order matching, and data delivery through public and private REST APIs.                                    |
| ZkOS Layer       | Provides privacy and proof verification for shielded transactions using Bulletproofs and UTXO-based accounts, accessible through RPC. |
| Client SDK Layer | Offers a Rust-based toolkit for building, signing, and submitting transactions across both the Exchange and zkOS layers.              |

Together, these layers form a modular stack:

```
    - Clients → SDK → RPC APIs → zkOS + Exchange Core
```

This layered structure allows developers to compose both open and private logic, from simple REST integrations to advanced proof-based settlement flows.

***

### What You’ll Find Here



The Developer Portal focuses on the tools and APIs used to build applications, bots, and services on top of Twilight.

* `API Suite` — Documentation for the Relayer and ZkOS APIs (REST and RPC endpoints).
* `Client SDK Guide` — Learn how to integrate using the Twilight SDK to connect directly with the network.
* `Architecture References` — Understand how the Relayer and ZkOS components interact within the Nyks chain framework.



For validator setup and operational details, see the separate Node & Operator Guide section.

For security and audit best practices, refer to the Security Section.

***

### Getting Started

1.  Explore the API Suite

    Learn how to connect to the exchange, stream live market data, or query shielded states from zkOS RPC.
2.  Use the Twilight SDK

    Integrate directly from your application using the Rust-based client library — simplifying proof submissions, transaction construction, and signing.
3.  Run a Node or Validator

    If you’re contributing infrastructure, follow the Operator Guide to deploy validator or relayer nodes for the testnet.
4.  Review the Security Guidelines

    Understand slashing conditions, validator key management, and zkOS safety parameters.

***

### Developer Resources

A collection of links to the most useful repositories, APIs, and reference materials for Twilight developers:

| Resource                                                                         | Description                                               |
| -------------------------------------------------------------------------------- | --------------------------------------------------------- |
| [Twilight API Reference](https://docs.twilight.rest/)                            | Full REST and RPC endpoint documentation.                 |
| [Client SDK Documentation](https://docs.twilight.rest/?rust#twilight-clinet-sdk) | Rust SDK with complete module-level functions and types.  |
| [Twilight Project GitHub](https://github.com/twilight-project)                   | Source code, testnet deployments, and development tools.  |
| [Relayer Core](https://github.com/twilight-project/relayer-core)                 | Off-chain order matching and execution engine.            |
| [zkOS Rust](https://github.com/twilight-project/zkos-rust)                       | Zero-knowledge UTXO state manager and proof verifier.     |
| [Nyks Chain](https://github.com/twilight-project/nyks-chain)                     | Cosmos SDK-based chain for consensus and state anchoring. |

***

### Summary



The Twilight Developer Portal serves as your foundation for all technical integrations.

It connects you with the SDKs, APIs, and documentation needed to build custom trading interfaces, proof-based wallets, or cross-layer infrastructure within the Twilight ecosystem.

For deeper details on protocol operations, validator setup, or network participation, explore the Operator Docs and Security sections following this one.

### Support

For integration questions or feedback:

Join the community Discord (#dev-support)

Open a GitHub issue under twilight-project/testnets
