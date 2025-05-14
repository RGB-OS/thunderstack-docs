---
description: 'URL: https://rgb-manager.thunderstack.org'
---

# RGB Manager

The ThunderLink RGB Manager, hosted by ThunderStack, manages RGB state for merchants. It offers a secure API for tasks such as creating RGB invoices, tracking asset transfers, and building Partially Signed Bitcoin Transactions (PSBTs). This server encapsulates the RGB protocol library, handling both off-chain state and on-chain commitments of RGB.

**Functional Responsibilities:**

### Security & Trust

The  ThunderLink RGB Manager minimizes risk by never needing users' or merchants' private keys. Merchants must trust ThunderLink to correctly use the RGB protocol without blocking or misdirecting transactions. Even if ThunderLink is compromised, the attacker can't directly steal assets since merchants have the signing keys. The worst-case scenario is service disruption, like incorrect invoice data or transaction broadcasting failures, which can be mitigated through audit logs and transaction verification by merchants.

### Broadcast & Confirmation

After a merchant signs a PSBT, ThunderLink can send the fully signed transaction to the RGB and monitor confirmations. For incoming payments, it looks for transactions that satisfy invoices, such as detecting UTXO with RGB asset transfers.

### PSBT Handling

For outgoing transactions, ThunderLink creates a Partially Signed Bitcoin Transaction (PSBT) with proper inputs, outputs, and RGB commitments. Without private keys, it relies on the merchant's server for the final signature. PSBT fosters secure collaboration between ThunderLink and merchants by keeping private keys offline. This allows ThunderLink RGB Manager to structure transaction details securely.

### State Tracking

The server tracks the merchant's RGB wallet state. It manages necessary data to validate and update asset ownership during transactions. When an invoice is paid, the asset state is updated and documented for the merchant. It also handles synchronization of state updates across transactions.
