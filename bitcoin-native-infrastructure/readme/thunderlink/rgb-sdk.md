# RGB SDK

## SDK Overview

This is the underlying SDK used by RGB client applications. It provides a complete set of TypeScript/Node.js bindings for interacting with the **RGB Node** and managing RGB-based transfers.

> **RGB Node**: This SDK requires an RGB Node instance to function. For more information about RGB Node, including setup instructions, public endpoints and API documentation, see the [RGB Node repository](https://github.com/RGB-OS/RGB-Node/tree/public).

***

### 🧰 What You Can Do with This Library

With this SDK, developers can:

* Generate RGB invoices
* Create and manage UTXOs
* Sign PSBTs using local private keys or hardware signing flows
* Fetch asset balances, transfer status, and other RGB-related state

***

### ⚙️ Capabilities of `rgb-sdk` (via `WalletManager`)

| Method                                                                          | Description                                                                |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `generateKeys(network)`                                                         | Generate new wallet keypair with mnemonic/xpub/xpriv/master fingerprint    |
| `deriveKeysFromMnemonic(network, mnemonic)`                                     | Derive wallet keys (xpub/xpriv) from existing mnemonic                     |
| `deriveKeysFromSeed(network, seed)`                                             | Derive wallet keys (xpub/xpriv) directly from a BIP39 seed                 |
| `registerWallet()`                                                              | Register wallet with the RGB Node                                          |
| `getBtcBalance()`                                                               | Get on-chain BTC balance                                                   |
| `getAddress()`                                                                  | Get a derived deposit address                                              |
| `listUnspents()`                                                                | List unspent UTXOs                                                         |
| `listAssets()`                                                                  | List RGB assets held                                                       |
| `getAssetBalance(asset_id)`                                                     | Get balance for a specific asset                                           |
| `createUtxosBegin({ up_to, num, size, fee_rate })`                              | Start creating new UTXOs                                                   |
| `createUtxosEnd({ signed_psbt })`                                               | Finalize UTXO creation with a signed PSBT                                  |
| `blindReceive({ asset_id, amount })`                                            | Generate blinded UTXO for receiving                                        |
| `witnessReceive({ asset_id, amount })`                                          | Generate witness UTXO for receiving                                        |
| `issueAssetNia({...})`                                                          | Issue a new Non-Inflationary Asset                                         |
| `signPsbt(psbt, mnemonic?)`                                                     | Sign PSBT using mnemonic and BDK (async)                                   |
| `signMessage(message, options?)`                                                | Produce a Schnorr signature for an arbitrary message                       |
| `verifyMessage(message, signature, options?)`                                   | Verify Schnorr message signatures using wallet keys or provided public key |
| `refreshWallet()`                                                               | Sync and refresh wallet state                                              |
| `syncWallet()`                                                                  | Trigger wallet sync without additional refresh logic                       |
| `listTransactions()`                                                            | List BTC-level transactions                                                |
| `listTransfers(asset_id)`                                                       | List RGB transfer history for asset                                        |
| `failTransfers(...)`                                                            | Mark expired transfers as failed                                           |
| `sendBegin(...)`                                                                | Prepare a transfer (build unsigned PSBT)                                   |
| `sendEnd(...)`                                                                  | Submit signed PSBT to complete transfer                                    |
| `send(...)`                                                                     | Complete send operation: begin → sign → end                                |
| `createBackup(password)`                                                        | Create an encrypted wallet backup on the RGB node                          |
| `downloadBackup(backupId?)`                                                     | Download the generated backup binary                                       |
| `restoreFromBackup({ backup, password, ... })`                                  | Restore wallet state from a backup file                                    |
| `getSingleUseDepositAddress()`                                                  | Get a single-use deposit address for receiving assets                      |
| `getUnusedDepositAddresses()`                                                   | Get unused deposit addresses                                               |
| `getBalance()`                                                                  | Get wallet balance including BTC and asset balances                        |
| `settle()`                                                                      | Settle balances in the wallet                                              |
| `createLightningInvoice({ amount_sats, asset, ... })`                           | Create a Lightning invoice for receiving BTC or asset payments             |
| `getLightningReceiveRequest(id)`                                                | Get the status of a Lightning invoice by request ID                        |
| `getLightningSendRequest(id)`                                                   | Get the status of a Lightning payment by request ID                        |
| `getLightningSendFeeEstimate({ invoice, asset? })`                              | Estimate the routing fee required to pay a Lightning invoice               |
| `payLightningInvoiceBegin({ invoice, max_fee_sats })`                           | Begin a Lightning invoice payment process                                  |
| `payLightningInvoiceEnd({ signed_psbt })`                                       | Complete a Lightning invoice payment using signed PSBT                     |
| `payLightningInvoice({ invoice, max_fee_sats }, mnemonic?)`                     | Pay a Lightning invoice (begin + sign + end)                               |
| `withdrawBegin({ address_or_rgbinvoice, amount_sats, fee_rate, asset? })`       | Begin a withdrawal process from UTEXO                                      |
| `withdrawEnd({ signed_psbt })`                                                  | Complete a withdrawal from UTEXO using signed PSBT                         |
| `withdraw({ address_or_rgbinvoice, amount_sats, fee_rate, asset? }, mnemonic?)` | Withdraw BTC or assets from UTEXO to Bitcoin L1 (begin + sign + end)       |
| `getWithdrawalStatus(withdrawal_id)`                                            | Get the status of a withdrawal by withdrawal ID                            |

***

### 🧩 Notes for Custom Integration

* All communication with the RGB Node is handled via HTTP API calls encapsulated in the `RGBClient` class.
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

### 📁 Repository&#x20;

JS

{% embed url="https://github.com/RGB-OS/rgb-sdk" %}

ReactNative

{% embed url="https://github.com/RGB-OS/rgb-sdk-rn" %}
