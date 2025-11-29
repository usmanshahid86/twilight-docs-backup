---
description: This guide explains how to set up and run Twilight nodes for the testnet.
---

# Node Setup

It covers how to build and deploy validator and relayer nodes using the official Docker Compose environments.

***

### 1. Overview

The Twilight testnet allows node operators to participate in validating blocks, relaying off-chain transactions, and supporting the privacy and data layers of the network.

Each node type is provided as a self-contained Docker Compose environment to simplify setup and ensure all dependencies are properly configured.

| **Node Type**  | **Directory**     | **Description**                                                   |
| -------------- | ----------------- | ----------------------------------------------------------------- |
| Validator Node | validator-docker/ | Runs the consensus engine, validator services, and zkOS modules.  |
| Relayer Node   | relayer-docker/   | Runs the off-chain matching engine and exchange relayer services. |

***

### 2. Docker Compose Environment (Testnet)

For the **Twilight Testnet**, all required components are bundled into a single Docker Compose environment.\
This setup provides an easy way to run validator or relayer nodes without managing multiple services manually.

Unlike precompiled images, operators build all containers locally from the provided repositories, ensuring compatibility with their host architecture.

You can find the official testnet setups here:\
👉 [https://github.com/twilight-project/testnets](https://github.com/twilight-project/testnets)

All node services — **Twilight Core (Nyks)**, **zkOS**, and their dependencies — are bundled within these Docker environments for simplified deployment and network consistency.

***

### 3. Component Overview

#### What’s Included

* **Nyks** — Cosmos SDK-based full node and validator service
* **zkOS** — Zero-knowledge account state module
* **PostgreSQL** — Persistent chain data and indexing storage
* **Faucet** — Testnet token distribution
* **Indexer** — Tracks usage metrics and chain statistics
* **Verification Server** — KYC and identity verification layer

#### What’s Not Included

* **BTC Oracle**
* **BTC Forkscanner**\
  These are excluded in the testnet setup and not required for validator or relayer operation.

***

### 4. Setup Instructions

#### Clone the Repository

```bash
git clone https://github.com/twilight-project/testnets.git
cd testnets/zkpass
```

#### Select Your Node Type

Navigate to either the validator or relayer setup:

```bash
cd validator-docker
# or
cd relayer-docker
```

#### Configure Environment

Copy and edit the example environment file:

```bash
cp .env.example .env
nano .env
```

#### Build and Start Containers

```bash
docker-compose build
docker-compose up -d
```

Once the setup is complete, check container status:

```bash
docker ps
```

***

### 5. Local Development Alternative

If you prefer to work directly with the source code, each core component can also be built and run independently from its respective GitHub repository:

* [twilight-project/nyks](https://github.com/twilight-project/nyks)
* [twilight-project/zkos-rust](https://github.com/twilight-project/zkos-rust)
* [twilight-project/relayer-core](https://github.com/twilight-project/relayer-core)

This approach is recommended for developers contributing to protocol code or integrating custom build pipelines.

***

### 6. Next Steps

Once your base node environment is ready, proceed with the detailed setup guides:

* Validator Setup\
  Learn how to configure and monitor a full validator node with Nyks and zkOS.
* Relayer Setup\
  Set up the off-chain matching engine and link your relayer to the Twilight testnet.
