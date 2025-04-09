---
icon: question
---

# FAQ

This document is organized into major sections for **Bitcoin-Native Infrastructure** (covering Lightning and RGB-based tools) and **Bitcoin Sidechains** (covering sidechain and rollup connectivity).

### General Questions

<details>

<summary><strong>What is ThunderStack in a nutshell?</strong></summary>

**ThunderStack** is a unified platform for Bitcoin layer-2 infrastructure. It simplifies deploying and managing the various “layers” built on top of Bitcoin. This includes the **Bitcoin-native layers** like the Lightning Network and RGB (where users keep custody of funds and use Bitcoin’s security model off-chain), and **Bitcoin side systems** like sidechains and rollups (which are separate networks anchored to Bitcoin’s security or value). ThunderStack offers cloud-hosted services: you can spin up Lightning/RGB nodes (ThunderCloud), integrate Lightning payments easily (ThunderEngine), get liquidity (ThunderFlow), backup states (ThunderSafe), accept payments (ThunderLink), and connect to sidechains (ThunderStack RPC) all under one roof. The goal is to let developers and businesses adopt these technologies without having to become experts in each one’s. ThunderStack takes care of hosting nodes, providing high availability, robust APIs, and security, so you can focus on building your application or business logic.

</details>

<details>

<summary><strong>How is ThunderStack different from running my own nodes or infrastructure?</strong></summary>

Running your own Bitcoin, Lightning, or sidechain nodes means dealing with hardware, uptime, networking, and updates yourself. ThunderStack abstracts that away as a managed service:

* **Ease of Use:** Via web interfaces and APIs, you accomplish in minutes what might take hours or days to set up on your own (e.g., configuring a Lightning node with channels and liquidity).
* **Expert Management:** ThunderStack team maintains the nodes (applying security patches, monitoring for issues 24/7). This reduces the risk of downtime or attacks compared to a DIY node that might be left unmonitored.
* **Integrated Ecosystem:** All ThunderStack services are designed to work together. For instance, a node from ThunderCloud can easily use ThunderFlow to get a channel, and ThunderSafe will automatically back it up. If you did it all yourself, you’d have to integrate different software pieces manually.
* **Scalability:** With ThunderStack, if your project suddenly needs to scale (more nodes, more RPC calls, more throughput), you can upgrade your plan or add resources quickly. Doing it yourself might mean ordering new servers or spending time re-architecting.
* **Cost Efficiency:** While ThunderStack isn’t free, consider the manpower and opportunity cost of maintaining infrastructure. For many companies, paying ThunderStack a subscription is cheaper than employing dedicated engineers to do the same job. Additionally, ThunderStack multi-tenant architecture can be more cost-efficient on hardware usage than many separate individual nodes.
* **Focus on Development:** You get to focus on your application’s features, user experience, and business growth instead of low-level infrastructure details. This can accelerate time-to-market for new Bitcoin-layer solutions.

</details>

<details>

<summary><strong>Which Bitcoin Layer-2 technologies does ThunderStack support?</strong></summary>

ThunderStack covers a broad spectrum of Bitcoin’s layer-2:

* **Lightning Network:** All aspects – running nodes (ThunderCloud), providing non-custodial access (ThunderEngine), liquidity provisioning (ThunderFlow), payment acceptance (ThunderLink).
* **RGB Protocol:** A smart contract and asset issuance layer that works with client-side validation on Bitcoin. ThunderStack Lightning nodes (RLN) are RGB-enabled, and services like ThunderFlow and ThunderLink fully support RGB assets. This means ThunderStack is ready for tokenized assets on Bitcoin and their transfer via Lightning.
* **Sidechains (BTC Sidechains):** These are separate blockchains that use BTC (or pegged BTC) as their gas currency, e.g. Rootstock (RSK), Liquid (possibly, if supported in future), and others. Thunderstack RPC provides connectivity to these chains. Essentially, if a chain is Bitcoin-adjacent (either merge-mined, pegged, or otherwise linked), ThunderStack aims to support it.
* **Rollups and Experimental Layers:** This includes things like **BitVM rollups** or staking rollups. While these are emerging, ThunderStack support for them is either active (if networks exist) or planned.
* **Other protocols (if any):** If Bitcoin layer-2 expands beyond these (e.g., Ark protocol or federated systems like Fedimint), ThunderStack could potentially support those as well, though they haven’t been explicitly mentioned. The concept of “Bitcoin layers” is broad, and ThunderStack mission is to be the go-to infrastructure provider for any promising Bitcoin layer.

