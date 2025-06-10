# Overwiew Updated

***

**ThunderLink** is a **drop-in solution** for integrating **RGB asset transfers**. It offers a **simple HTTP REST API**, designed for **developer-friendliness** and **ease of integration** with **near-zero configuration**.

ThunderLink eliminates the complexity of handling RGB asset transactions, making it ideal for developers, businesses, and platforms looking to accept RGB transfers seamlessly.

## ThunderLink: UI/UX and Functional Design Description

ThunderLink is a plug-and-play integration layer for RGB assets on Bitcoin L1, aiming to be the “Stripe for RGB payments.” It enables businesses (exchanges, online merchants, etc.) to easily accept and manage RGB-based tokens with an experience similar to familiar payment gateways (Stripe, PayPal, Coinbase Commerce, CoinPayments). The system is composed of three main components working together:

* ThunderLink RGB Manager (managed by ThunderLink) – provides the core RGB asset functionality and APIs (invoice generation, state tracking, PSBT handling).\

* ThunderLink RGB Server (Merchant Node) – run by the customer (exchange or merchant) on their own infrastructure, holding their private keys and responsible for signing Bitcoin transactions (via PSBT).\

* ThunderLink RGB Client SDK (Frontend) – a lightweight React library that integrates into websites or apps (web stores, exchange UIs) to present UI elements (e.g. “Pay with RGB” buttons, payment widgets) and communicate with the merchant’s Server.\


Target Users: The primary users are crypto exchanges (who can integrate ThunderLink for RGB token deposits & withdrawals) and RGB application developers or merchants who want to accept RGB asset payments in their apps or online stores. Secondary users include end-customers using an RGB-compatible wallet to make payments on these platforms.

This design description will detail the UI/UX flow for each component, illustrate the integration touchpoints, and breakdown functional operations like invoice creation, payment processing, balance inquiries, and PSBT signing. We also outline security boundaries between components and provide a glimpse of a future ThunderLink Dashboard for monitoring transactions and integrations.



### Architecture and Components

ThunderLink architecture separates concerns of asset logic, private key custody, and user interface for maximum security and ease of integration. Below is an overview of how the components interact in the system:

The **Client SDK** (used in the front-end) communicates with the **ThunderLink RGB Manager** _via the merchant’s Server_, which is operated by the customer. This server holds sensitive data, signs transactions, and acts as a secure intermediary. It interacts with the **ThunderLink RGB Manager** through API calls to coordinate RGB asset state and Bitcoin transactions using PSBTs (Partially Signed Bitcoin Transactions). The **Bitcoin Layer 1 network** serves as the settlement layer, while users pay invoices using an **RGB-compatible wallet**.

###

### ------------------------------------------------------------------------------------------

### ThunderLink: UI/UX and Functional Design Description

**ThunderLink** is a plug-and-play integration layer for RGB assets on Bitcoin L1, aiming to be the “Stripe for RGB payments.” It enables businesses (exchanges, online merchants, etc.) to easily accept and manage RGB-based tokens with an experience similar to familiar payment gateways (Stripe, PayPal, Coinbase Commerce, CoinPayments).

ThunderLink offers a simple HTTP REST API designed for ease of integration, developer-friendliness, and near-zero configuration. It removes the complexity of handling RGB asset transactions—such as blinded UTXOs, PSBTs, and state syncing—making it ideal for anyone looking to add RGB support to their applications.

***

### System Components

The ThunderLink system is composed of three main components working together:

#### 1. **ThunderLink RGB Manager** _(Managed by ThunderLink)_

This is the core backend service that provides RGB functionality via a public HTTP API. It handles:

* Invoice generation and tracking
* RGB asset state coordination
* UTXO and transaction management
* PSBT construction

> ⚠️ The Manager **does not store or access private keys**. Instead, it delegates signing responsibilities to a separate secure component: the **ThunderLink RGB Signer**.

#### 2. **ThunderLink RGB Signer** _(Run by the customer)_

This is a lightweight service that must be deployed by the merchant, exchange, or platform operator. It:

* Holds or accesses the customer’s Bitcoin private keys
* Signs PSBTs sent by the ThunderLink RGB Manager
* Communicates via **RabbitMQ**, listening for signing requests on a private channel

> 🔐 The RGB Signer **does not expose any internet-facing ports**. It runs entirely within the customer’s infrastructure and only interacts with the Manager through a secured message queue, ensuring sensitive keys remain isolated and protected.

#### 3. **ThunderLink RGB Client SDK** _(Frontend)_

A lightweight React library that can be embedded into merchant websites or applications. It provides:

* UI elements like “Pay with RGB” buttons, modals, and widgets
* Communication with the ThunderLink RGB Manager API
* Real-time invoice tracking and payment UX

***

### Target Users

* **Exchanges**: Enable RGB token deposits and withdrawals.
* **Merchants**: Accept RGB asset payments in online stores.
* **Developers**: Integrate RGB support into apps or platforms.
* **End-users**: Use RGB-compatible wallets to pay invoices seamlessly.

***

### Architecture and Component Interaction

The ThunderLink architecture cleanly separates the concerns of asset logic, key management, and UI interaction for better security and developer experience.

* The **Client SDK** (frontend) communicates directly with the **ThunderLink RGB Manager** via REST API.
* The **RGB Manager** handles asset logic but never holds private keys.
* When a transaction requires signing, the Manager sends a **signing request** to the **RGB Signer** using **RabbitMQ**.
* The **RGB Signer**, running securely in the customer’s environment, signs the PSBT and returns the result via the same message channel.
* The **Bitcoin L1 network** provides the settlement layer for finalizing transactions.
* **Users pay invoices using an RGB-compatible wallet** (e.g., mobile or desktop wallet that supports blinded UTXOs and RGB transfer formats).
