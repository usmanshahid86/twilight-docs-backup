# Privacy Architecture

Twilight is privacy-preserving by design. Traders interact through **shielded accounts** backed by **Bulletproofs**, ensuring that sensitive position data remains confidential while validators can still verify correctness.

***

### 1) Objectives

* **Hide sensitive details** (margin, leverage, position sizing) from public view
* **Keep settlement verifiable** by validators and LPs
* **Minimize developer burden** — proofs are handled by the SDK and relayer; users do not manage cryptography directly

***

### 2) zkOS at a Glance

**zkOS** is Twilight’s UTXO-style privacy layer:

* **UTXO-based state:** trading balances live in shielded UTXOs
* **Bulletproofs:** zero-knowledge range proofs ensure validity without revealing amounts
* **Off-chain proving, on-chain verification:** proofs are generated client-side / relayer-side and verified by validators during settlement
* **Separation of concerns:** Cosmos chain (Nyks) anchors public consensus; zkOS shields per-position details

> We do **not** use Ethereum SNARK verifiers here. References to SNARK contracts do not apply.

***

### 3) What’s Private vs. Public?

| Aspect                        | Visibility  | Notes                                                    |
| ----------------------------- | ----------- | -------------------------------------------------------- |
| Position margin, leverage     | **Private** | Hidden inside zkOS commitments                           |
| Per-position size & PnL path  | **Private** | Realized effects reflected only at settlement            |
| Aggregate pool stats (TVL, U) | **Public**  | Reported via public metrics                              |
| Execution price               | **Public**  | External oracle (Binance mid)                            |
| Settlement events             | **Public**  | Block-level confirmations; no per-trade internals leaked |

***

### 4) Private Trade Lifecycle

1. **Compose:** Client (or SDK) builds a shielded order referencing funding → trading account flows.
2. **Prove:** SDK/relayer generates Bulletproofs proving balance sufficiency and rule compliance.
3. **Execute:** Relayer prices against oracle and sequences the fill.
4. **Verify:** Validators check proofs and state transitions; settlement is recorded in the next block.
5. **Reflect:** Pool NAV and account balances update; internal per-position details remain encrypted.

```mermaid
flowchart L
    

```

***

***

### 5) Developer Experience

* No manual proof handling required — official SDKs abstract proving/verification calls
* Consistent RPCs: zkOS endpoints expose the shielded state needed by wallets, explorers, and indexers
* Rust-first tooling: the Client SDK provides typed builders for shielded transfers and orders

<br>

See also: _Developer Portal → Client SDK Guide_ and _API Suite → zkOS RPC_.

***

### 6) Threat Model Notes

* Integrity: Validators reject any settlement that fails proof verification
* Linkability: UTXO design reduces linkage between deposits, trades, and withdrawals
* Oracle surface: Price integrity is separate from privacy; multi-venue indices and bands are on the roadmap

***

Related:

* [_Pricing & Execution_](pricing-and-execution.md) — oracle-priced fills and sequencing
* [_PnL & Settlement_](pnl-and-settlement.md) — how shielded positions realize PnL at close
