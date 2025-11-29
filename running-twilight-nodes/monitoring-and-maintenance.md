# Monitoring & Maintenance

This section describes how to monitor, maintain, and secure your Twilight nodes.\
It covers logging, telemetry, updates, and security best practices to ensure reliable operation across validator and relayer environments.

***

## Overview

Effective monitoring helps node operators ensure uptime, track performance, and identify potential issues early.\
Maintenance involves regular updates, backups, and proactive security practices to keep your node resilient and synchronized with the Twilight testnet.

***

## Log Management

Logs provide real-time insight into node activity and potential issues.

### View Logs

```bash
docker logs -f validator

# or
docker logs -f relayer
```

### Check Running Containers

```bash
docker ps
```

### Common Log Checks

* Look for `"catching_up": false` to confirm synchronization.
* Watch for connection errors to peers or RPC endpoints.
* Use `grep` to filter logs for keywords such as `error` or `panic`.

```bash
docker logs validator | grep error
```

***

## Telemetry & Prometheus

To monitor node metrics (block time, peers, memory, etc.), enable Prometheus telemetry in your configuration files.

### Enable Telemetry

File: `/root/.nyks/config/app.toml`

```toml
[telemetry]
service-name = ""
enabled = true
enable-hostname = true
enable-hostname-label = true
enable-service-label = true
prometheus-retention-time = 5000
global-labels = []
```

File: `/root/.nyks/config/config.toml`

```toml
[instrumentation]
prometheus = true
prometheus_listen_addr = ":26660"
max_open_connections = 3
namespace = "tendermint"
```

Once enabled, Prometheus metrics will be available on **port 26660**.

***

## Grafana Integration

Grafana allows visualization of Prometheus metrics in real time.

### Sample Prometheus Job Configuration

```yaml
- job_name: 'twilight'
  static_configs:
    - targets: ['your-node-ip:26660']
```

After setup, dashboards can display node uptime, peer count, block latency, and memory usage.

***

## Node Updates & Maintenance

Regular updates are essential for security and performance improvements.\
Always back up your `.env` and volume data before updating.

### Update Process

```bash
git pull
docker-compose down
docker-compose build
docker-compose up -d
```

This sequence safely rebuilds your containers while preserving blockchain data.

***

## Security Best Practices

{% hint style="warning" %}
At present, the network is under active development — do **not** deposit large amounts of BTC into the testnet.
{% endhint %}

As a node operator, it is your responsibility to protect your node from unauthorized access, loss, and theft.

{% stepper %}
{% step %}
### Device Security

Physical Access\
Restrict physical access to the device running your node. For rented or virtual servers, verify the provider’s security policies and disable unnecessary administration panels.

Personal Devices\
Secure all devices holding wallets, SSH keys, or authentication tokens. Include them in your threat model.
{% endstep %}

{% step %}
### Platform Security

Operating System Maintenance\
Regularly update your OS and third-party software (e.g., OpenSSH).

Firewall Configuration\
Allow only necessary ports — for example, `26657` for RPC and `26656` for P2P communication.

```bash
sudo ufw allow 26656,26657/tcp
sudo ufw enable
```

Network Security\
Restrict access using trusted IPs or VPN. Prefer key-based SSH authentication over passwords.
{% endstep %}

{% step %}
### Software Security

Installation Verification\
Verify binaries and source code using PGP and `git verify-tag`. Ensure dependencies like Go are from trusted sources.

Regular Updates\
Track updates in official GitHub repositories and pull regularly to stay in sync with testnet versions.
{% endstep %}

{% step %}
### Wallet Security

Twilight nodes use Cosmos SDK keyring wallets in Validator/Judge mode and BTC wallets when operating as Signers.

Node Wallet\
Use the `file` or `pass` backend for secure key storage. Avoid the `test` backend — it stores unencrypted keys and is meant only for development.

Seed Phrase Protection\
Write your 24-word mnemonic on paper and store it securely (fireproof safe or encrypted storage). Do **not** store it in plaintext or online.

Passphrase Management\
Encrypt your wallet with a passphrase but store it offline or in a password manager. If lost, there is no recovery option. Never use the same passphrase across multiple nodes.

Backup\
Encrypt private key backups and store them offline (USB drives, encrypted disks). Test your backups periodically to ensure recoverability.
{% endstep %}
{% endstepper %}

***

## Backup & Recovery

### Backup Important Data

* `.env` file
* Database volumes
* Wallet keys and mnemonics

### Create Encrypted Backups

```bash
tar -czf twilight_backup.tar.gz .env data/
gpg -c twilight_backup.tar.gz
```

Store backups on secure offline devices.

### Recovery Process

To restore from backup:

```bash
gpg -d twilight_backup.tar.gz.gpg | tar -xz
docker-compose up -d
```

***

## Troubleshooting

### Node Not Starting

```bash
sudo systemctl status docker
docker logs -f validator
```

### Sync or Peer Issues

Verify that required ports are open and reachable.

### Reset Node State

```bash
docker-compose down -v
docker-compose up -d
```

***

## Related Sections

* [Configuration](/broken/pages/4fef0da82fd6bb3e89b6fe3c6fd40ac1380cc5c9)
* [Validator Setup](/broken/pages/61c34b59baba64c1a72920559950ce134b35845d)
* [Relayer Setup](/broken/pages/e3face2be48e5624a2573f01a30a954371cc21d6)

***

By maintaining your node with these practices, you help keep the Twilight network stable, secure, and transparent.
