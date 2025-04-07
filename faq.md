# FAQ

This document is organized into major sections for **Bitcoin-Native Infrastructure** (covering Lightning and RGB-based tools) and **Bitcoin Sidechains** (covering sidechain and rollup connectivity).

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

#### Thunderstack RPC (Sidechain/Rollup Connectivity)

<details>

<summary><strong>What is ThunderStack RPC?</strong></summary>

**Thunderstack RPC** is a cloud-based RPC (Remote Procedure Call) service that gives you instant API access to various Bitcoin-aligned sidechains and layer-2 networks. In essence, it’s a **hosted node service**: instead of running your own node for a Bitcoin sidechain or rollup, you can use Thunderstack’s RPC endpoints to read and submit transactions to those networks. It works over HTTPS with standard JSON-RPC calls, acting as a gateway to blockchain nodes. Thunderstack RPC currently focuses on **Bitcoin EVM-compatible layers** and will extend to other emerging Bitcoin rollup tech. Using Thunderstack RPC means you don’t have to worry about syncing nodes, scalability, or maintenance – you get reliable access with a simple web API key.

</details>

<details>

<summary><strong>Who should use ThunderStack RPC?</strong></summary>

* Need to interact with Bitcoin sidechains or rollups as part of their application (querying data or broadcasting transactions), but do not want the hassle of running full nodes for each network.

- Are building multi-chain dApps or services where Bitcoin sidechains are involved (for example, an application on RSK, or a DeFi platform that utilizes a Bitcoin drivechain or rollup).

* Want a unified, high-performance API for multiple networks. If your app touches many chains, keeping all those nodes updated and running is complex – Thunderstack RPC provides one platform to access many.

- Require reliable uptime and scaling for RPC. If your project scales to many users making RPC calls, Thunderstack can handle the load balancing and scaling, whereas a self-hosted node might become a bottleneck.

* Are exploring new Bitcoin layer-2 tech (like BitVM-based rollups) without committing resources to run each experimental node.

</details>

<details>

<summary><strong>Which networks can I access with Thunderstack RPC?</strong></summary>

ThunderStack RPC supports a **diverse range of Bitcoin sidechains and related networks**, all chosen for their alignment with Bitcoin’s security model. The list is continually growing. As of now, supported networks include:

* **Rootstock (RSK):** A Bitcoin sidechain that is EVM-compatible, merge-mined with Bitcoin, using BTC pegged tokens. RSK allows Ethereum-like smart contracts with Bitcoin security, and ThunderStack provides full RPC access to it.
* **Bitlayer & Others:** Emerging BitVM-powered networks like _Bitlayer_ itself, which integrate Bitcoin via BitVM. ThunderStack supports their RPC so you can interact with cross-chain bridges and smart contracts involving Bitcoin.
* **Strata, Citrea, BitcoinOS, Bsquared, GOAT, Plasma, Ola, Zulu, BOB, Merlin, CoreDAO, zkLayer, LayerEdge, Bitfinity, Babylon, Anduro, etc.:** These are various experimental or new networks (some might be rollups, some sidechains) that Thunderstack is supporting. Each of these has its own focus (for instance, Babylon focuses on Bitcoin-finalized consensus for other chains, etc.). By using Thunderstack RPC, you can access any of them with the same API approach.
* **Mainnet vs Testnet:** Many of these networks have testnets; Thunderstack likely offers endpoints for testnets and regtest/dev networks as well, so you can develop and test easily.

[_ThunderStack documentation_](bitcoin-sidesystems-infrastructure/products/thunder-rpc/supported-networks.md) _and dashboard will have the exact up-to-date list of supported networks, including chain IDs or identifiers you’ll use in the API to select a network._

</details>

<details>

<summary><strong>What about security and reliability for the RPC service?</strong></summary>

From a security standpoint:

* **Transport Security:** All endpoints are HTTPS, meaning the data (your RPC calls and responses) are encrypted in transit. No one can snoop on your queries or responses.
* **Authentication:** The API key ensures only authorized users make requests. Internally, Thunderstack’s _eRPC proxy_ infrastructure assigns your requests to your account and monitors usage. If someone stole your key, they could use your quota, so it’s important to keep it secret.
* **Isolation:** Each user’s requests run through an isolated proxy instance (as mentioned, for every API key, Thunderstack launches an eRPC proxy that routes to the appropriate node cluster). This isolation means even if one user overloads the system or triggers an issue, it shouldn’t affect others – their traffic is segmented.
* **Consensus and Data Integrity:** Because you’re not running the node, you trust ThunderStack nodes to give correct data. ThunderStack uses the official client software for each network and connects to the genuine peer-to-peer network, ensuring authentic data (e.g., they run RSK nodes that follow the RSK consensus). They likely monitor for chain reorganizations or issues and might use multiple nodes for the same network to cross-verify responses.
* **Uptime:** Thunderstack will have monitoring and failover for node clusters. If a node goes down, your calls get rerouted to another. They probably maintain near 99.9% uptime for the RPC endpoints, suitable for production services.
* **Rate Limiting and Abuse Prevention:** To maintain reliability, there are quotas (both daily usage via **Compute Units (CUs)** and per-second rate limits via **CUPS**) for each plan. This prevents any single user from overwhelming the service and ensures fair usage. If you hit limits, you’ll get error responses, but the service remains stable for everyone.
* **Updates and Compatibility:** ThunderStack keeps the nodes updated to the latest stable versions, meaning you get all the latest protocol features and security patches. If any breaking changes occur (say a network upgrade that changes the RPC API), Thunderstack will handle the upgrade and notify users if any action is needed.

</details>

<details>

<summary><strong>How is usage measured and what are Compute Units (CUs) and CUPS?</strong></summary>

ThunderStack uses a credit system to measure RPC usage:

* **Compute Units (CUs):** A Compute Unit is a unit of measure that represents the computational/IO cost of a given RPC call. Not all RPC calls are equal – ThunderStack assigns a CU price to each RPC method. Your account has a **daily CU quota** (depending on your plan). Every time you make a request, the corresponding CU cost is deducted from your daily allotment. This way, you’re only paying (or limited) by actual usage rather than a flat number of requests. It encourages efficient use of calls. For details refer to [docs](bitcoin-sidesystems-infrastructure/products/thunder-rpc/compute-units-cus.md)
* **CUPS (Calls (or Compute Units) Per Second):** This likely refers to the rate limit. Even if you have a large daily CU quota, you can’t use it all at once; there’s a per-second cap to prevent bursts from overloading the system. For example, your plan might allow, say, 100 CUs per second. If one RPC call costs 10 CUs, that’s effectively 10 calls per second of that type. If you exceed, subsequent calls might be throttled or rejected until the next second. This ensures fairness and stability. For details refer to [docs](bitcoin-sidesystems-infrastructure/products/thunder-rpc/cups-rate-limit.md)
* **Monitoring CUs:** The Thunderstack dashboard will show how many CUs you’ve used and how many remain today, so you can monitor if you’re approaching limits.&#x20;
* **Efficiency:** The CU system encourages you to avoid extremely expensive calls or at least be mindful of them. For example, instead of requesting full data repeatedly, you might store results or use filters to narrow queries – saving your CU budget.
* **Unused Quota:** Typically, unused daily CUs don’t carry over (each day it resets according to plan). If you need more on a particular day, you might upgrade your plan or contact support for a bump.

</details>

#### Future Support for Bitcoin Rollups RPC Infrastructure

<details>

<summary><strong>What are Bitcoin rollups and will ThunderStack support them?</strong></summary>