</details>

<details>

<summary><strong>Is ThunderStack non-custodial? Do I retain control of my funds and keys?</strong></summary>

ThunderStack strives to enable non-custodial use wherever possible:

* **RGB:** RGB is inherently client-side, meaning the state and secret data for assets reside with the user. ThunderStack’s infrastructure helps transport and manage state, but the authority to create a valid state transition (like spending an asset) lies with your keys. ThunderStack doesn’t get those secrets – it just relays the data.
* **ThunderSafe:** It’s designed to be trustless (encrypted backups) ThunderStack stores your data but cannot read or misuse it without your keys.

- **Sidechain/RPC usage:** When using ThunderStack RPC, you might be sending signed transactions. Thunderstack isn’t taking custody; you control the keys that sign those transactions. We’re just a relay.&#x20;

</details>

### Bitcoin-Native Infrastructure

_Bitcoin-native protocols are those built on Bitcoin where users retain custody of their funds and can always fall back to the Bitcoin base layer. Thunderstack’s Bitcoin-Native Infrastructure provides managed services for such protocols, including the RGB smart-contract protocol and the Lightning Network._



<details>

<summary><strong>What is ThunderCloud?</strong></summary>

**ThunderCloud** is a cloud-based solution designed for seamless node management of **RGB** and RGB Lightning Network applications. It provides one-click deployment and hosting of full **RGB Lightning Nodes (RLN)**, which combine Bitcoin Lightning Network functionality with RGB asset support. ThunderCloud ensures high availability, enhanced security, and ease of use for running Lightning/RGB nodes in the cloud.

</details>

<details>

<summary><strong>How do I integrate ThunderCloud into my application?</strong></summary>

Integration methods:

* **Web Dashboard:** You can manage nodes via a user-friendly dashboard (create, monitor, and control your RLN instances with a few clicks).
* **API Access:** For programmatic control, ThunderCloud exposes RESTful endpoints (and client libraries) so your application can spin up or shut down nodes, query node info, and even initiate RLN operations via API.
* **Webhooks:** Set up webhooks to get real-time callbacks to your app on important events (like a channel opened or afer) for seamless automation.

</details>

<details>

<summary><strong>What security and reliability measures does ThunderCloud have?</strong></summary>

Security and reliability are top priorities for ThunderCloud:

