# RGB Signer

**ThunderLink RGB Signer** is a lightweight backend service designed to securely sign PSBTs (Partially Signed Bitcoin Transactions) on behalf of the ThunderLink RGB Node. It is meant to be run in a customer-controlled environment and never exposes any external HTTP interfaces. All communication is handled over **RabbitMQ channels**, ensuring a secure and isolated signing process.

***

### Purpose

* Holds or accesses the merchant’s private keys
* Listens for signing requests from ThunderLink RGB Node
* Signs PSBTs and returns signed transactions through a secure message queue
* **Never exposes keys or services over the public internet**

***

### How It Works

1. The service establishes a secure RabbitMQ connection and listens to the queue `rpc.to-client`.
2. When a `sign` request is received, it:
   * Parses the PSBT from the message payload
   * Init RGB wallet use mnemonic, xpub-van, xpub-col
   * Signs the PSBT
   * Sends the signed result to `rpc.to-server` with the same transaction ID (`txId`)

***

### Communication Model

| Direction            | Queue           | Purpose                 |
| -------------------- | --------------- | ----------------------- |
| RGB Manager → Signer | `rpc.to-client` | Sends PSBTs for signing |
| Signer → RGB Manager | `rpc.to-server` | Returns signed PSBTs    |

***

### Method Handlers

Currently supported:

| Method | Description                                                                            |
| ------ | -------------------------------------------------------------------------------------- |
| `sign` | Signs a base64-encoded PSBT using the mnemonic-derived key and returns the signed PSBT |

> New methods can be added by extending the `methodHandlers` map.\
>

***

### 🔒 Security Notes

* **No public-facing ports**: All communication is internal via RabbitMQ.
* **Private keys are never transmitted**: Only unsigned PSBTs and signed responses are exchanged.
* **Signer is isolated**: Runs inside your infrastructure, fully under customer control.

### 📁 Repository

Git repo: \
&#x20;[ThunderLink RGB Signer on GitHub](https://github.com/RGB-OS/thunderlink-signer)