**Bitcoin rollups** are a new class of layer-2 solutions where transaction data is compressed and posted on Bitcoin (L1) for security, allowing higher throughput and smart contracts without changing Bitcoin’s rules. They often rely on validity proofs (like SNARKs) or fraud proofs to ensure correctness. Examples of early concepts include **BitVM** and various zk-rollup proposals that use Bitcoin for data and arbitration. ThunderStack is closely following these developments and has architecture ready to support rollup RPC endpoints as they become available. In fact, networks like **Bitlayer** which ThunderStack already supports can be seen as early Bitcoin rollup implementations using BitVM technology. As more Bitcoin rollup frameworks mature (e.g. rollups, drivechains functioning as rollups, or other sidechains that post state to Bitcoin), ThunderStack will integrate them into the RPC service. The goal is to make ThunderStack RPC the go-to hub for interacting with _all_ Bitcoin layer-2 networks, including true rollups.

</details>

<details>

<summary><strong>How will ThunderStack integrate future rollup infrastructure?</strong></summary>

ThunderStack infrastructure is built to be adaptable:

* They will run the necessary **rollup nodes** or clients on the backend. If a new rollup chain launches (say an Optimistic Rollup anchored in Bitcoin), ThunderStack would run a full node or sequencer client for that rollup.
* The **RPC interface** would likely mimic whatever the rollup node’s API is. If the rollup is EVM-like, it might just appear as another EVM chain endpoint. If it’s something new, ThunderStack will provide documentation on how to call it.
* **BitVM-based rollups:** BitVM is a concept enabling off-chain computation with Bitcoin as the enforcer. Should a rollup use BitVM, ThunderStack might need to run special software (like a BitVM prover/verifier node — more on that in the BitVM section below) to support it. They will incorporate those so developers can, for example, submit transactions to a BitVM rollup via a ThunderStack endpoint.
* **Data Availability (DA) and Proof Services:** Some rollups might require separate Data Availability layers or proof submission endpoints. ThunderStack could offer integrated services for those as well (so not only the RPC to interact with the chain, but maybe an API to fetch proofs or status of proofs if that’s part of interacting with the rollup).
* **Continuous Updates:** Rollup tech is evolving, so ThunderStack will update supported protocols as needed (if a rollup changes its proving system, etc.). By using ThunderStack, developers can avoid constantly updating their own nodes for each upgrade – ThunderStack handles it and maintains backward compatibility as much as possible.
* **Unified Experience:** As with sidechains, ThunderStack will aim to keep the experience uniform. Whether you’re dealing with a sidechain or a rollup, you’ll likely manage it similarly in the ThunderStack dashboard, with similar API patterns, just specifying a different network. This means learning curve for one network translates to others easily on the platform.

</details>

<details>

<summary><strong>How does ThunderStack handle the security and trust aspects of rollups?</strong></summary>

One important aspect: some rollups may run centralized sequencers or be in early stages. ThunderStack providing access to them doesn’t change their trust model (e.g., if a rollup has a centralized sequencer, using ThunderStack doesn’t decentralize it). However, Thunderstack will:

* Vet the networks it supports to some degree – ensuring they are not outright scams and have some community trust or backing.
* Possibly label **experimental networks** as such, so developers understand the risk.
* For user clarity, they might highlight whether a network is a true trustless rollup or something like a federated sidechain, so you know what you’re dealing with.
* Continue to ensure the RPC data is relayed correctly. In a rollup with fraud proofs, for example, ThunderStack node will show the state as is, but finality might be deferred until a challenge period passes. ThunderStack might provide that info (like a flag if a transaction is still in a challenge window).
* If a rollup submits proofs to Bitcoin, ThunderStack could cross-verify events with Bitcoin’s chain (for instance, confirming that a rollup epoch is finalized only after the Bitcoin transaction including its proof is confirmed). This could be extra logic they add to ensure developers get accurate read on finality.

Ultimately, ThunderStack aims to simplify usage, but devs should still understand the underlying trust model of whichever rollup they use (e.g., an optimistic rollup has a fraud window; a ZK-rollup is instant finality if proofs verify; a sidechain might be fully trustful with a federation, etc.).

</details>

#### BitVM Prover & Observation Dashboard/API

<details>

<summary><strong>What is BitVM, and what is Thunderstack’s BitVM Prover &#x26; Observation Dashboard?</strong></summary>

