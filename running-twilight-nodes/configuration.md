# Configuration

This section explains how to configure and customize **Twilight Nodes** (Validator and Relayer) after installation.\
Configuration ensures your node runs efficiently, remains connected to the testnet, and stores data correctly across restarts.

## Overview

Both the **Validator** and **Relayer** nodes rely on environment variables and configuration files defined in their respective Docker directories.\
Each setup includes a `.env.example` file that contains the basic parameters needed to connect, identify, and run your node.

All configuration values can be customized before deployment or updated later by modifying the environment files or Docker Compose parameters.

## Environment Variables (.env)

Each node type includes an example environment file that you can copy and edit.

{% stepper %}
{% step %}
### Copy and edit the environment file

```bash
cp .env.example .env
nano .env
```
{% endstep %}

{% step %}
### Example Configuration

```bash
MONIKER="your-node-name"
CHAIN_ID="twilight-zkpass"
KEY_NAME="your-key-name"
PEERS="peer-id@ip:26656"
```

Descriptions:

| Variable                  | Description                                                                                     |
| ------------------------- | ----------------------------------------------------------------------------------------------- |
| **MONIKER**               | A unique human-readable name for your node instance.                                            |
| **CHAIN\_ID**             | Identifier for the testnet chain. Always use the correct value (`twilight-zkpass` for testnet). |
| **KEY\_NAME**             | Local key or wallet name used for validator identification.                                     |
| **PEERS**                 | List of peer nodes your validator should connect to.                                            |
| **RPC\_PORT / P2P\_PORT** | Networking ports for node RPC and peer communication.                                           |

Ensure your node’s ports are open to allow incoming and outgoing connections.

```bash
sudo ufw allow 26656,26657/tcp
sudo ufw enable
```
{% endstep %}
{% endstepper %}

## Processor Architecture Selection

The validator Docker setup supports multiple architectures. You must ensure the correct binary is extracted during build time.

In the `nyks/Dockerfile`, adjust line 47 according to your hardware:

| Environment            | Command                                |
| ---------------------- | -------------------------------------- |
| Linux (Apple M-series) | `RUN tar -xf nyks_linux_arm64.tar.gz`  |
| Linux (Intel/AMD)      | `RUN tar -xf nyks_linux_amd64.tar.gz`  |
| macOS (Apple M-series) | `RUN tar -xf nyks_darwin_arm64.tar.gz` |

Incorrect architecture selection will cause build errors or unexpected behavior.

## Storage and Data Persistence

Docker uses mounted volumes to persist blockchain and database data between container restarts. This ensures that chain history, account balances, and state information are retained.

Persistent directories:

```bash
/nyks/data/
/psql/data/
```

To completely reset your node and remove old state data:

```bash
docker-compose down -v
docker-compose up -d
```

{% hint style="warning" %}
This action permanently deletes all local blockchain data.
{% endhint %}

## Accessing Containers via SSH

You may need to inspect or modify configurations directly inside the Docker containers.

List running containers:

```bash
docker ps
```

Access a specific container:

```bash
docker exec -it <container_id> /bin/bash
```

Inside the container, key configuration files are located at:

```
/root/.nyks/config/config.toml
/root/.nyks/config/app.toml
/root/.nyks/config/genesis.json
```

You can use `nano` or `vi` to edit these files. Restart the node after any configuration changes:

```bash
docker-compose restart
```

## Configuration Sync and Updates

Use the following sequence to update the node with the latest configurations or Docker images without losing data:

{% stepper %}
{% step %}
### Fetch latest code

```bash
git pull
```
{% endstep %}

{% step %}
### Rebuild and restart

```bash
docker-compose down
docker-compose build
docker-compose up -d
```

This fetches the latest codebase and rebuilds images while preserving your existing blockchain data and configurations.
{% endstep %}
{% endstepper %}

## Troubleshooting Configuration Issues

<details>

<summary>Common causes if your node fails to start or connect to peers</summary>

* Verify `.env` syntax (no missing quotes or equal signs).
* Ensure ports `26656` and `26657` are open.
* Confirm the correct architecture in the Dockerfile.
* Rebuild the containers using `docker-compose build`.

</details>

## Related Sections

* [Validator Setup](/broken/pages/bd63c33bb9f1370db9477114de105071b139f4d5)
* [Relayer Setup](/broken/pages/658309daf4aa4665c1b229830fecf5bac1a7071c)
* [Monitoring & Maintenance](/broken/pages/d4b7359156ef7eec81f029535650da5cd3fb982b)

Tip: For advanced operators, consider exporting Prometheus metrics and Grafana dashboards — covered in the Monitoring & Maintenance section.