[https://docs.thunderstack.org/bitcoin-native-infrastructure/readme/getting-started-with-thunderstack-rgb-cloud/security](https://docs.thunderstack.org/bitcoin-native-infrastructure/readme/getting-started-with-thunderstack-rgb-cloud/security)

</details>

<details>

<summary><strong>Does ThunderCloud provide an API ?</strong></summary>

Yes. Everything in ThunderCloud is controllable via its **API** and web interfaces:

* The **ThunderCloud API** allows full control of nodes
* There is a **Node API (RLN API)** for the node itself. Essentially, they can be operated via REST JSON APIs. [RLN API docs](bitcoin-native-infrastructure/readme/getting-started-with-thunderstack-rgb-cloud/rgb-lightning-node-api.md)

</details>





### Bitcoin Sidechains Infrastructure

_Bitcoin sidechains (and similar “side systems”) are independent blockchains that are pegged to Bitcoin or use BTC as their native asset. Rollups are another type of layer that post data to Bitcoin. ThunderStack’s sidechain infrastructure provides connectivity to these systems so developers can interact with them without running full nodes themselves._

#### ThunderStack RPC (Sidechain/Rollup Connectivity)

<details>

<summary><strong>What is ThunderStack RPC?</strong></summary>

**ThundeStack RPC** is a cloud-based RPC (Remote Procedure Call) service that gives you instant API access to various Bitcoin-aligned sidechains and layer-2 networks. In essence, it’s a **hosted node service**: instead of running your own node for a Bitcoin sidechain or rollup, you can use ThunderStack’s RPC endpoints to read and submit transactions to those networks. It works over HTTPS with standard JSON-RPC calls, acting as a gateway to blockchain nodes. ThunderStack RPC currently focuses on **Bitcoin EVM-compatible layers** and will extend to other emerging Bitcoin rollup tech. Using ThunderStack RPC means you don’t have to worry about syncing nodes, scalability, or maintenance – you get reliable access with a simple web API key.

</details>

<details>

<summary><strong>Who should use ThunderStack RPC?</strong></summary>

* Need to interact with Bitcoin sidechains or rollups as part of their application (querying data or broadcasting transactions), but do not want the hassle of running full nodes for each network.

- Are building multi-chain dApps or services where Bitcoin sidechains are involved (for example, an application on RSK, or a DeFi platform that utilizes a Bitcoin drivechain or rollup).

* Want a unified, high-performance API for multiple networks. If your app touches many chains, keeping all those nodes updated and running is complex – ThunderStack RPC provides one platform to access many.

- Require reliable uptime and scaling for RPC. If your project scales to many users making RPC calls, ThunderStack can handle the load balancing and scaling, whereas a self-hosted node might become a bottleneck.

* Are exploring new Bitcoin layer-2 tech (like BitVM-based rollups) without committing resources to run each experimental node.

</details>

<details>

<summary><strong>Which networks can I access with Thunderstack RPC?</strong></summary>

ThunderStack RPC supports a **diverse range of Bitcoin sidechains and related networks**, all chosen for their alignment with Bitcoin’s security model. The list is continually growing. As of now, supported networks include:

* **Rootstock (RSK):** A Bitcoin sidechain that is EVM-compatible, merge-mined with Bitcoin, using BTC pegged tokens. RSK allows Ethereum-like smart contracts with Bitcoin security, and ThunderStack provides full RPC access to it.
* **Strata, Citrea, Bitlayer, BitcoinOS & Others:** Emerging BitVM-powered networks, which integrate Bitcoin via BitVM. ThunderStack supports their RPC so you can interact with cross-chain bridges and smart contracts involving Bitcoin.

- **Mainnet vs Testnet:** Many of these networks have testnets; Thunderstack likely offers endpoints for testnets and regtest/dev networks as well, so you can develop and test easily.

[_ThunderStack documentation_](bitcoin-sidesystems-infrastructure/products/thunder-rpc/supported-networks.md) _and dashboard will have the exact up-to-date list of supported networks, including chain IDs or identifiers you’ll use in the API to select a network._

</details>

<details>

<summary><strong>What about security and reliability for the RPC service?</strong></summary>

From a security standpoint:

* **Transport Security:** All endpoints are HTTPS, meaning the data (your RPC calls and responses) are encrypted in transit. No one can snoop on your queries or responses.
* **Authentication:** The API key ensures only authorized users make requests. Internally, ThunderStack’s _eRPC proxy_ infrastructure assigns your requests to your account and monitors usage. If someone stole your key, they could use your quota, so it’s important to keep it secret.
* **Isolation:** Each user’s requests run through an isolated proxy instance (as mentioned, for every API key, ThunderStack launches an eRPC proxy that routes to the appropriate node cluster). This isolation means even if one user overloads the system or triggers an issue, it shouldn’t affect others – their traffic is segmented.
* **Consensus and Data Integrity:** Because you’re not running the node, you trust ThunderStack nodes to give correct data. ThunderStack uses the official client software for each network and connects to the genuine peer-to-peer network, ensuring authentic data.
* **Uptime:** ThunderStack will have monitoring and failover for node clusters. If a node goes down, your calls get rerouted to another. We maintain near 99.9% uptime for the RPC endpoints, suitable for production services.
* **Rate Limiting and Abuse Prevention:** To maintain reliability, there are quotas (both daily usage via **Compute Units (CUs)** and per-second rate limits via **CUPS**) for each plan. This prevents any single user from overwhelming the service and ensures fair usage. If you hit limits, you’ll get error responses, but the service remains stable for everyone.
* **Updates and Compatibility:** ThunderStack keeps the nodes updated to the latest stable versions, meaning you get all the latest protocol features and security patches. If any breaking changes occur (say a network upgrade that changes the RPC API), ThunderStack will handle the upgrade and notify users if any action is needed.

</details>

<details>

<summary><strong>How is usage measured and what are Compute Units (CUs) and CUPS?</strong></summary>

ThunderStack uses a credit system to measure RPC usage:

* **Compute Units (CUs):** A Compute Unit is a unit of measure that represents the computational/IO cost of a given RPC call. Not all RPC calls are equal – ThunderStack assigns a CU price to each RPC method. Your account has a **daily CU quota** (depending on your plan). Every time you make a request, the corresponding CU cost is deducted from your daily allotment. This way, you’re only paying (or limited) by actual usage rather than a flat number of requests. It encourages efficient use of calls. For details refer to [docs](bitcoin-sidesystems-infrastructure/products/thunder-rpc/compute-units-cus.md)
* **CUPS (Calls (or Compute Units) Per Second):**  Even if you have a large daily CU quota, you can’t use it all at once; there’s a per-second cap to prevent bursts from overloading the system. For example, your plan might allow, say, 100 CUs per second. If one RPC call costs 10 CUs, that’s effectively 10 calls per second of that type. If you exceed, subsequent calls might be throttled or rejected until the next second. This ensures fairness and stability. For details refer to [docs](bitcoin-sidesystems-infrastructure/products/thunder-rpc/cups-rate-limit.md)
* **Monitoring CUs:** The ThunderStack dashboard will show how many CUs you’ve used and how many remain today, so you can monitor if you’re approaching limits.&#x20;
* **Efficiency:** The CU system encourages you to avoid extremely expensive calls or at least be mindful of them. For example, instead of requesting full data repeatedly, you might store results or use filters to narrow queries – saving your CU budget.
* **Unused Quota:** Typically, unused daily CUs don’t carry over (each day it resets according to plan). If you need more on a particular day, you might upgrade your plan or contact support for a bump.

</details>

#### Future Support for Bitcoin Rollups RPC Infrastructure

<details>

<summary><strong>What are Bitcoin rollups and will ThunderStack support them?</strong></summary>

**Bitcoin rollups** are a new class of layer-2 solutions where transaction data is compressed and posted on Bitcoin (L1) for security, allowing higher throughput and smart contracts without changing Bitcoin’s rules. They often rely on validity proofs (like SNARKs) or fraud proofs to ensure correctness. Examples of early concepts include **BitVM** and various zk-rollup proposals that use Bitcoin for data and arbitration. ThunderStack is closely following these developments and has architecture ready to support rollup RPC endpoints as they become available. In fact, networks like **Strata**, **Citrea**, **Bitlayer** which ThunderStack already supports can be seen as early Bitcoin rollup implementations using BitVM technology. As more Bitcoin rollup frameworks mature, ThunderStack will integrate them into the RPC service. The goal is to make ThunderStack RPC the go-to hub for interacting with _all_ Bitcoin layer-2 networks, including true rollups.

</details>

<details>

<summary><strong>How does ThunderStack handle the security and trust aspects of rollups?</strong></summary>

One important aspect: some rollups may run centralized sequencers or be in early stages. ThunderStack providing access to them doesn’t change their trust model (e.g., if a rollup has a centralized sequencer, using ThunderStack doesn’t decentralize it). However, ThunderStack will:

* Vet the networks it supports to some degree – ensuring they are not outright scams and have some community trust or backing.
* Possibly label **experimental networks** as such, so developers understand the risk.
* For user clarity, we might highlight whether a network is a true trustless rollup or something like a federated sidechain, so you know what you’re dealing with.
* Continue to ensure the RPC data is relayed correctly. In a rollup with fraud proofs, for example, ThunderStack node will show the state as is, but finality might be deferred until a challenge period passes. ThunderStack might provide that info (like a flag if a transaction is still in a challenge window).
* If a rollup submits proofs to Bitcoin, ThunderStack could cross-verify events with Bitcoin’s chain (for instance, confirming that a rollup epoch is finalized only after the Bitcoin transaction including its proof is confirmed). This could be extra logic we add to ensure developers get accurate read on finality.

Ultimately, ThunderStack aims to simplify usage, but devs should still understand the underlying trust model of whichever rollup they use (e.g., an optimistic rollup has a fraud window; a ZK-rollup is instant finality if proofs verify; a sidechain might be fully trustful with a federation, etc.).

</details>