**BitVM** is a proposed framework that allows for Turing-complete computations to be verified via the Bitcoin blockchain, _without changing Bitcoin’s consensus_. It’s like enabling smart contracts using Bitcoin as the judge in a dispute. BitVM involves two parties (a Prover and a Verifier); the Prover performs some computation off-chain and the Verifier can challenge if they disagree, with Bitcoin scripts used to resolve disputes. This can be used to build Bitcoin rollups or advanced contracts (often categorized under “validity rollups” or “optimistic rollups” in Bitcoin context).

ThunderStack **BitVM Prover & Observation Dashboard/API** is an upcoming service aimed at supporting this BitVM paradigm. Essentially, ThunderStack wants to provide the heavy lifting infrastructure for BitVM-based applications:

* The **Prover** part refers to running the computations and generating the transcripts/proofs that would be needed if challenged. ThunderStack can operate Prover nodes on behalf of users or developers, so you don’t need to have a powerful server constantly running the computations.
* The **Observation Dashboard** refers to a monitoring interface (dashboard) and API for the Verifier side (or observers in the system). This would track the state of BitVM computations, whether any disputes have been raised, and the status of on-chain commitments. It’s essentially a control center to observe and interact with BitVM sequences.

In simpler terms, if you build a rollup or application using BitVM, ThunderStack service would let you:

* Submit your computations to be executed by a robust Prover service.
* Monitor the execution and outcome via a dashboard.
* Rely on ThunderStack to watch the Bitcoin blockchain for any needed actions (like posting a fraud proof or responding to a challenge within the allotted time).

</details>

<details>

<summary><strong>Who would use the BitVM Prover service and why?</strong></summary>

This is targeted at advanced Bitcoin developers and businesses working on Layer-2 solutions like rollups or complex smart contracts:

* **Rollup Operators:** If you are running a Bitcoin rollup that uses BitVM for proving state transitions, you need to continuously produce proofs for each batch of transactions and be ready to respond to disputes. ThunderStack Prover service can take on that role, ensuring proofs are generated quickly and correctly.
* **Watchtowers/Observers:** These are entities that watch for invalid behavior. The Observation API would enable them to easily get data and possibly react. For example, a watchtower service could use ThunderStack API to get the transcript of a BitVM computation and automatically detect a fraud, then use ThunderStack to submit the fraud proof on-chain.
* **Developers experimenting with BitVM:** If you are building an app like a DLC (Discrete Log Contract) or some complex conditional payment that leverages BitVM technique, you might use the ThunderStack Prover to simulate or actually execute the computation part and verify outcomes, without having to implement the entire verification game logic from scratch.
* **Businesses needing off-chain compute with Bitcoin settlement:** Imagine a company wants to run some logic (like a random draw, or an auction) off-chain but have the final result enforced by Bitcoin. They could define it in a BitVM-compatible way. The ThunderStack Prover would execute the logic, and everyone can trust the outcome because if the Prover tried to cheat, it’d get caught on-chain. So businesses can offload the compute to ThunderStack while maintaining trust through Bitcoin.

</details>

<details>

<summary><strong>What does the BitVM Prover &#x26; Observation service actually do (features)?</strong></summary>



Key components and features likely include:

* **BitVM Execution Engine:** ThunderStack will run an engine that takes a BitVM program (the set of instructions for the off-chain computation) and executes it as the Prover would. It produces the cryptographic transcript of the execution (the sequence of hashes or commitments that summarize each step of computation). This transcript is what a Verifier would use to challenge a specific step if they disagree.
* **Automated Verifier Response:** Alongside proving, ThunderStack system can act as a Verifier or assist external Verifiers. For example, if ThunderStack is proving on behalf of a rollup operator, the users (Verifiers) might run their own nodes or trust a third-party. However, ThunderStack could also provide a service to do the verification and challenge on behalf of users who delegate that role (similar to a watchtower concept). This could be offered as a service: ThunderStack monitors and if its own Prover (or any Prover) cheats, ThunderStack will call it out on-chain.
* **Dashboard UI:** A web interface where you can see all ongoing BitVM sessions. For each BitVM contract or rollup:
  * See the current step of execution, the outputs produced, etc.
  * See the time left in challenge periods.
  * Buttons or controls to manually initiate a challenge or respond to one (if for some reason automation is off or you want manual control).
  * Logs of what has happened – e.g., “Computation for batch #123 completed off-chain, awaiting challenge period (24h remaining).” or “No fraud detected. Batch #123 finalized.”
  * Visualizations: possibly an instruction-by-instruction view or at least summary of computation results for easier analysis.
