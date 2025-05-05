# RGB Client Server

**RGB Client Server**

The RGB Client Server is a backend service operated by the customer (e.g., a merchant or exchange) within their own infrastructure. It serves as a secure bridge between the **ThunderLink RGB Manager** and the customer's private Bitcoin key infrastructure (or key management system). This component is responsible for holding or accessing the necessary keys to sign PSBTs (Partially Signed Bitcoin Transactions).

The server includes a pluggable authorization layer, allowing customers to implement their own authentication and verification logic as needed. The overall configuration is customizable to meet the specific operational and security requirements of the customer. It also monitors transfer states and performs background operations such as refreshing in-progress transfers or marking expired ones as failed.

These actions are coordinated by internal watchers that periodically poll the ThunderLink RGB Manager to retrieve the current state and apply necessary updates, such as creating new UTXOs or failing outdated transfers. In addition to facilitating communication with the **ThunderLink RGB Manager**, the RGB Client Server exposes local API endpoints that the **Client SDK** (frontend) can interact with.

Beyond transaction signing, the server is responsible for additional logic around UTXO management: customers can configure the number of UTXOs that should remain available, and the server will ensure this threshold is maintained.

**Deployment**

This component can run as a Docker container or a standalone Node.js server, allowing merchants to choose the setup that best fits their

**Configurations**\
Operational options—such as enabling or disabling watchers, setting the UTXO limit, and customizing watcher intervals—can be configured using environment variables.

Functional Responsibilities:

**1. Key Management & Signing**\
The primary responsibility of the **RGB Client Server** is to securely manage the merchant’s  keys or interface with a secure key management system.\
When the ThunderLink RGB Manager provides a PSBT, the server validates the request, signs the transaction using the private key associated with the relevant UTXO, and returns the signed PSBT (or fully finalized transaction). Since PSBTs include metadata for validation, the merchant can always verify what they are signing. For example, if an exchange initiates an RGB asset withdrawal, the PSBT will include the UTXO holding the asset and the destination output. The server signs the input only if it controls the corresponding key.

**2. API Proxy for Client SDK**\
The server exposes API endpoints for the front-end Client SDK (e.g., `/invoice`,`/listassets` ,`/balance`). These endpoints proxy requests to ThunderLink RGB Manager APIs while allowing the merchant to enforce their own rules, apply authentication, or inject metadata. For instance, when the frontend requests invoice creation, the server can validate the amount, attach business-specific tags, and then forward the request to ThunderLink Cloud. This architecture ensures full visibility and control over interactions initiated by the user.

**3. UTXO Management & Monitoring**\
The server maintains knowledge of owned addresses or UTXOs—either by deriving them from the merchant’s extended public key or retrieving them via ThunderLink RGB manager. This enables it to validate incoming payments, select UTXOs for withdrawals, and manage UTXO fragmentation.\
Through configurable watchers, the server periodically polls ThunderLink Cloud for updated transfer states. It can trigger automated actions such as generating new UTXOs when availability drops below a configurable UTXO limit, or failing expired transfers. These background tasks can be enabled or disabled individually and tuned via environment variables.

**4.Security & Isolation**\
The design maintains a clear security boundary:  the Server-Client is the only component with access to private keys. It should run in a secure environment under the merchant’s control. All communications between the Server-Client and ThunderLink RGB Manager are authenticated. The RGB Client Server should verify the PSBT details from ThunderLink RGB Manager before signing, acting as a final gate against any tampering. The merchant trusts this component as part of their infrastructure, similar to how they would trust their own wallet or exchange backend.
