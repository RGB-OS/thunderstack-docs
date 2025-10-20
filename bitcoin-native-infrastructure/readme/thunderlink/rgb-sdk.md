# RGB SDK

## SDK Overview

This is the underlying SDK used by RGB client applications. It provides a complete set of TypeScript/Node.js bindings for interacting with the **RGB Node** and managing RGB-based transfers.

> **Note**: This SDK requires an RGB Node instance to function. The RGB Node is a drop-in HTTP service that handles RGB state management, invoice creation, PSBT building, and transfer lifecycle. For more information about setting up and running an RGB Node, see the [RGB-Node repository](https://github.com/RGB-OS/RGB-Node).

***

### 🧰 What You Can Do with This Library

With this SDK, developers can:

* Generate RGB invoices
* Create and manage UTXOs
* Sign PSBTs using local private keys or hardware signing flows
* Fetch asset balances, transfer status, and other RGB-related state

***

### ⚙️ Capabilities of `rgb-connect-nodejs` (via `WalletManager`)

| Method                               | Description                                                                                                   |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `generateKeys()`                     | Generate wallet keypair with mnemonic/xpub/xprv                                                               |
| `registerWallet()`                   | Register wallet with the RGB Node                                                                             |
| `getBtcBalance()`                    | Get on-chain BTC balance                                                                                      |
| `getAddress()`                       | Get a derived deposit address                                                                                 |
| `listUnspents()`                     | List unspent UTXOs                                                                                            |
| `listAssets()`                       | List RGB assets held                                                                                          |
| `getAssetBalance(assetId)`           | Get balance for a specific asset                                                                              |
| `createUtxosBegin(params)`           | Start creating new UTXOs                                                                                      |
| `createUtxosEnd({ signedPsbt })`     | Finalize UTXO creation with a signed PSBT                                                                     |
| `blindRecive({ asset_id, amount })`  | Generate blinded UTXO for receiving                                                                           |
| `issueAssetNia({...})`               | Issue a new Non-Inflationary Asset                                                                            |
| `signPsbt({ psbtBase64, mnemonic })` | Sign PSBT using mnemonic and BDK                                                                              |
| `refreshWallet()`                    | Sync and refresh wallet state                                                                                 |
| `listTransactions()`                 | List BTC-level transactions                                                                                   |
| `listTransfers(assetId)`             | List RGB transfer history for asset                                                                           |
| `failTransfers(...)`                 | Mark expired transfers as failed                                                                              |
| `sendBegin(...)`                     | Prepare a transfer (build unsigned PSBT)                                                                      |
| `sendEnd(...)`                       | Submit signed PSBT to complete transfer                                                                       |
| `send(...)`                          | Internal backend method that automates `sendBegin` + `signPsbt` + `sendEnd` (should not be exposed to client) |

***

### 🧩 Notes for Custom Integration

* All communication with the RGB Node is handled via HTTP API calls encapsulated in the `ThunderLink` class.
* The `signPsbt` method demonstrates how to integrate a signing flow using `bdk-wasm`. This can be replaced with your own HSM or hardware wallet integration if needed.
* By using this SDK, developers have full control over:
  * Transfer orchestration
  * UTXO selection
  * Invoice lifecycle
  * Signing policy

This pattern enables advanced use cases, such as:

* Integrating with third-party identity/auth layers
* Applying custom fee logic or batching
* Implementing compliance and audit tracking

***

### 📁 Repository

Git repo: \
&#x20;[RGB SDK on GitHub](https://github.com/RGB-OS/rgb-connect-nodejs)
