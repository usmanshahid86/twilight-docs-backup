# Relayer Setup

This guide provides step-by-step instructions for setting up a **Twilight Relayer Node** using the official Docker Compose environment.\
The relayer serves as the off-chain matching and execution engine for the Twilight exchange layer, linking user activity and market logic to the on-chain consensus network.

## Overview

A **Relayer Node** coordinates trade execution, market matching, and order management off-chain while maintaining secure synchronization with the Nyks validator network.\
It is essential for handling exchange operations such as trade matching, price discovery, and settlement finalization.

In the testnet, the relayer environment is designed for rapid deployment using Docker Compose. It includes all necessary dependencies and services for running a complete, local relayer stack.

## Components

The relayer Docker setup includes the following services:

| Component                | Description                                                                   |
| ------------------------ | ----------------------------------------------------------------------------- |
| **Relayer Core**         | Core matching engine and transaction handler for the exchange.                |
| **Relayer API**          | RESTful interface for client requests and external integrations.              |
| **PostgreSQL (Storage)** | Database container to persist exchange data, orders, and trade states.        |
| **Kafka**                | Messaging backbone for asynchronous communication between relayer modules.    |
| **Zookeeper**            | Required by Kafka for managing distributed coordination and message tracking. |
| **Redis**                | In-memory data store for caching, queues, and session handling.               |

## Architecture

The relayer node runs as a multi-service containerized environment.\
All dependencies (Kafka, Redis, PostgreSQL, etc.) are orchestrated through Docker Compose and interconnected on an internal network.

A single command deployment launches the entire stack. The relayer communicates with the validator node using RPC and gRPC APIs to broadcast validated transactions and order updates.

## Prerequisites

{% hint style="info" %}
Before installing, ensure the following system configuration:

* Operating System: Ubuntu 20.04 or later (Linux recommended)
* RAM: Minimum 8 GB (16 GB recommended)
* Storage: 100 GB free disk space
* Docker Engine: 24.0+
* Docker Compose: v2.0+
* Validator Node: Must be running and reachable (see [Validator Setup](/broken/pages/f1658dae07bb6fa3f505f5616ddaae126b771db3))
{% endhint %}

### Install Dependencies

{% code title="Install dependencies" %}
```bash
sudo apt update && sudo apt install -y git docker.io docker-compose
sudo systemctl enable --now docker
```
{% endcode %}

## Cloning the Repository

{% code title="Clone repository" %}
```bash
git clone https://github.com/twilight-project/testnets.git
cd testnets/open-testnet-2/relayer-docker
```
{% endcode %}

The `relayer-docker` directory includes all necessary Docker Compose files, environment templates, and dependency configurations.

## Running the Relayer Node

{% stepper %}
{% step %}
### Confirm Validator Node Connectivity

Ensure your validator node is active and running.\
The relayer will attempt to connect to its RPC endpoint during initialization.
{% endstep %}

{% step %}
### Build and Launch the Relayer Stack

{% code title="Start relayer stack" %}
```bash
docker compose up
```
{% endcode %}

This command will:

* Pull all necessary dependencies
* Build local Docker images
* Initialize PostgreSQL, Kafka, and Redis containers
* Start relayer-core and relayer-api services

Once all services are running, the relayer will automatically start indexing and begin processing trade simulation data for the testnet environment.
{% endstep %}
{% endstepper %}

## Verifying the Relayer

To confirm that the relayer has started successfully:

{% code title="Verify containers and logs" %}
```bash
docker ps
docker logs -f relayer-core
```
{% endcode %}

You should see initialization logs confirming connections to PostgreSQL, Kafka, and the validator RPC endpoint.

You can also verify the REST API endpoint:

{% code title="Check API status" %}
```bash
curl http://localhost:8080/api/status
```
{% endcode %}

Expected response:

{% code title="Expected API response (example)" %}
```json
{
  "status": "ok",
  "validator_connected": true,
  "uptime": "2m30s"
}
```
{% endcode %}

## Node Management

Use these commands to manage the environment:

{% code title="Management commands" %}
```bash
docker-compose down        # Stop
docker-compose restart     # Restart
docker-compose up -d       # Relaunch in detached mode
```
{% endcode %}

## Summary

* The relayer node forms the backbone of Twilight’s off-chain exchange operations.
* It depends on a running validator node to finalize transactions on-chain.
* Configuration settings (e.g., ports, credentials, and message queue tuning) are detailed in the **Configuration** section.
* Monitoring and telemetry setup for Kafka, Redis, and relayer services are covered in the **Monitoring & Maintenance** section.
