# Advanced Usage

### Build Your Own RGB Client Server

While the **ThunderLink RGB Client Server** offers a ready-to-deploy backend service with sensible defaults and a full feature set, **advanced users** may choose to build their own custom implementation to better fit their infrastructure, business logic, or deployment model.

To support this, ThunderLink provides the core library: [`rgb-connect-nodejs`](https://www.npmjs.com/package/rgb-connect-nodejs).

#### `rgb-connect-nodejs` Library

This is the underlying SDK used by the official RGB Client Server. It provides a complete set of TypeScript/Node.js bindings for interacting with the **ThunderLink RGB Manager** and managing RGB-based transfers.

With this library, developers can:

* Generate RGB invoices&#x20;
* Create and manage UTXOs
* Sign PSBTs using local private keys or hardware signing flows
* Fetch asset balances, transfer status, and other RGB-related state

#### When to Use a Custom Server

You might consider building your own RGB Client Server if:

* You need to integrate with an existing wallet infrastructure
* You want to expose a different API surface to your frontend SDK
* You are integrating deeply with other financial systems or order management logic
* You want to apply non-standard business rules or policy enforcement
* You’re running a large-scale exchange or payment platform and require full control over infrastructure, monitoring, and debugging

#### Capabilities of `rgb-connect-nodejs` (via WalletManager)

<table><thead><tr><th width="368.6484375">Method</th><th>Description</th></tr></thead><tbody><tr><td><code>generateKeys()</code></td><td>Generates a new wallet keypair and returns mnemonic, xpub</td></tr><tr><td><code>registerWallet()</code></td><td>Registers the wallet with the ThunderLink RGB Manager and syncs the current state.</td></tr><tr><td><code>getBtcBalance()</code></td><td>Returns the current on-chain BTC balance of the wallet.</td></tr><tr><td><code>getAddress()</code></td><td>Returns a new deposit address derived from the wallet’s xpub.</td></tr><tr><td><code>listUnspents()</code></td><td>Lists all unspent UTXOs associated with the wallet.</td></tr><tr><td><code>listAssets()</code></td><td>Lists all RGB assets currently held by the wallet.</td></tr><tr><td><code>getAssetBalance(assetId)</code></td><td>Retrieves the balance for a specific RGB asset.</td></tr><tr><td><code>createUtxosBegin(params)</code></td><td>Begins the process of creating new UTXOs (e.g., for future asset transfers).</td></tr><tr><td><code>createUtxosEnd({ signedPsbt })</code></td><td>Finalizes UTXO creation by submitting a signed PSBT.</td></tr><tr><td><code>blindRecive({ asset_id, amount })</code></td><td>Generates a blinded UTXO to receive a transfer of an RGB asset.</td></tr><tr><td><code>issueAssetNia({...})</code></td><td>Issues a new RGB asset of type NIA (Non-Inflationary Asset).</td></tr><tr><td><code>signPsbt({ psbtBase64, mnemonic })</code></td><td>Signs a PSBT using a mnemonic phrase-derived key (via BDK WASM).</td></tr><tr><td><code>refreshWallet()</code></td><td>Refreshes wallet state (balances, transfers, etc.) by re-syncing with ThunderLink Manager.</td></tr><tr><td><code>listTransactions()</code></td><td>Returns a list of BTC transactions related to the wallet.</td></tr><tr><td><code>listTransfers(asset_id)</code></td><td>Returns a list of RGB transfers (incoming/outgoing) for a specific asset.</td></tr><tr><td><code>failTransfers({ batch_transfer_idx })</code></td><td>Marks a transfer batch as failed—used for expired or invalid transfers.</td></tr><tr><td><code>sendBegin(params)</code></td><td>Initiates an RGB asset transfer by preparing a PSBT (unsigned).</td></tr><tr><td><code>sendEnd({ signed_psbt })</code></td><td>Finalizes the asset transfer by submitting the signed PSBT.</td></tr><tr><td><code>send(invoiceTransfer, mnemonic)</code></td><td>High-level utility: combines <code>sendBegin</code>, <code>signPsbt</code>, and <code>sendEnd</code> into a full transfer flow.</td></tr></tbody></table>

***

#### RGB Asset Transfer Flow

To send an RGB asset using a PSBT flow:

1. **Prepare Transfer (PSBT):**\
   Call `sendBegin(params)` with a `recipient_map`, fee rate, and optional donation flag. This returns an unsigned PSBT.
2. **Sign PSBT:**\
   Use `signPsbt({ psbtBase64, mnemonic })` to sign the PSBT using the wallet's private key derived from the mnemonic.
3. **Finalize Transfer:**\
   Submit the signed PSBT via `sendEnd({ signed_psbt })` to complete the transfer.

> 💡 The `send()` method automates this three-step flow, making it easier to programmatically send assets in one function call. This method is intended to be used **internally on the backend only**, after all transfer requirements (such as sufficient balance, recipient validation, and policy checks) have been verified. It should not be exposed to client-side logic.



#### Notes for Custom Integration

* All communication with the ThunderLink RGB Manager is handled via HTTP API calls encapsulated in the `ThunderLink` class.
* The `signPsbt` method demonstrates how to integrate a signing flow using `bdk-wasm`. This can be replaced with your own HSM or hardware wallet integration if needed.
* By using this SDK, developers have full control over transfer orchestration, UTXO selection, invoice lifecycle, and signing policies.
* This pattern enables advanced use cases, such as integrating with third-party identity/auth layers, batching logic, compliance tracking, or internal audit flows.
