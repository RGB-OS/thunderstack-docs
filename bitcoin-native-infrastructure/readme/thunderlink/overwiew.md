---
icon: book-open
---

# Overwiew

**ThunderLink** is a **drop-in solution** for integrating **RGB asset transfers**. It offers a **simple HTTP REST API**, designed for **developer-friendliness** and **ease of integration** with **near-zero configuration**.

ThunderLink eliminates the complexity of handling RGB asset transactions, making it ideal for developers, businesses, and platforms looking to accept RGB transfers seamlessly.

## ThunderLink: UI/UX and Functional Design Description

ThunderLink is a plug-and-play integration layer for RGB assets on Bitcoin L1, aiming to be the “Stripe for RGB payments.” It enables businesses (exchanges, online merchants, etc.) to easily accept and manage RGB-based tokens with an experience similar to familiar payment gateways (Stripe, PayPal, Coinbase Commerce, CoinPayments). The system is composed of three main components working together:

* ThunderLink RGB Manager (managed by ThunderLink) – provides the core RGB asset functionality and APIs (invoice generation, state tracking, PSBT handling).\

* **ThunderLink RGB Signer**  (Merchant Node) – is a lightweight service run by the customer (exchange or merchant) on their own infrastructure, holding their private keys and responsible for signing Bitcoin transactions (via PSBT). Communicating via RabbitMQ to listen for signing requests on a private channel, signing PSBTs sent by the ThunderLink RGB Manager,&#x20;



* ThunderLink RGB Client SDK (Frontend) – a lightweight React library that integrates into websites or apps (web stores, exchange UIs) to present UI elements (e.g. “Pay with RGB” buttons, payment widgets) and communicate with the merchant’s Server.\


Target Users: The primary users are crypto exchanges (who can integrate ThunderLink for RGB token deposits & withdrawals) and RGB application developers or merchants who want to accept RGB asset payments in their apps or online stores. Secondary users include end-customers using an RGB-compatible wallet to make payments on these platforms.

This design description will detail the UI/UX flow for each component, illustrate the integration touchpoints, and breakdown functional operations like invoice creation, payment processing, balance inquiries, and PSBT signing. We also outline security boundaries between components and provide a glimpse of a future ThunderLink Dashboard for monitoring transactions and integrations.



### Architecture and Components

ThunderLink architecture separates concerns of asset logic, private key custody, and user interface for maximum security and ease of integration. Below is an overview of how the components interact in the system:

The **Client SDK** (used in the front-end) communicates with the RGB _via_ **ThunderLink RGB Manager.** This server holds no sensitive data, signs transactions, and acts as a secure intermediary. It interacts with the **ThunderLink RGB Manager** through API calls to coordinate RGB asset state and Bitcoin transactions using PSBTs (Partially Signed Bitcoin Transactions). The **Bitcoin Layer 1 network** serves as the settlement layer, while users pay invoices using an **RGB-compatible wallet**.

\
