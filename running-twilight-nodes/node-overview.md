---
description: >-
  This section introduces the core node types and their roles within the
  Twilight network.
---

# Node Overview

Twilight nodes form the core infrastructure of the network, powering its decentralized exchange architecture and ensuring that every service remains verifiable, private, and reliable.

Together, these nodes sustain the flow of encrypted trading data, maintain consensus integrity, and enable secure coordination between the Nyks chain and the ZkOS privacy layer.

Operating a Twilight node means maintaining a critical link in a system designed to merge privacy, security, and Bitcoin liquidity into a unified, resilient network.

### Node Roles

***

### Validator Node

Validators are at the core of network security.

They participate in block production and consensus, signing and verifying transactions that form the canonical chain.

By staking and maintaining uptime, validators ensure trustless coordination across Twilight’s hybrid architecture — where chain state meets zkOS privacy layers.

***

### Full / Archive Node

Full nodes maintain the entire blockchain state, serving RPC endpoints for explorers, indexers, wallets, and analytics services.

Archive nodes go a step further — storing the complete historical record of all blocks and state transitions, providing the data integrity that researchers and auditors rely on.

***

### Relayer Node

Relayer nodes act as the bridge between on-chain consensus and off-chain execution.

They run the matching engine and exchange layer, handling order routing, data feeds, and transaction settlements.

In the Twilight architecture, relayers are essential for ensuring that encrypted trading and settlement operations remain verifiable and consistent with the chain’s state.

> For more details about the system architecture, see [Technical Architecture](../overview/architecture-overview.md).

***

### &#x20;Network Environments

| **Testnet**  | For development and experimentation | Tokens are test assets            |
| ------------ | ----------------------------------- | --------------------------------- |
| **Mainnet**  | Production environment              | Requires registration and staking |

Testnet participation helps operators familiarize themselves with the infrastructure, experiment safely, and prepare for production deployment once the Mainnet is active.

***

### Rewards & Participation

Validators earn rewards for producing blocks and maintaining network consensus.

Relayers and other operators supporting the exchange layer may also receive uptime-based or performance-based incentives during the testnet and later in the mainnet phase.

By running a node, operators directly contribute to Twilight’s mission — enabling spot-priced Bitcoin derivatives, encrypted market activity, and transparent decentralized coordination.

***

### Next Steps

Once you’ve reviewed the node roles and environments, you can proceed to:

* [Node Setup](node-setup/) — how to deploy validator or relayer nodes using Docker.
* [Configuration](configuration.md) — environment variables, ports, and connectivity.
* [Monitoring & Maintenance](monitoring-and-maintenance.md) — logs, metrics, and uptime management.
* [Troubleshooting](troubleshooting.md) — common issues and quick recovery steps.
