# Client SDK

The **Twilight Client SDK** provides a developer-friendly interface for interacting with Twilight’s network components — including the **Relayer API**, **ZkOS RPC layer**, and **Nyks chain**.\
It is written in **Rust**, with planned bindings for TypeScript and Python, allowing seamless integration with both backend services and frontend wallets.

This guide introduces what can be done using the SDK, with practical examples and context. For detailed, auto-generated Rust documentation and full function references, visit the canonical reference site:

👉 [**Twilight Client SDK Reference**](https://docs.twilight.rest/?rust#twilight-clinet-sdk)

## Overview

The SDK simplifies access to Twilight’s hybrid infrastructure by abstracting low-level JSON-RPC and REST interactions into high-level, strongly typed methods. Through a single client instance, developers can interact with:

* The **Relayer Service** — to access market data, manage trades, or integrate automated strategies.
* The Z**kOS Node** — to query UTXO states, perform shielded transactions, and access encrypted balances.
* The **Nyks Chain** — for validator and node status queries or service health checks.

This modular design allows flexible integration across on-chain, off-chain, and privacy-preserving environments.

## Installation

Add the SDK to your Rust project:

```toml
[dependencies]
twilight-client-sdk = { git = "https://github.com/twilight-project/twilight-client-sdk" }
```

Or build locally:

```bash
git clone https://github.com/twilight-project/twilight-client-sdk.git
cd twilight-client-sdk
cargo build --release
```

## Getting Started

### Initialize the Client

```rust
use twilight_client_sdk::Client;

#[tokio::main]
async fn main() {
    let client = Client::new("https://nykschain.twilight.rest");
    let status = client.status().await.unwrap();
    println!("Connected to Twilight node at height: {}", status.height);
}
```

You can also connect to local development nodes:

```rust
let client = Client::new("http://localhost:3030");
```

## Core Capabilities

{% stepper %}
{% step %}
### Account and UTXO Management

Query encrypted account state or retrieve UTXOs directly from the zkOS node.

```rust
let account = client.zkos().get_account_state("shielded_addr").await?;
println!("Commitments: {:?}", account.commitments);
```
{% endstep %}

{% step %}
### Building and Submitting Transactions

Construct and sign private transactions locally before submitting to the zkOS RPC endpoint.

```rust
let tx = client.zkos().build_transaction(inputs, outputs, fee)?;
let signed = client.wallet().sign_transaction(tx, keypair)?;
client.zkos().submit_transaction(signed).await?;
```
{% endstep %}

{% step %}
### Exchange Integration

Access live price feeds or place orders through the exchange interface.

```rust
let ticker = client.exchange().ticker("BTCUSDT").await?;
println!("BTC/USDT price: {}", ticker.price);
```
{% endstep %}

{% step %}
### Hybrid Workflows

Develop workflows that combine transparent and shielded operations — examples include:

* Fetching UTXOs → Processing proofs → Submitting private settlements.
* Automated bots performing both public and private order routing.
* Explorers indexing both on-chain and zkOS data layers.
{% endstep %}
{% endstepper %}

## Example: Private Transfer Flow

```rust
let sender = client.wallet().load("sender.key")?;
let recipient = "tw1qxyz...";

let utxos = client.zkos().get_utxos(&sender.address()).await?;
let tx = client.zkos().prepare_transfer(&sender, recipient, 0.5)?;
client.zkos().submit_transaction(tx).await?;
```

Flow Explanation:

{% stepper %}
{% step %}
The SDK fetches encrypted UTXOs for the sender.
{% endstep %}

{% step %}
A Bulletproof-verified transfer is constructed and signed locally.
{% endstep %}

{% step %}
The transaction is submitted to the zkOS RPC node.
{% endstep %}

{% step %}
Local state is updated once the transaction is confirmed.
{% endstep %}
{% endstepper %}

## Supported Modules

| Module     | Description                                                                     |
| ---------- | ------------------------------------------------------------------------------- |
| `exchange` | Wrapper for public/private market APIs and trading utilities.                   |
| `zkos`     | Manages UTXO queries, Bulletproof transactions, and encrypted state operations. |
| `wallet`   | Local key handling, signing, and address management utilities.                  |
| `nyks`     | Provides chain-level status, validator info, and telemetry endpoints.           |
| `utils`    | Helper functions for encoding, serialization, and verification.                 |

Each module can be imported and used independently, or through the unified `Client` abstraction.

## Error Handling

The SDK uses typed errors for predictable control flow:

```rust
match client.zkos().get_utxos("invalid_addr").await {
    Ok(data) => println!("{:?}", data),
    Err(e) => eprintln!("Error: {:?}", e),
}
```

Common error categories:

* `NetworkError` — RPC connectivity or timeout issues.
* `ParseError` — Invalid or unexpected response data.
* `ProofError` — Transaction rejected during Bulletproof validation.
* `Unauthorized` — Missing or invalid key context.



## Learn More

For the complete function reference, type definitions, and advanced integration examples, visit:\
👉 [**Twilight Client SDK Reference**](https://docs.twilight.rest/?rust#twilight-clinet-sdk)

_Last updated: November 2025_
