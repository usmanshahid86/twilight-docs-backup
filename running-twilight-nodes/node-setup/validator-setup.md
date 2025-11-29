# Validator Setup

This guide describes how to deploy and operate a **Twilight Validator Node** using the official Docker Compose environment.\
The validator node is a core component of the Twilight testnet and performs consensus, state validation, and zero-knowledge service coordination through the Nyks and ZkOS modules.

## Overview

The **Validator Node** runs the on-chain consensus layer for the Twilight network.\
It participates in block production, validates transactions, and exposes RPC endpoints for other services and clients.

In the Testnet configuration, the validator node also includes essential support services such as the faucet, indexer, and ZkOS module.<br>

This allows developers and operators to run a full-featured, self-contained environment without external dependencies.

## Components

The validator Docker environment includes the following services:

| Component                | Description                                                                   |
| ------------------------ | ----------------------------------------------------------------------------- |
| **Nyks**                 | Cosmos SDK–based blockchain node handling consensus and block production.     |
| **zkOS**                 | Zero-knowledge operations server managing proof validation and private state. |
| **PostgreSQL (Storage)** | Database container for persistent chain and zkOS data.                        |
| **Faucet**               | Provides testnet Nyks and BTC-on-Nyks tokens for validators and developers.   |
| **Indexer**              | Collects on-chain and transaction data for analytics and block explorers.     |
| **Verification Server**  | Handles identity verification (used with zkPass or Self systems).             |

{% hint style="warning" %}
Not included in this Docker setup (not required in the testnet environment):

* BTC Oracle
* BTC Forkscanner
{% endhint %}

## Architecture

The validator environment bundles all required containers under a single Docker Compose file.<br>

A detailed architecture diagram for the Validator Node is available here:\
👉 https://github.com/twilight-project/testnets/blob/zkpass/open-testnet-2/validator-docker/architecture-open-testnet-2.jpg

Each container communicates over an internal Docker network, exposing only the necessary ports for RPC, P2P, and Prometheus.

## Prerequisites

* Operating System: Ubuntu 20.04 or later (Linux recommended)
* RAM: Minimum 8 GB (16 GB recommended)
* Storage: 100 GB free disk space
* Docker Engine: 24.0+
* Docker Compose: v2.0+
* Network: Stable connection with open ports `26656` and `26657`

## Quick setup

{% stepper %}
{% step %}
### Install dependencies

Run the following to install required packages and enable Docker:

{% code title="Install dependencies" %}
```bash
sudo apt update && sudo apt install -y git docker.io docker-compose
sudo systemctl enable --now docker
```
{% endcode %}
{% endstep %}

{% step %}
### Clone repository

Fetch the testnet repository and move into the validator compose directory:

{% code title="Clone repository" %}
```bash
git clone https://github.com/twilight-project/testnets.git
cd testnets/open-testnet-2/validator-docker
```
{% endcode %}

The `validator-docker` directory contains Docker Compose files, environment variables, and configuration templates.
{% endstep %}

{% step %}
### Build and run the validator environment

Start the Docker Compose environment:

{% code title="Start Docker Compose" %}
```bash
docker compose up
```
{% endcode %}

This will:

* Pull required images and build local Docker images.
* Initialize the Nyks blockchain and zkOS services.
* Launch all components defined in the compose file.

Once setup completes, the validator node will begin block synchronization and participate in consensus.
{% endstep %}
{% endstepper %}

## Validator address retrieval

Inspect validator addresses and key details from inside the `nyks` container:

{% code title="Enter nyks container" %}
```bash
docker exec -it <nyks_container_id> /bin/bash
cd /testnet/nyks/release
```
{% endcode %}

View keys and addresses:

{% code title="View validator & account addresses" %}
```bash
# View validator address (consensus/bech32 val prefix)
nyksd keys show validator-self --bech val --keyring-backend test

# View canonical account address
nyksd keys show validator-self --keyring-backend test
```
{% endcode %}

## Node verification

Check that your validator node is active and connected:

{% code title="Node status (RPC)" %}
```bash
curl http://localhost:26657/status
```
{% endcode %}

The status output includes:

* Node ID
* Peer count
* Synchronization status
* Current block height

If the node shows `"catching_up": false`, it is synchronized.

## Summary

* The validator environment is a full-featured testnet setup combining Nyks, zkOS, and supporting services.
* It is self-contained, Docker-based, and persistent between restarts.
* Configuration and telemetry settings are described in detail in the **Configuration** and **Monitoring & Maintenance** sections.
* Validators form the backbone of the Twilight network by securing consensus and validating zkOS-linked transactions.