* **Observation API:** All the data in the dashboard likely is available via an API. Developers can query the status of a computation, subscribe to events (like “challenge filed” or “proof posted”), and maybe retrieve parts of the transcript or state.
* **On-Chain Action Integration:** The service will be connected to the Bitcoin node/network to actually post transactions when needed. For instance, if a fraud is detected at step 57 of the computation, the ThunderStack service could automatically construct the Bitcoin transaction with the proof (perhaps revealing the bad step) and broadcast it to penalize the Prover. Conversely, if ThunderStack is the Prover and someone else challenges, the service will automatically respond with the next step in the halving dispute (BitVM uses a binary search type dispute resolution).
* **Scalability & Performance:** Running a BitVM Prover could be heavy if the computations are large. ThunderStack will optimize this by running on powerful servers, possibly using parallelism or specialized techniques. This means even complex programs can be handled within the time limits of challenges.
* **Auditability:** Possibly, ThunderStack might allow third parties read-only access to certain transcripts via the API, so multiple observers can independently verify. This transparency builds trust that the Prover service is doing its job correctly.

</details>

<details>

<summary><strong>How do you integrate or use the BitVM Prover &#x26; Observer API?</strong></summary>



Using this service would involve:

* **Defining your BitVM contract/logic:** First, you have to have a BitVM-compatible contract. This usually means you have a specific Bitcoin transaction setup that locks funds with a script that enforces the BitVM game (one party can spend if they provide a valid result or both can spend if they agree, etc., and the dispute mechanism is in place). You also have the off-chain code that represents the computation.
* **Registering the computation with ThunderStack:** You’d tell ThunderStack service about your BitVM instance. This could be via API: you’d provide the details like the initial commitment (hash) of the computation, identities of Prover and Verifier (could be keys or identifiers), and perhaps the actual program or a reference to it. If ThunderStack is acting as Prover, you’d provide the program and inputs. If ThunderStack is just observing, maybe you give it the initial commitment and it fetches details from the blockchain.
* **Using Prover Service:** If ThunderStack is the Prover, once it knows the program and inputs, you call an API like `/execute` to run the computation. The service returns the final result and the series of commitments (transcript). It also likely starts a timer for the dispute window and informs the Observation system.
* **Observation/Challenge:** If you (or your users) are the Verifier, you could use the dashboard to watch what ThunderStack (as Prover) computed. If something looks wrong (or automatically, if an independent verification finds a mismatch), you’d call the challenge API. That might be as simple as specifying which step or which output you dispute. ThunderStack service (or another party) then prepares the Bitcoin transaction to escalate the dispute on-chain.
* **Automated Workflow:** In many cases, you might set it up so that ThunderStack is both proving and verifying (with separate internal agents or in cooperation with you). In such a case, if the Prover service cheats, the verifier agent will catch it and publish the proof on-chain, slashing the Prover’s bond (if BitVM is used in a rollup, typically the operator’s bond is lost on fraud). If the Prover is honest, no action is needed and after the challenge period, the result stands.
* **API for State:** During and after computation, you can query the API for outcomes. For example, a rollup operator after challenge period might query “was batch #123 finalized or was there fraud?” The API would respond with final status.
* **Integration with Rollup Node:** If you run a rollup node, you might integrate the BitVM service so that whenever you produce a new block/batch, you automatically send the computation to ThunderStack Prover, get the result (which could be a new state root), and then post that result in a Bitcoin transaction. The BitVM service would then watch for any challenges. Essentially your rollup software delegates the proof game to ThunderStack.

