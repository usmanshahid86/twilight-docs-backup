# ZkOS RPC API

ZkOS is Twilight’s UTXO-based privacy layer. This API exposes encrypted account state, commitments, and UTXO outputs tracked by a Twilight node. It’s built for indexers, explorers, front-ends, and infrastructure that need to read shielded state and (where enabled) submit transactions carrying Bulletproofs-based proofs.

***

### Endpoints

* Testnet (Nyks chain): https://nykschain.twilight.rest/zkos/
* Local development: http://localhost:3030/
* Protocol: JSON over HTTP(S), JSON-RPC 2.0

***

### What you can do

* Retrieve encrypted account state for frontend wallets (balances, commitments, recent outputs).
* Query UTXO sets by owner/address, type, or ranges.
* Get specific outputs (coin, memo, state) by UTXO key.
* Index UTXO data for explorers and analytics.
* Monitor UTXO activity (spent/unspent transitions) to reduce double-spend risk.
* Submit transactions with Bulletproofs (where node supports tx submission).

***

### JSON-RPC Method Families

ZkOS RPC exposes several categories of methods, each designed for a different aspect of UTXO management and proof validation.

* `Transaction endpoints` — Submit hex-encoded transactions to zkOS nodes for inclusion and validation.
* `UTXO queries` — Retrieve encrypted outputs for specific shielded addresses (balances, notes, state).
* `Output lookups` — Fetch details of a specific UTXO by its key (coin, memo, or state outputs).
* `Global scans`— Iterate over the full UTXO set for explorers, analytics, or monitoring dashboards.

Each category follows the JSON-RPC 2.0 format with parameters defined in the detailed API reference.

For the authoritative list of endpoint names, parameters, and full response schemas, see:

👉 [zkOS RPC Detailed Specification](https://docs.twilight.rest/_zkos)

***

### Shape-Only Examples

The following examples illustrate typical interaction shapes with ZkOS RPC.

They are not complete specifications — field names and response structures may vary depending on node version and configuration.

Use the [detailed API reference](https://docs.twilight.rest/_zkos) for the authoritative schema.

1\. Submit a Transaction

Submit a hex-encoded, Bulletproofs-verified transaction for inclusion.

```json
{
  "jsonrpc": "2.0",
  "method": "txCommit",
  "params": ["<hex_encoded_transaction>"],
  "id": 1
}
```

***

#### 2. Query UTXOs by Address

Fetch encrypted UTXOs belonging to a shielded address.

```json5
{ "jsonrpc": "2.0", 
  "method": "getUtxos", 
  "params": ["<hex_encoded_zkos_address>"], 
  "id": 1 
}
```

Example response (representative):

```json
{
  "height": 123456,
  "outputs": [
    {"commitment":"...", "amount_enc":"...", "type":"note", "spent":false}
  ]
}
```

***

3\. Lookup a Specific Output

Retrieve a single UTXO output (coin, memo, or state) by its key.

```json
{
  "jsonrpc": "2.0",
  "method": "getOutput",
  "params": ["<hex_encoded_utxo_key>"],
  "id": 1
}

```

***

#### 4. Global Scan for Indexers

Iterate over all UTXOs of a given type for analytics or explorers.

```json
{
  "jsonrpc": "2.0",
  "method": "allCoinUtxos",
  "params": [],
  "id": 1
}
```

> For pagination, range filters, and response schemas, see the spec.

***

### Notes for implementers

* Some fields are encrypted; clients/wallets must decrypt locally.
* Expect pagination (cursor or offset/limit) in global scans and address queries.
* Transactions are Bulletproofs-validated and must obey UTXO spend rules.

***

### Full specification

For endpoint tables, parameters, and full JSON schemas, see:

[https://docs.twilight.rest/\_zkos](https://docs.twilight.rest/?rust#zkos-rpc-api)
