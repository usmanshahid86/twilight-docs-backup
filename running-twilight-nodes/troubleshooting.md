# Troubleshooting

This section provides common solutions to issues that may occur while setting up or running **Twilight Validator** or **Relayer Nodes**.\
Before making changes, always check logs and verify configuration settings.

Overview

Twilight nodes rely on multiple interlinked services (Docker, Nyks, zkOS, PostgreSQL, Redis, Kafka).\
Issues can arise from configuration errors, missing dependencies, or network problems.\
This guide lists the most frequent problems and their fixes.

{% stepper %}
{% step %}
### Node Startup Issues

**Problem:** Node fails to start or terminates immediately.\
**Possible causes and fixes:**

* Docker not running:

{% code title="commands" %}
```bash
sudo systemctl status docker
sudo systemctl enable --now docker
```
{% endcode %}

* Missing dependencies: Ensure `docker` and `docker-compose` are installed and at the correct versions.
* Incorrect architecture: Confirm you’re using the correct binary line in `nyks/Dockerfile` (`arm64`, `amd64`, etc.).
* .env syntax error: Check for missing quotes or equal signs in `.env`.
{% endstep %}

{% step %}
### Network & Synchronization Problems

**Problem:** Node stays in “catching\_up” state or cannot connect to peers.

**Possible causes:**

* Peers not reachable
* Firewall blocking required ports
* Outdated genesis or chain configuration

**Solutions:**

{% code title="commands" %}
```bash
curl http://localhost:26657/status
sudo ufw allow 26656,26657/tcp
git pull && docker-compose restart
```
{% endcode %}

If peer count remains zero, ensure your `.env` `PEERS` variable is properly set and validator addresses are valid.
{% endstep %}

{% step %}
### Relayer Connectivity Issues

**Problem:** Relayer cannot reach the validator RPC or REST API.

**Solutions:**

* Confirm the validator node is running and RPC is active:

{% code title="check-rpc" %}
```bash
curl http://localhost:26657/status
```
{% endcode %}

* Verify relayer logs:

{% code title="relayer-logs" %}
```bash
docker logs -f relayer-core
```
{% endcode %}

* Check that Docker containers for **Kafka**, **Redis**, and **PostgreSQL** are running:

{% code title="docker-ps" %}
```bash
docker ps
```
{% endcode %}

If Kafka or Zookeeper repeatedly restart, increase available memory or check your `docker-compose.yml` for port conflicts.
{% endstep %}

{% step %}
### Storage & Data Problems

**Problem:** Node or database fails to initialize due to corrupted data.\
**Solution:** Safely reset and rebuild node state:

{% code title="reset" %}
```bash
docker-compose down -v
docker-compose up -d
```
{% endcode %}

{% hint style="warning" %}
This deletes local blockchain and database data. Back up your `.env` and wallet keys before performing this step.
{% endhint %}
{% endstep %}

{% step %}
### Log Diagnostics

Logs are your most powerful troubleshooting tool.

#### Common Commands

{% code title="common-log-commands" %}
```bash
docker logs -f validator
docker logs -f relayer
docker logs validator | grep error
```
{% endcode %}

To inspect configuration paths inside a container:

{% code title="inspect-container" %}
```bash
docker exec -it <container_id> /bin/bash
cd /root/.nyks/config
```
{% endcode %}
{% endstep %}

{% step %}
### Port Conflicts

If Docker reports an error like “port already in use,” identify the conflicting process:

{% code title="lsof-ports" %}
```bash
sudo lsof -i :26656
sudo lsof -i :26657
```
{% endcode %}

Stop the process or change the port in your `.env` file before restarting.
{% endstep %}

{% step %}
### Quick Recovery Commands

When in doubt, these commands resolve most runtime issues:

{% code title="quick-recovery" %}
```bash
git pull
docker-compose down
docker-compose build
docker-compose up -d
```
{% endcode %}

To completely reset the environment:

{% code title="full-reset" %}
```bash
docker-compose down -v
```
{% endcode %}
{% endstep %}

{% step %}
### Additional Tips

* Verify Docker Compose file indentation — YAML errors can cause silent startup failures.
* If metrics are unavailable, ensure telemetry ports (e.g., 26660) are exposed.
* Use `docker stats` to monitor container resource usage.
* Keep system time synchronized:

{% code title="ntp" %}
```bash
sudo timedatectl set-ntp true
```
{% endcode %}
{% endstep %}
{% endstepper %}

Related Sections

* [Configuration](/broken/pages/d46ee3f95009645f45309daec7ebbb362040c6d1)
* [Monitoring & Maintenance](/broken/pages/8b87ed832f93cfd6317e23c6ee97b8e8a4b6e613)
* [Validator Setup](/broken/pages/e2684b91f0a9491f6cffb6ef46b511e8a479e785)
* [Relayer Setup](/broken/pages/b0fbf7ba159a41a75a0588894f27ffaedf255b4a)

{% hint style="info" %}
If issues persist, open a GitHub issue under [`twilight-project/testnets`](https://github.com/twilight-project/testnets) with your log excerpts and system details.
{% endhint %}