</details>

<details>

<summary><strong>What about security and trust when using ThunderStack’s BitVM service?</strong></summary>

This is crucial:

* **Non-Custodial for Funds:** Ideally, even though ThunderStack helps in the protocol, it should not hold your funds. In a BitVM setup, usually the Prover has to put a bond in a Bitcoin transaction, and the Verifier might have to as well, to ensure they don't spam challenges. You or your smart contract on Bitcoin would handle locking those funds. ThunderStack service just works with the data and can craft transactions for you to sign or you give it keys with limited rights (maybe a multi-sig where ThunderStack can co-sign challenges but not move funds arbitrarily). There needs to be a clear separation so ThunderStack cannot run away with funds; it can only help publish correct or incorrect proofs.
* **Trust in Computation:** If you’re using ThunderStack as the Prover, you are trusting it to perform the computation honestly _off-chain_. But because of how BitVM is designed, if it tries to lie about the result, any honest Verifier can catch it. So trust comes from the fact that cheating can be proven on-chain. You either trust ThunderStack not to cheat (for convenience) or you (or someone) verify behind it to keep it honest. In many cases, the party opposite ThunderStack in BitVM (the Verifier) might be your own agent or a third-party auditor.
* **Timeliness:** One thing to trust is that ThunderStack will respond within the required time windows. BitVM disputes might require responses within a certain number of Bitcoin blocks. By using a service, you offload that responsibility to them – they must be online and attentive. ThunderStack will have high-availability systems to ensure if a challenge comes at 3am, it responds in time. Using a service is actually beneficial here, because a single individual might miss a challenge window, but a service with 24/7 uptime won’t.
* **Privacy of Computation:** Off-chain computations might be private. If you give ThunderStack your program and data, you might be revealing some logic or info to them. If that’s sensitive (for instance, if the computation involves some secret data), you’d need to consider that. Possibly, ThunderStack could support ZK verification such that it doesn’t need the raw data (this is advanced; BitVM itself is interactive, but maybe in future a zero-knowledge element could hide data). For now, assume the service sees whatever it’s proving. If privacy is needed, maybe you wouldn’t outsource that computation.
* **Audit Logs:** ThunderStack will likely log all actions it takes on your behalf. You might be able to get those logs to audit that the service acted correctly. For example, if it posted a transaction because of a challenge, you’ll see the details. This transparency helps build trust that it’s doing only what it’s supposed to.
* **Updates to BitVM:** BitVM is still new – as it evolves (or if a better scheme comes along), ThunderStack service will update. They might sandbox each BitVM protocol version separately to avoid any unexpected interactions.

In summary, the BitVM Prover & Observer service allows you to leverage Bitcoin’s security for arbitrary computations without running all the infrastructure yourself. It’s a complex but powerful tool, and ThunderStack offering is to handle that complexity in a reliable, developer-friendly way.

</details>

<details>

<summary><strong>Is the BitVM Prover &#x26; Observation Dashboard available now?</strong></summary>

This service is on the ThunderStack roadmap. BitVM itself is experimental, so ThunderStack is likely building and testing this with early partners (perhaps in private beta). If you are keen, you might reach out to Thunderstack for early access or updates. They will announce publicly once it’s production-ready. Expect it to roll out as Bitcoin rollup tech matures, possibly in stages (alpha testing with specific rollups, then broader release). Meanwhile, developers can familiarize themselves with BitVM concepts and even try out Thunderstack’s RPC on BitVM-integrated networks (like Bitlayer) which is a stepping stone to the full prover service.

</details>



### General Questions

<details>

<summary><strong>What is ThunderStack in a nutshell?</strong></summary>

**ThunderStack** is a unified platform for Bitcoin layer-2 infrastructure. It simplifies deploying and managing the various “layers” built on top of Bitcoin. This includes the **Bitcoin-native layers** like the Lightning Network and RGB (where users keep custody of funds and use Bitcoin’s security model off-chain), and **Bitcoin side systems** like sidechains and rollups (which are separate networks anchored to Bitcoin’s security or value). ThunderStack offers cloud-hosted services: you can spin up Lightning/RGB nodes (ThunderCloud), integrate Lightning payments easily (ThunderEngine), get liquidity (ThunderFlow), backup states (ThunderSafe), accept payments (ThunderLink), and connect to sidechains (ThunderStack RPC) all under one roof. The goal is to let developers and businesses adopt these technologies without having to become experts in each one’s DevOps. Thunderstack takes care of hosting nodes, providing high availability, robust APIs, and security, so you can focus on building your application or business logic.

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
* **Sidechains (BTC Sidechains):** These are separate blockchains that use BTC (or pegged BTC) as their currency, e.g. Rootstock (RSK), Liquid (possibly, if supported in future), and others. Thunderstack RPC provides connectivity to these chains. Essentially, if a chain is Bitcoin-adjacent (either merge-mined, pegged, or otherwise linked), ThunderStack aims to support it.
* **Rollups and Experimental Layers:** This includes things like **BitVM rollups** or drivechains. While these are emerging, ThunderStack support for them is either active (if networks exist) or planned. The mention of BitVM and rollups in their materials indicates they’re building for the future where Bitcoin could have optimistic or ZK-rollups posting data to L1.
* **Other protocols (if any):** If Bitcoin layer-2 expands beyond these (e.g., Ark protocol or federated systems like Fedimint), ThunderStack could potentially support those as well, though they haven’t been explicitly mentioned. The concept of “Bitcoin layers” is broad, and ThunderStack mission is to be the go-to infrastructure provider for any promising Bitcoin layer.

</details>

<details>

<summary><strong>Is ThunderStack non-custodial? Do I retain control of my funds and keys?</strong></summary>

ThunderStack strives to enable non-custodial use wherever possible:

* **RGB:** RGB is inherently client-side, meaning the state and secret data for assets reside with the user. Thunderstack’s infrastructure helps transport and manage state, but the authority to create a valid state transition (like spending an asset) lies with your keys. ThunderStack doesn’t get those secrets – it just relays the data.
* **ThunderSafe:** It’s designed to be trustless (encrypted backups) ThunderStack stores your data but cannot read or misuse it without your keys.
* **ThunderLink payments:** When you receive payments, ThunderStack does take custody of those funds on your behalf at the moment of receipt (since the payment goes to a node they control). However, those are your funds, and you can withdraw them. Think of it like how a payment processor holds funds briefly. For maximum trustlessness, you could run your own Lightning node and just use ThunderLink for invoice generation, but that loses some convenience. Thunderstack likely isolates merchant funds and lets you withdraw regularly to your on-chain wallet or a Lightning channel you control.
* **Sidechain/RPC usage:** When using ThunderStack RPC, you might be sending signed transactions. Thunderstack isn’t taking custody; you control the keys that sign those transactions. They’re just a relay. If you use their service to manage an account on a sidechain (some providers offer wallet-as-a-service), that could be custodial – but ThunderStack hasn’t indicated they do that. It’s mostly an access service.
* **BitVM Prover Service:** If ThunderStack is acting as Prover, there might be a scenario where it has to co-sign something or hold a bond for you. Ideally, you’d send your bond to a multi-sig or smart contract that ensures if Thunderstack misbehaves it gets slashed, otherwise you get it back. That’s a form of _ensured_ non-custodial behavior: they can’t steal your bond, only lose it if they cheat. More details will depend on implementation.

In summary, ThunderStack is built to let you keep control of the important keys. In cases where they do hold something (like funds on a Lightning node or sidechain node), it’s typically because you’ve opted for that convenience or because technically someone must manage the hot wallet for the service (like an LSP node or a sidechain account). Even then, it’s compartmentalized and you can minimize trust by frequent withdrawals or by hybrid approaches (like hold a multi-sig key). Always refer to specific service documentation to understand custody implications

</details>









