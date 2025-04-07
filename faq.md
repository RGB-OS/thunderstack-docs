# FAQ

This document is organized into major sections for **Bitcoin-Native Infrastructure** (covering Lightning and RGB-based tools) and **Bitcoin Sidechains** (covering sidechain and rollup connectivity).

### Bitcoin-Native Infrastructure

_Bitcoin-native protocols are those built on Bitcoin where users retain custody of their funds and can always fall back to the Bitcoin base layer. Thunderstack’s Bitcoin-Native Infrastructure provides managed services for such protocols, including the RGB smart-contract protocol and the Lightning Network._

* **What is ThunderCloud?**\
  **ThunderCloud** is a cloud-based solution designed for seamless node management of **RGB** and RGB Lightning Network applications. It provides one-click deployment and hosting of full **RGB Lightning Nodes (RLN)**, which combine Bitcoin Lightning Network functionality with RGB asset support. ThunderCloud ensures high availability, enhanced security, and ease of use for running Lightning/RGB nodes in the cloud.
* **How do I integrate ThunderCloud into my application?**\
  Integration methods:
  * **Web Dashboard:** You can manage nodes via a user-friendly dashboard (create, monitor, and control your RLN instances with a few clicks).
  * **API Access:** For programmatic control, ThunderCloud exposes RESTful endpoints (and client libraries) so your application can spin up or shut down nodes, query node info, and even initiate RLN operations via API.
  * **Webhooks:** Set up webhooks to get real-time callbacks to your app on important events (like a channel opened or afer) for seamless automation.
*   **What security and reliability measures does ThunderCloud have?**\
    Security and reliability are top priorities for ThunderCloud:

    [https://docs.thunderstack.org/bitcoin-native-infrastructure/readme/getting-started-with-thunderstack-rgb-cloud/security](https://docs.thunderstack.org/bitcoin-native-infrastructure/readme/getting-started-with-thunderstack-rgb-cloud/security)
* **Does ThunderCloud provide an API ?**\
  Yes. Everything in ThunderCloud is controllable via its **API** and web interfaces:
  * The **ThunderCloud API** allows full control of nodes
  * There is a **Node API (RLN API)** for the node itself. Essentially, they can be operated via REST JSON APIs. [RLN API docs](bitcoin-native-infrastructure/readme/getting-started-with-thunderstack-rgb-cloud/rgb-lightning-node-api.md)



### Bitcoin Sidechains Infrastructure

_Bitcoin sidechains (and similar “side systems”) are independent blockchains that are pegged to Bitcoin or use BTC as their native asset. Rollups are another type of layer that post data to Bitcoin. ThunderStack’s sidechain infrastructure provides connectivity to these systems so developers can interact with them without running full nodes themselves._

#### Thunderstack RPC (Sidechain/Rollup Connectivity)

* **What is Thunderstack RPC?**\
  **Thunderstack RPC** is a cloud-based RPC (Remote Procedure Call) service that gives you instant API access to various Bitcoin-aligned sidechains and layer-2 networks. In essence, it’s a **hosted node service**: instead of running your own node for a Bitcoin sidechain or rollup, you can use Thunderstack’s RPC endpoints to read and submit transactions to those networks. It works over HTTPS with standard JSON-RPC calls, acting as a gateway to blockchain nodes. Thunderstack RPC currently focuses on **Bitcoin EVM-compatible layers** and will extend to other emerging Bitcoin rollup tech. Using Thunderstack RPC means you don’t have to worry about syncing nodes, scalability, or maintenance – you get reliable access with a simple web API key.
* **Who should use Thunderstack RPC?**\
  This service is ideal for developers and teams who:
  * Need to interact with Bitcoin sidechains or rollups as part of their application (querying data or broadcasting transactions), but do not want the hassle of running full nodes for each network.
  * Are building multi-chain dApps or services where Bitcoin sidechains are involved (for example, an application on RSK, or a DeFi platform that utilizes a Bitcoin drivechain or rollup).
  * Want a unified, high-performance API for multiple networks. If your app touches many chains, keeping all those nodes updated and running is complex – Thunderstack RPC provides one platform to access many.
  * Require reliable uptime and scaling for RPC. If your project scales to many users making RPC calls, Thunderstack can handle the load balancing and scaling, whereas a self-hosted node might become a bottleneck.
  * Are exploring new Bitcoin layer-2 tech (like BitVM-based rollups) without committing resources to run each experimental node.
*   **Which networks can I access with Thunderstack RPC?**\
    Thunderstack RPC supports a **diverse range of Bitcoin sidechains and related networks**, all chosen for their alignment with Bitcoin’s security model. The list is continually growing. As of now, supported networks include:

    * **Rootstock (RSK):** A Bitcoin sidechain that is EVM-compatible, merge-mined with Bitcoin, using BTC pegged tokens. RSK allows Ethereum-like smart contracts with Bitcoin security, and Thunderstack provides full RPC access to it.
    * **Bitlayer & Others:** Emerging BitVM-powered networks like _Bitlayer_ itself, which integrate Bitcoin via BitVM (allowing Bitcoin to be used in multi-chain DeFi)【42†L580-L589】. Thunderstack supports their RPC so you can interact with cross-chain bridges and smart contracts involving Bitcoin.
    * **Strata, Citrea, BitcoinOS, Bsquared, GOAT, Plasma, Ola, Zulu, BOB, Merlin, CoreDAO, zkLayer, LayerEdge, Bitfinity, Babylon, Anduro, etc.:** These are various experimental or new networks (some might be rollups, some sidechains) that Thunderstack is supporting. Each of these has its own focus (for instance, Babylon focuses on Bitcoin-finalized consensus for other chains, etc.). By using Thunderstack RPC, you can access any of them with the same API approach.
    * **Mainnet vs Testnet:** Many of these networks have testnets; Thunderstack likely offers endpoints for testnets and regtest/dev networks as well, so you can develop and test easily.

    [_ThunderStack documentation_](bitcoin-sidesystems-infrastructure/products/thunder-rpc/supported-networks.md) _and dashboard will have the exact up-to-date list of supported networks, including chain IDs or identifiers you’ll use in the API to select a network._
* **What about security and reliability for the RPC service?**\
  From a security standpoint:
  * **Transport Security:** All endpoints are HTTPS, meaning the data (your RPC calls and responses) are encrypted in transit. No one can snoop on your queries or responses.
  * **Authentication:** The API key ensures only authorized users make requests. Internally, Thunderstack’s _eRPC proxy_ infrastructure assigns your requests to your account and monitors usage. If someone stole your key, they could use your quota, so it’s important to keep it secret.
  * **Isolation:** Each user’s requests run through an isolated proxy instance (as mentioned, for every API key, Thunderstack launches an eRPC proxy that routes to the appropriate node cluster). This isolation means even if one user overloads the system or triggers an issue, it shouldn’t affect others – their traffic is segmented.
  * **Consensus and Data Integrity:** Because you’re not running the node, you trust ThunderStack nodes to give correct data. ThunderStack uses the official client software for each network and connects to the genuine peer-to-peer network, ensuring authentic data (e.g., they run RSK nodes that follow the RSK consensus). They likely monitor for chain reorganizations or issues and might use multiple nodes for the same network to cross-verify responses.
  * **Uptime:** Thunderstack will have monitoring and failover for node clusters. If a node goes down, your calls get rerouted to another. They probably maintain near 99.9% uptime for the RPC endpoints, suitable for production services.
  * **Rate Limiting and Abuse Prevention:** To maintain reliability, there are quotas (both daily usage via **Compute Units (CUs)** and per-second rate limits via **CUPS**) for each plan. This prevents any single user from overwhelming the service and ensures fair usage. If you hit limits, you’ll get error responses, but the service remains stable for everyone.
  * **Updates and Compatibility:** ThunderStack keeps the nodes updated to the latest stable versions, meaning you get all the latest protocol features and security patches. If any breaking changes occur (say a network upgrade that changes the RPC API), Thunderstack will handle the upgrade and notify users if any action is needed.
* **How is usage measured and what are Compute Units (CUs) and CUPS?**\
  Thunderstack uses a credit system to measure RPC usage:
  * **Compute Units (CUs):** A Compute Unit is a unit of measure that represents the computational/IO cost of a given RPC call. Not all RPC calls are equal – ThunderStack assigns a CU price to each RPC method. Your account has a **daily CU quota** (depending on your plan). Every time you make a request, the corresponding CU cost is deducted from your daily allotment. This way, you’re only paying (or limited) by actual usage rather than a flat number of requests. It encourages efficient use of calls. For details refer to [docs](bitcoin-sidesystems-infrastructure/products/thunder-rpc/compute-units-cus.md)
  * **CUPS (Calls (or Compute Units) Per Second):** This likely refers to the rate limit. Even if you have a large daily CU quota, you can’t use it all at once; there’s a per-second cap to prevent bursts from overloading the system. For example, your plan might allow, say, 100 CUs per second. If one RPC call costs 10 CUs, that’s effectively 10 calls per second of that type. If you exceed, subsequent calls might be throttled or rejected until the next second. This ensures fairness and stability. For details refer to [docs](bitcoin-sidesystems-infrastructure/products/thunder-rpc/cups-rate-limit.md)
  * **Monitoring CUs:** The Thunderstack dashboard will show how many CUs you’ve used and how many remain today, so you can monitor if you’re approaching limits.&#x20;
  * **Efficiency:** The CU system encourages you to avoid extremely expensive calls or at least be mindful of them. For example, instead of requesting full data repeatedly, you might store results or use filters to narrow queries – saving your CU budget.
  * **Unused Quota:** Typically, unused daily CUs don’t carry over (each day it resets according to plan). If you need more on a particular day, you might upgrade your plan or contact support for a bump.

#### Future Support for Bitcoin Rollups RPC Infrastructure

* **What are Bitcoin rollups and will Thunderstack support them?**\
  **Bitcoin rollups** are a new class of layer-2 solutions where transaction data is compressed and posted on Bitcoin (L1) for security, allowing higher throughput and smart contracts without changing Bitcoin’s rules. They often rely on validity proofs (like SNARKs) or fraud proofs to ensure correctness. Examples of early concepts include **BitVM** and various zk-rollup proposals that use Bitcoin for data and arbitration. ThunderStack is closely following these developments and has architecture ready to support rollup RPC endpoints as they become available. In fact, networks like **Bitlayer** which ThunderStack already supports can be seen as early Bitcoin rollup implementations using BitVM technology. As more Bitcoin rollup frameworks mature (e.g. rollups, drivechains functioning as rollups, or other sidechains that post state to Bitcoin), ThunderStack will integrate them into the RPC service. The goal is to make ThunderStack RPC the go-to hub for interacting with _all_ Bitcoin layer-2 networks, including true rollups.
* **How will ThunderStack integrate future rollup infrastructure?**\
  ThunderStack infrastructure is built to be adaptable:
  * They will run the necessary **rollup nodes** or clients on the backend. If a new rollup chain launches (say an Optimistic Rollup anchored in Bitcoin), ThunderStack would run a full node or sequencer client for that rollup.
  * The **RPC interface** would likely mimic whatever the rollup node’s API is. If the rollup is EVM-like, it might just appear as another EVM chain endpoint. If it’s something new, ThunderStack will provide documentation on how to call it.
  * **BitVM-based rollups:** BitVM is a concept enabling off-chain computation with Bitcoin as the enforcer. Should a rollup use BitVM, ThunderStack might need to run special software (like a BitVM prover/verifier node — more on that in the BitVM section below) to support it. They will incorporate those so developers can, for example, submit transactions to a BitVM rollup via a ThunderStack endpoint.
  * **Data Availability (DA) and Proof Services:** Some rollups might require separate Data Availability layers or proof submission endpoints. ThunderStack could offer integrated services for those as well (so not only the RPC to interact with the chain, but maybe an API to fetch proofs or status of proofs if that’s part of interacting with the rollup).
  * **Continuous Updates:** Rollup tech is evolving, so ThunderStack will update supported protocols as needed (if a rollup changes its proving system, etc.). By using ThunderStack, developers can avoid constantly updating their own nodes for each upgrade – ThunderStack handles it and maintains backward compatibility as much as possible.
  * **Unified Experience:** As with sidechains, ThunderStack will aim to keep the experience uniform. Whether you’re dealing with a sidechain or a rollup, you’ll likely manage it similarly in the Thunderstack dashboard, with similar API patterns, just specifying a different network. This means learning curve for one network translates to others easily on the platform.
*   **Do I need to change anything to use future rollups on Thunderstack?**\
    If you’re already using Thunderstack RPC, adding a new rollup might be as simple as:

    * Enabling the new network for your API key (some networks might be restricted during beta or require you to opt-in).
    * Using the new network’s identifier in your RPC calls.
    * Adjusting for any unique RPC methods that network has (Thunderstack will document any differences).

    For example, if a ZK-Rollup called “ZKLayer1” goes live and Thunderstack supports it, you might see it appear in the “Supported Networks” list. You could then take your dApp that uses, say, Ethereum RPC, and point it at “ZKLayer1” via Thunderstack RPC to deploy contracts or send transactions on it (assuming it’s EVM-like). If the rollup uses Bitcoin addresses and script semantics instead (a different model), Thunderstack would provide an API for those operations, possibly a bit different from JSON-RPC (or encapsulated in JSON-RPC calls).

    Thunderstack will likely publish guides for each new major rollup they support, detailing how to connect and any changes from existing methods.
*   **How does Thunderstack handle the security and trust aspects of rollups?**\
    One important aspect: some rollups may run centralized sequencers or be in early stages. Thunderstack providing access to them doesn’t change their trust model (e.g., if a rollup has a centralized sequencer, using Thunderstack doesn’t decentralize it). However, Thunderstack will:

    * Vet the networks it supports to some degree – ensuring they are not outright scams and have some community trust or backing.
    * Possibly label **experimental networks** as such, so developers understand the risk.
    * For user clarity, they might highlight whether a network is a true trustless rollup or something like a federated sidechain, so you know what you’re dealing with.
    * Continue to ensure the RPC data is relayed correctly. In a rollup with fraud proofs, for example, Thunderstack’s node will show the state as is, but finality might be deferred until a challenge period passes. Thunderstack might provide that info (like a flag if a transaction is still in a challenge window).
    * If a rollup submits proofs to Bitcoin, Thunderstack could cross-verify events with Bitcoin’s chain (for instance, confirming that a rollup epoch is finalized only after the Bitcoin transaction including its proof is confirmed). This could be extra logic they add to ensure developers get accurate read on finality.

    Ultimately, Thunderstack aims to simplify usage, but devs should still understand the underlying trust model of whichever rollup they use (e.g., an optimistic rollup has a fraud window; a ZK-rollup is instant finality if proofs verify; a sidechain might be fully trustful with a federation, etc.).
*   **Is there a roadmap or timeline for Thunderstack’s rollup support?**\
    While specifics would come from Thunderstack’s announcements, generally:

    * **Near-term:** Support for networks that are already out or nearly out (BitVM test networks, perhaps Ark or Drivechain experiments if any, etc.).
    * **Mid-term:** As Bitcoin’s ecosystem possibly soft-forks to enable more rollup friendliness (for example, if Covenants or CTV or some changes come that make rollups easier), Thunderstack will likely support any new layers that pop up utilizing those.
    * **BitVM Prover Service:** (Segue to next topic) Thunderstack is actively working on BitVM support, which is one of the key components to enabling rollups on Bitcoin without a soft fork. This suggests they’re preparing the infrastructure needed for those kinds of networks.
    * They might also collaborate with emerging projects (the press release about Bitlayer suggests Thunderstack being aligned with that ecosystem).

    In short, as the Bitcoin community develops the rollup concept, Thunderstack plans to be at the forefront of providing infrastructure for it. Keep an eye on their blog or updates for new additions.

#### BitVM Prover & Observation Dashboard/API

*   **What is BitVM, and what is Thunderstack’s BitVM Prover & Observation Dashboard?**\
    **BitVM** is a proposed framework (introduced in late 2023 by Robin Linus and others) that allows for Turing-complete computations to be verified via the Bitcoin blockchain, _without changing Bitcoin’s consensus_. It’s like enabling smart contracts using Bitcoin as the judge in a dispute. BitVM involves two parties (a Prover and a Verifier); the Prover performs some computation off-chain and the Verifier can challenge if they disagree, with Bitcoin scripts used to resolve disputes. This can be used to build Bitcoin rollups or advanced contracts (often categorized under “validity rollups” or “optimistic rollups” in Bitcoin context).

    Thunderstack’s **BitVM Prover & Observation Dashboard/API** is an upcoming service aimed at supporting this BitVM paradigm. Essentially, Thunderstack wants to provide the heavy lifting infrastructure for BitVM-based applications:

    * The **Prover** part refers to running the computations and generating the transcripts/proofs that would be needed if challenged. Thunderstack can operate Prover nodes on behalf of users or developers, so you don’t need to have a powerful server constantly running the computations.
    * The **Observation Dashboard** refers to a monitoring interface (dashboard) and API for the Verifier side (or observers in the system). This would track the state of BitVM computations, whether any disputes have been raised, and the status of on-chain commitments. It’s essentially a control center to observe and interact with BitVM sequences.

    In simpler terms, if you build a rollup or application using BitVM, Thunderstack’s service would let you:

    * Submit your computations to be executed by a robust Prover service.
    * Monitor the execution and outcome via a dashboard.
    * Rely on Thunderstack to watch the Bitcoin blockchain for any needed actions (like posting a fraud proof or responding to a challenge within the allotted time).
* **Who would use the BitVM Prover service and why?**\
  This is targeted at advanced Bitcoin developers and businesses working on Layer-2 solutions like rollups or complex smart contracts:
  * **Rollup Operators:** If you are running a Bitcoin rollup that uses BitVM for proving state transitions, you need to continuously produce proofs for each batch of transactions and be ready to respond to disputes. Thunderstack’s Prover service can take on that role, ensuring proofs are generated quickly and correctly.
  * **Watchtowers/Observers:** These are entities that watch for invalid behavior. The Observation API would enable them to easily get data and possibly react. For example, a watchtower service could use Thunderstack’s API to get the transcript of a BitVM computation and automatically detect a fraud, then use Thunderstack to submit the fraud proof on-chain.
  * **Developers experimenting with BitVM:** If you are building an app like a DLC (Discrete Log Contract) or some complex conditional payment that leverages BitVM technique, you might use the Thunderstack Prover to simulate or actually execute the computation part and verify outcomes, without having to implement the entire verification game logic from scratch.
  * **Businesses needing off-chain compute with Bitcoin settlement:** Imagine a company wants to run some logic (like a random draw, or an auction) off-chain but have the final result enforced by Bitcoin. They could define it in a BitVM-compatible way. The Thunderstack Prover would execute the logic, and everyone can trust the outcome because if the Prover tried to cheat, it’d get caught on-chain. So businesses can offload the compute to Thunderstack while maintaining trust through Bitcoin.
* **What does the BitVM Prover & Observation service actually do (features)?**\
  Key components and features likely include:
  * **BitVM Execution Engine:** Thunderstack will run an engine that takes a BitVM program (the set of instructions for the off-chain computation) and executes it as the Prover would. It produces the cryptographic transcript of the execution (the sequence of hashes or commitments that summarize each step of computation). This transcript is what a Verifier would use to challenge a specific step if they disagree.
  * **Automated Verifier Response:** Alongside proving, Thunderstack’s system can act as a Verifier or assist external Verifiers. For example, if Thunderstack is proving on behalf of a rollup operator, the users (Verifiers) might run their own nodes or trust a third-party. However, Thunderstack could also provide a service to do the verification and challenge on behalf of users who delegate that role (similar to a watchtower concept). This could be offered as a service: Thunderstack monitors and if its own Prover (or any Prover) cheats, Thunderstack will call it out on-chain.
  * **Dashboard UI:** A web interface where you can see all ongoing BitVM sessions. For each BitVM contract or rollup:
    * See the current step of execution, the outputs produced, etc.
    * See the time left in challenge periods.
    * Buttons or controls to manually initiate a challenge or respond to one (if for some reason automation is off or you want manual control).
    * Logs of what has happened – e.g., “Computation for batch #123 completed off-chain, awaiting challenge period (24h remaining).” or “No fraud detected. Batch #123 finalized.”
    * Visualizations: possibly an instruction-by-instruction view or at least summary of computation results for easier analysis.
  * **Observation API:** All the data in the dashboard likely is available via an API. Developers can query the status of a computation, subscribe to events (like “challenge filed” or “proof posted”), and maybe retrieve parts of the transcript or state.
  * **On-Chain Action Integration:** The service will be connected to the Bitcoin node/network to actually post transactions when needed. For instance, if a fraud is detected at step 57 of the computation, the Thunderstack service could automatically construct the Bitcoin transaction with the proof (perhaps revealing the bad step) and broadcast it to penalize the Prover. Conversely, if Thunderstack is the Prover and someone else challenges, the service will automatically respond with the next step in the halving dispute (BitVM uses a binary search type dispute resolution).
  * **Scalability & Performance:** Running a BitVM Prover could be heavy if the computations are large. Thunderstack will optimize this by running on powerful servers, possibly using parallelism or specialized techniques. This means even complex programs can be handled within the time limits of challenges.
  * **Auditability:** Possibly, Thunderstack might allow third parties read-only access to certain transcripts via the API, so multiple observers can independently verify. This transparency builds trust that the Prover service is doing its job correctly.
* **How do you integrate or use the BitVM Prover & Observer API?**\
  Using this service would involve:
  * **Defining your BitVM contract/logic:** First, you have to have a BitVM-compatible contract. This usually means you have a specific Bitcoin transaction setup that locks funds with a script that enforces the BitVM game (one party can spend if they provide a valid result or both can spend if they agree, etc., and the dispute mechanism is in place). You also have the off-chain code that represents the computation.
  * **Registering the computation with Thunderstack:** You’d tell Thunderstack’s service about your BitVM instance. This could be via API: you’d provide the details like the initial commitment (hash) of the computation, identities of Prover and Verifier (could be keys or identifiers), and perhaps the actual program or a reference to it. If Thunderstack is acting as Prover, you’d provide the program and inputs. If Thunderstack is just observing, maybe you give it the initial commitment and it fetches details from the blockchain.
  * **Using Prover Service:** If Thunderstack is the Prover, once it knows the program and inputs, you call an API like `/execute` to run the computation. The service returns the final result and the series of commitments (transcript). It also likely starts a timer for the dispute window and informs the Observation system.
  * **Observation/Challenge:** If you (or your users) are the Verifier, you could use the dashboard to watch what Thunderstack (as Prover) computed. If something looks wrong (or automatically, if an independent verification finds a mismatch), you’d call the challenge API. That might be as simple as specifying which step or which output you dispute. Thunderstack’s service (or another party) then prepares the Bitcoin transaction to escalate the dispute on-chain.
  * **Automated Workflow:** In many cases, you might set it up so that Thunderstack is both proving and verifying (with separate internal agents or in cooperation with you). In such a case, if the Prover service cheats, the verifier agent will catch it and publish the proof on-chain, slashing the Prover’s bond (if BitVM is used in a rollup, typically the operator’s bond is lost on fraud). If the Prover is honest, no action is needed and after the challenge period, the result stands.
  * **API for State:** During and after computation, you can query the API for outcomes. For example, a rollup operator after challenge period might query “was batch #123 finalized or was there fraud?” The API would respond with final status.
  * **Integration with Rollup Node:** If you run a rollup node, you might integrate the BitVM service so that whenever you produce a new block/batch, you automatically send the computation to Thunderstack’s Prover, get the result (which could be a new state root), and then post that result in a Bitcoin transaction. The BitVM service would then watch for any challenges. Essentially your rollup software delegates the proof game to Thunderstack.
*   **What about security and trust when using Thunderstack’s BitVM service?**\
    This is crucial:

    * **Non-Custodial for Funds:** Ideally, even though Thunderstack helps in the protocol, it should not hold your funds. In a BitVM setup, usually the Prover has to put a bond in a Bitcoin transaction, and the Verifier might have to as well, to ensure they don't spam challenges. You or your smart contract on Bitcoin would handle locking those funds. Thunderstack’s service just works with the data and can craft transactions for you to sign or you give it keys with limited rights (maybe a multi-sig where Thunderstack can co-sign challenges but not move funds arbitrarily). There needs to be a clear separation so Thunderstack cannot run away with funds; it can only help publish correct or incorrect proofs.
    * **Trust in Computation:** If you’re using Thunderstack as the Prover, you are trusting it to perform the computation honestly _off-chain_. But because of how BitVM is designed, if it tries to lie about the result, any honest Verifier can catch it. So trust comes from the fact that cheating can be proven on-chain. You either trust Thunderstack not to cheat (for convenience) or you (or someone) verify behind it to keep it honest. In many cases, the party opposite Thunderstack in BitVM (the Verifier) might be your own agent or a third-party auditor.
    * **Timeliness:** One thing to trust is that Thunderstack will respond within the required time windows. BitVM disputes might require responses within a certain number of Bitcoin blocks. By using a service, you offload that responsibility to them – they must be online and attentive. Thunderstack will have high-availability systems to ensure if a challenge comes at 3am, it responds in time. Using a service is actually beneficial here, because a single individual might miss a challenge window, but a service with 24/7 uptime won’t.
    * **Privacy of Computation:** Off-chain computations might be private. If you give Thunderstack your program and data, you might be revealing some logic or info to them. If that’s sensitive (for instance, if the computation involves some secret data), you’d need to consider that. Possibly, Thunderstack could support ZK verification such that it doesn’t need the raw data (this is advanced; BitVM itself is interactive, but maybe in future a zero-knowledge element could hide data). For now, assume the service sees whatever it’s proving. If privacy is needed, maybe you wouldn’t outsource that computation.
    * **Audit Logs:** Thunderstack will likely log all actions it takes on your behalf. You might be able to get those logs to audit that the service acted correctly. For example, if it posted a transaction because of a challenge, you’ll see the details. This transparency helps build trust that it’s doing only what it’s supposed to.
    * **Updates to BitVM:** BitVM is still new – as it evolves (or if a better scheme comes along), Thunderstack’s service will update. They might sandbox each BitVM protocol version separately to avoid any unexpected interactions.

    In summary, the BitVM Prover & Observer service allows you to leverage Bitcoin’s security for arbitrary computations without running all the infrastructure yourself. It’s a complex but powerful tool, and Thunderstack’s offering is to handle that complexity in a reliable, developer-friendly way.
* **Is the BitVM Prover & Observation Dashboard available now?**\
  As of the latest information, this service is on the Thunderstack roadmap. BitVM itself is experimental, so Thunderstack is likely building and testing this with early partners (perhaps in private beta). If you are keen, you might reach out to Thunderstack for early access or updates. They will announce publicly once it’s production-ready. Expect it to roll out as Bitcoin rollup tech matures, possibly in stages (alpha testing with specific rollups, then broader release). Meanwhile, developers can familiarize themselves with BitVM concepts and even try out Thunderstack’s RPC on BitVM-integrated networks (like Bitlayer) which is a stepping stone to the full prover service.

### General Questions

* **What is Thunderstack in a nutshell?**\
  **Thunderstack** is a unified platform for Bitcoin layer-2 infrastructure. It simplifies deploying and managing the various “layers” built on top of Bitcoin. This includes the **Bitcoin-native layers** like the Lightning Network and RGB (where users keep custody of funds and use Bitcoin’s security model off-chain), and **Bitcoin side systems** like sidechains and rollups (which are separate networks anchored to Bitcoin’s security or value). Thunderstack offers cloud-hosted services: you can spin up Lightning/RGB nodes (ThunderCloud), integrate Lightning payments easily (ThunderEngine), get liquidity (ThunderFlow), backup states (ThunderSafe), accept payments (ThunderLink), and connect to sidechains (Thunderstack RPC) all under one roof. The goal is to let developers and businesses adopt these technologies without having to become experts in each one’s DevOps. Thunderstack takes care of hosting nodes, providing high availability, robust APIs, and security, so you can focus on building your application or business logic【22†L100-L107】.
* **How is Thunderstack different from running my own nodes or infrastructure?**\
  Running your own Bitcoin, Lightning, or sidechain nodes means dealing with hardware, uptime, networking, and updates yourself. Thunderstack abstracts that away as a managed service:
  * **Ease of Use:** Via web interfaces and APIs, you accomplish in minutes what might take hours or days to set up on your own (e.g., configuring a Lightning node with channels and liquidity).
  * **Expert Management:** Thunderstack’s team maintains the nodes (applying security patches, monitoring for issues 24/7). This reduces the risk of downtime or attacks compared to a DIY node that might be left unmonitored.
  * **Integrated Ecosystem:** All Thunderstack services are designed to work together. For instance, a node from ThunderCloud can easily use ThunderFlow to get a channel, and ThunderSafe will automatically back it up. If you did it all yourself, you’d have to integrate different software pieces manually.
  * **Scalability:** With Thunderstack, if your project suddenly needs to scale (more nodes, more RPC calls, more throughput), you can upgrade your plan or add resources quickly. Doing it yourself might mean ordering new servers or spending time re-architecting.
  * **Cost Efficiency:** While Thunderstack isn’t free, consider the manpower and opportunity cost of maintaining infrastructure. For many companies, paying Thunderstack a subscription is cheaper than employing dedicated engineers to do the same job. Additionally, Thunderstack’s multi-tenant architecture can be more cost-efficient on hardware usage than many separate individual nodes.
  * **Focus on Development:** You get to focus on your application’s features, user experience, and business growth instead of low-level infrastructure details. This can accelerate time-to-market for new Bitcoin-layer solutions.
* **Which Bitcoin Layer-2 technologies does Thunderstack support?**\
  Thunderstack covers a broad spectrum of Bitcoin’s layer-2:
  * **Lightning Network:** All aspects – running nodes (ThunderCloud), providing non-custodial access (ThunderEngine), liquidity provisioning (ThunderFlow), payment acceptance (ThunderLink).
  * **RGB Protocol:** A smart contract and asset issuance layer that works with client-side validation on Bitcoin. Thunderstack’s Lightning nodes (RLN) are RGB-enabled, and services like ThunderFlow and ThunderLink fully support RGB assets. This means Thunderstack is ready for tokenized assets on Bitcoin and their transfer via Lightning.
  * **Sidechains (BTC Sidechains):** These are separate blockchains that use BTC (or pegged BTC) as their currency, e.g. Rootstock (RSK), Liquid (possibly, if supported in future), and others【31†L111-L119】. Thunderstack RPC provides connectivity to these chains. Essentially, if a chain is Bitcoin-adjacent (either merge-mined, pegged, or otherwise linked), Thunderstack aims to support it.
  * **Rollups and Experimental Layers:** This includes things like **BitVM rollups** or drivechains. While these are emerging, Thunderstack’s support for them is either active (if networks exist) or planned. The mention of BitVM and rollups in their materials indicates they’re building for the future where Bitcoin could have optimistic or ZK-rollups posting data to L1【22†L97-L100】.
  * **Other protocols (if any):** If Bitcoin layer-2 expands beyond these (e.g., Ark protocol or federated systems like Fedimint), Thunderstack could potentially support those as well, though they haven’t been explicitly mentioned. The concept of “Bitcoin layers” is broad【31†L75-L83】, and Thunderstack’s mission is to be the go-to infrastructure provider for any promising Bitcoin layer.
*   **Is Thunderstack non-custodial? Do I retain control of my funds and keys?**\
    Thunderstack strives to enable non-custodial use wherever possible:

    * **Lightning (ThunderCloud/ThunderEngine):** You can have it so that you (or your end-users) hold the private keys to channels. ThunderEngine, for instance, is explicitly non-custodial【11†L104-L110】. Typically, you’d generate seeds or signing keys locally, and Thunderstack’s node will coordinate with that. However, if you choose convenience, Thunderstack can manage keys for you – but that’s optional. By default, your funds in channels can’t move without your signature.
    * **RGB Assets:** RGB is inherently client-side, meaning the state and secret data for assets reside with the user. Thunderstack’s infrastructure helps transport and manage state, but the authority to create a valid state transition (like spending an asset) lies with your keys. Thunderstack doesn’t get those secrets – it just relays the data.
    * **ThunderSafe:** It’s designed to be trustless (encrypted backups)【16†L103-L110】. Thunderstack stores your data but cannot read or misuse it without your keys.
    * **ThunderLink payments:** When you receive payments, Thunderstack does take custody of those funds on your behalf at the moment of receipt (since the payment goes to a node they control). However, those are your funds, and you can withdraw them. Think of it like how a payment processor holds funds briefly. For maximum trustlessness, you could run your own Lightning node and just use ThunderLink for invoice generation, but that loses some convenience. Thunderstack likely isolates merchant funds and lets you withdraw regularly to your on-chain wallet or a Lightning channel you control.
    * **Sidechain/RPC usage:** When using Thunderstack RPC, you might be sending signed transactions. Thunderstack isn’t taking custody; you control the keys that sign those transactions. They’re just a relay. If you use their service to manage an account on a sidechain (some providers offer wallet-as-a-service), that could be custodial – but Thunderstack hasn’t indicated they do that. It’s mostly an access service.
    * **BitVM Prover Service:** If Thunderstack is acting as Prover, there might be a scenario where it has to co-sign something or hold a bond for you. Ideally, you’d send your bond to a multi-sig or smart contract that ensures if Thunderstack misbehaves it gets slashed, otherwise you get it back. That’s a form of _ensured_ non-custodial behavior: they can’t steal your bond, only lose it if they cheat. More details will depend on implementation.

    In summary, Thunderstack is built to let you keep control of the important keys. In cases where they do hold something (like funds on a Lightning node or sidechain node), it’s typically because you’ve opted for that convenience or because technically someone must manage the hot wallet for the service (like an LSP node or a sidechain account). Even then, it’s compartmentalized and you can minimize trust by frequent withdrawals or by hybrid approaches (like hold a multi-sig key). Always refer to specific service documentation to understand custody implications.
* **How does Thunderstack ensure the privacy of my data and transactions?**\
  Privacy is a consideration in using any cloud service:
  * **Lightning/RGB:** Thunderstack running your node means they can see certain info that a node sees (channel balances, LN transactions, RGB transfer details). They likely have a privacy policy that they won’t snoop or share your data. Technically, they could log your channel balances, but they have no incentive and doing so at scale for all clients would be impractical. On Lightning, outside parties (routing nodes) don’t know the identities behind Thunderstack’s nodes, so your usage is as private as any Lightning usage (which is considered reasonably private, though not perfectly anonymous).
  * **RPC Calls:** If you are querying
*   **How does Thunderstack ensure the privacy of my data and transactions?**\
    Thunderstack is mindful of privacy on multiple levels:

    * **Data Encryption:** Communication with Thunderstack (APIs, node connectivity) is always encrypted (HTTPS, TLS). Any sensitive credentials or backups are additionally encrypted client-side (like ThunderSafe backups). So eavesdroppers cannot intercept useful info.
    * **Minimal Logging:** Thunderstack will log operational metrics (like API usage counts, performance data) to maintain the service, but not unnecessary personal data. Transaction data passing through (like a Lightning invoice or a sidechain transaction) is generally transiently in memory or short-term logs and is not used to profile users. They likely have a privacy policy committing not to sell or misuse customer data.
    * **Privacy in Lightning:** When using Thunderstack’s Lightning services, your channels and transactions are as private as they would be on any Lightning node you control. Thunderstack nodes use standard Lightning Network protocol which has privacy features (onion routing hides payment paths). Thunderstack (as the node operator) would know the source and destination if it’s your direct channel, but not the content of payments beyond amount and maybe an invoice memo. They treat that as your confidential business information.
    * **Isolation:** Each user’s environment is isolated. Your node instances (ThunderCloud) are separate from others, so one customer cannot snoop on another’s data. Similarly, on the RPC side, calls from your account don’t mix with others in a way that leaks info between users.
    * **Optional Self-Managed Keys:** For maximum privacy, you can keep certain things self-managed. For instance, you might use Thunderstack’s node but route it through Tor or a VPN to even hide metadata from Thunderstack about your usage patterns. Or use ThunderEngine with clients holding keys, so Thunderstack never sees individual users’ financial info.
    * **Transparency and Control:** You have access to your data (through dashboards/APIs) and can delete or revoke services. If you stop using Thunderstack, you can revoke API keys, destroy nodes (Thunderstack will wipe associated data on their servers), and because you hold keys/backups, you take all your info with you.
    * **Regulatory Compliance:** Thunderstack likely complies with regulations in how it handles data. They probably avoid personally identifying information unless needed for billing. Most usage can be anonymous (e.g., an email and payment method might be all that’s needed for an account; the rest is just technical data).

    In short, Thunderstack doesn’t add new surveillance compared to using the networks yourself; it operates the infrastructure and keeps your data as private as possible, with you in control of keys wherever feasible.

### Support and Documentation

* **What support channels are available for Thunderstack?**\
  Thunderstack provides multiple support avenues:
  * **Email Support:** You can reach their support team via email (e.g., contact@thunderstack.org) for any issues or questions【45†L101-L109】. They aim to respond promptly, especially for paid plans where SLA (Service Level Agreements) might guarantee response times.
  * **Community Chat (Telegram/Discord):** They have an official Telegram group【45†L109-L115】 where you can ask questions, engage with the community, and often get help from Thunderstack team members in real-time. Discord or Slack might also be available (if not now, potentially in the future), given many developer communities use those.
  * **Knowledge Base/Docs:** Their documentation site (docs.thunderstack.org) is extensive, covering “Getting Started” guides, API references, tutorials, and FAQs. Many common questions can be answered by searching the docs. It’s kept up-to-date (pages show last updated times)【8†L140-L144】 which is a good sign.
  * **Tutorials and Examples:** Besides reference docs, Thunderstack likely offers example code, how-to guides for specific tasks (like “Integrate ThunderLink into a Node.js app” or “Using ThunderSafe with XYZ Wallet”). These might be in the docs or on a blog.
  * **Developer Community:** There may be forums or Q\&A sites (maybe a category on Stack Exchange or Reddit) where Thunderstack users help each other. If not officially maintained, the Telegram/Discord would serve this purpose.
  * **Onboarding Support:** For enterprise or higher-tier plans, Thunderstack offers personal onboarding【33†L112-L120】. That means a team member will guide you through initial setup, integration, and answer architecture questions. This can be via scheduled calls or remote sessions.
  * **Ticket System:** Likely integrated with email support, there’s a ticketing system for tracking issues. Paid plan users (Starter, Growth, Scale) all have Ticket Support and Chat Support【34†L108-L116】【34†L128-L136】, meaning you can raise an issue and track it.
  * **Status Page:** They might have a status page to monitor uptime of various services (so you can check if an outage is on your side or theirs).
  * **Updates and Announcements:** Thunderstack probably sends out updates via email or their website when new features or maintenance is happening. Staying tuned to those helps you get the most out of the platform.
*   **Where can I find documentation for Thunderstack’s APIs and services?**\
    All documentation is consolidated on the Thunderstack Docs portal (docs.thunderstack.org). Within it:

    * **ThunderCloud Docs:** Contains guides on creating and managing RLN nodes, security configurations (like mTLS), backup/restore, and node API references【2†L16-L24】【2†L37-L44】. Look here for anything related to running and connecting to your nodes.
    * **ThunderEngine Docs:** Explains the API endpoints for account management, scheduling nodes, making payments, etc., along with authorization setup (API tokens)【11†L117-L125】【11†L152-L160】.
    * **ThunderFlow Docs:** Includes an overview of LSP concepts, how to get started with liquidity, pricing details【14†L130-L138】【37†L113-L121】, and technical specs (like the LSPS1 API structure).
    * **ThunderSafe Docs:** Describes how ThunderSafe works, features like Cloud Backups and Multi-Device Sync【18†L100-L108】, and steps to integrate it into wallets (the Getting Started section).
    * **ThunderLink Docs:** Provides integration instructions for accepting payments, including API usage and example workflows【48†L100-L107】【48†L113-L121】. Possibly with sample code or a demo.
    * **Bitcoin Sidesystems (Thunder RPC) Docs:** Contains explanation of the service (how it works, features)【4†L95-L104】, how to get started (obtaining API keys, making calls), details on Compute Units and rate limiting【43†L94-L102】, and a list of supported networks with any specifics for each【41†L104-L112】.
    * **Glossary:** Since Thunderstack references terms from the Bitcoin Layers taxonomy, the docs may link to an external glossary (bitcoinlayers.org) for definitions of terms like “Bitcoin native”, “sidechain”, “rollup”, etc. This is useful to understand context and how Thunderstack positions itself with respect to those concepts.
    * **Examples and SDK References:** If Thunderstack offers SDKs or client libraries, their docs will also have sections for those, including code snippets.
    * **Changelog:** There might be a changelog or “Release Notes” section in the docs where you can see what changed recently (like “added support for XYZ network” or “improved ThunderEngine API performance”).
    * **FAQs:** Some sections of the docs (or the support section) may have frequently asked questions, though this document you’re reading aims to serve as a comprehensive FAQ itself.

    All documentation is in English and written for a technical audience, but generally approachable. If something isn’t clear in docs, that’s when you reach out to support or community.
*   **Does Thunderstack offer any service level agreements (SLAs) or reliability guarantees?**\
    For paying customers, Thunderstack likely offers certain guarantees:

    * **Uptime SLA:** Typically, cloud services commit to something like 99.9% uptime for critical services (e.g., the RPC being available, or nodes staying online). If they fall short, there may be credits or refunds. The specifics might be in terms and conditions rather than docs.
    * **Support SLA:** Higher-tier plans might guarantee faster support responses. For example, Scale plan customers might get priority support with responses within a few hours, whereas Starter might be within 24 hours. Personal onboarding for paid plans indicates a higher level of attention.
    * **Data Durability:** For ThunderSafe, they may not explicitly say it, but one can assume strong durability (multiple copies of backups, etc., possibly aiming for 99.999999% durability akin to AWS S3). If you back up with them, it should be exceedingly unlikely to lose that backup.
    * **Latency/Performance:** They might not guarantee a specific latency, but by design, the infrastructure is low-latency (Lightning payments settle in milliseconds, RPC calls in usually < 200ms, depending on network). If performance issues arise on their side, that’s covered under uptime/reliability.
    * **Notification of Issues:** A part of reliability is communication. Thunderstack likely promises to inform users of maintenance or incidents promptly via a status page or email.

    It would be wise to check Thunderstack’s Terms of Service or ask sales for specifics on SLA if you are an enterprise needing formal guarantees.
*   **How can I get help if I run into an issue integrating Thunderstack’s services?**\
    If you face an issue:

    1. **Consult Documentation:** Re-read the relevant docs to ensure you didn’t miss a step. There might be troubleshooting tips (like common error messages and what they mean).
    2. **Community Help:** Post your issue in the Thunderstack Telegram or community chat. Often, someone might have encountered it and can offer a quick fix. Provide details (service you’re using, what you tried, any error logs).
    3. **Open a Support Ticket:** Use the Thunderstack support email or web form to describe the problem. Include as much detail as possible: your account (if comfortable), what you were doing, timestamps, any relevant node IDs or request IDs, and how to reproduce the issue. The support team can then investigate the logs on their side.
    4. **Personal Onboarding/Account Manager:** If you are an enterprise client or on a plan that includes onboarding, you might have a direct contact or account manager. Reach out to them—they can coordinate with engineering if needed to resolve your issue.
    5. **Check for Software Updates:** If it’s an integration issue (like using an SDK), ensure you have the latest version of the Thunderstack SDK or that your dependencies (like a web3 library for RPC) are up to date. Sometimes issues come from outdated client libraries.
    6. **Log and Isolate:** Provide logs or use debugging tools. For instance, if ThunderLink webhook isn’t called, confirm that your server is reachable and the webhook secret matches. Using tools like ngrok (for local dev) or checking server logs can isolate whether the issue is on Thunderstack’s side or the user’s side.

    Thunderstack’s support is known to be developer-friendly and responsive, as the success of their platform depends on customers being able to integrate smoothly. Don’t hesitate to ask for help.
*   **Is there a community or forum for Thunderstack where I can ask questions?**\
    Yes, as mentioned:

    * The **Telegram community chat**【45†L109-L115】 is an interactive place for quick questions and discussion.
    * If a forum style is preferred, Thunderstack might encourage use of platforms like **Stack Exchange (Bitcoin StackExchange)** for generic questions about Lightning/RGB where the answers benefit all (but for Thunderstack-specific, the official channels are better).
    * Possibly a subreddit (though none is specifically cited, you might find discussions on r/Bitcoin or r/lightningnetwork when Thunderstack was launched).
    * Developer conferences/meetups: Thunderstack’s team may appear in Bitcoin developer chats (like the LDK or Lightning dev mailing lists, etc.) but that’s more to discuss technical improvements. For user support, stick to their channels.
    * They may host **webinars or workshops** from time to time. Joining those can be a way to learn best practices and ask questions live.

    Engaging with the community is beneficial – not only do you get help, but you might discover use-cases or ideas from others on how to leverage Thunderstack’s full potential.

### Billing and Pricing

*   **How is Thunderstack priced? Are there different plans?**\
    Thunderstack’s pricing is typically structured in tiers:

    * **Starter Plan:** For individuals or small projects starting out. As per the docs, it’s around $35 per month【34†L102-L110】. It includes 1 managed RLN node, a generous amount of API calls (e.g., 250k per month or maybe per period)【34†L108-L116】, and 1 user access. Support is provided via ticket and chat, plus personal onboarding to help you get started.
    * **Growth Plan:** For expanding projects or small companies. Around $99 per month【34†L124-L132】. It bumps resources to maybe 3 nodes, 1,000,000 API calls【34†L128-L136】, possibly more team members, etc. Support similarly via ticket/chat and onboarding.
    * **Scale (Wallet Plan):** This is usage-based【34†L144-L152】, aimed at larger deployments such as wallet services or enterprise. Instead of a fixed node count, you pay per node usage: e.g., $15 per node per month if a node is running continuously, or $5 per node per month if it’s paused (not actively running)【34†L146-L154】. This plan includes unlimited API calls, multiple users (for team access), ability to host additional Bitcoin/RGB software (perhaps you can run an indexer or a custom service alongside)【34†L154-L162】. Essentially, it’s more flexible and scalable and you pay for what you use. Support and onboarding are of course included at this level too.
    * **Custom/Enterprise:** Not listed but often available if you need something beyond these plans. Enterprise plans might involve custom SLAs, dedicated infrastructure, on-premise options, etc. Pricing would be negotiated.
    * **Trial:** They mention a 3-month free trial for any plan【34†L93-L100】 which is very generous. This allows you to test things in depth. After trial, you start paying the monthly fee. You should cancel before trial ends if you don’t want to be charged.
    * **Add-Ons:** Some services might be add-ons. For example, ThunderFlow’s liquidity channels have fees (sats for opening channels)【37†L115-L124】, which is separate from the monthly plan. Similarly, ThunderLink might charge a percentage per transaction or per invoice (even if integration is free).
    * **Compute Units for RPC:** If Thunderstack RPC is included in these plans, the API call counts/compute units are part of it. If not, there might be separate pricing for RPC usage (maybe pay-as-you-go beyond a certain free amount). It’s likely integrated though – e.g., the API call counts given might apply to any mix of node API or RPC calls.
    * **Overages:** Check if the plans allow overage (e.g., if you exceed 250k calls on Starter, will it throttle or charge extra?). Possibly they’ll throttle to keep you within plan, or prompt an upgrade. It’s safer to upgrade in advance if you foresee more usage.

    Always refer to their official pricing page for the latest details, as they can adjust plans/prices.
*   **How is billing handled for ThunderFlow LSP channels or ThunderLink transactions?**\
    These are usage-based beyond the base subscription:

    *   **ThunderFlow (LSP) Channels:** When you request a channel via ThunderFlow, you typically pay a fee for that channel. ThunderFlow has a fee structure:

        * Base fee: e.g., 1500 sats flat for opening a channel【37†L115-L119】.
        * Plus a percentage on the amount of capacity (0.05% of the capacity provided)【37†L115-L123】.
        * Plus a small percentage on any push amount (initial balance pushed to you)【37†L117-L120】.
        * Plus the on-chain fee to open the channel (depends on Bitcoin’s fee rate)【37†L123-L127】.

        Example: If you open a channel with 1,000,000 sats capacity and they push 100,000 sats to you, the fee might be 1500 + (1,000,00&#x30;_&#x30;.0005) + (100,00&#x30;_&#x30;.0005) + on-chain fee. That’s 1500 + 500 + 50 + maybe 1000 (if mempool fee = 2 sat/vB \* 500 vbytes) = \~3050 sats in total (roughly a few cents at current BTC price) – quite reasonable for liquidity on demand.

        You would pay this either by invoice (send sats over Lightning to ThunderFlow) or it might be deducted from an account balance if Thunderstack has a deposit system.

        If you choose a volume-based subscription instead, you might pay a fixed monthly fee to get unlimited channels within certain volume, etc. The details for volume subscription might be tailored to specific needs (like an LSP as a service for wallets).
    * **ThunderLink Transactions:** ThunderLink itself is free for integrators, meaning there’s likely no monthly charge or setup fee. However:
      * They might charge a small fee on each transaction (like 1% or a fixed tiny fee) to monetize the service. It’s not explicitly stated, but “free for integrators” implies you don’t pay to use the API, but maybe they earn via routing fees or a cut of payments.
      * If ThunderLink uses LN routing, some fees are inherent (routing nodes take a few ppm). If Thunderstack controls the route (like they have a direct channel to the payer), they might waive extra fees. Possibly they run it like a loss leader to attract usage.
      * Check terms: maybe up to a certain volume it’s free, and beyond that, they introduce fees.
      * For planning, if you integrate ThunderLink, ask Thunderstack or watch for an announcement on their fee policy for payments. It could simply be that you pay normal Lightning fees and nothing additional to Thunderstack.
    * **ThunderSafe:** Usually, backup services are included in plan. They didn’t list an extra price for ThunderSafe, so assume it’s bundled (i.e., unlimited backups for your nodes).
    * **Exceeding Limits:** If you use more than your plan’s nodes or calls, likely you need to upgrade. If you temporarily exceed (like spike in API calls), they might not immediately cut you off – maybe they’ll warn you or auto-upgrade you for that period. It’s good to clarify how that’s handled to avoid surprises.

    In summary: Subscription covers base infrastructure, and certain value-adds like liquidity and payments are pay-per-use, usually directly in BTC or via usage charges.
* **Can I change plans or cancel my Thunderstack subscription easily?**\
  Typically yes:
  * You can upgrade or downgrade plans via the Thunderstack dashboard or by contacting support. Upgrades usually take effect immediately (and you pay pro-rated difference or new price). Downgrades might take effect after your current billing cycle completes (to avoid mid-cycle issues if you already paid).
  * **Cancellation:** You should be able to cancel anytime. If you cancel, your services would likely continue until the end of the paid period then shut down. Thunderstack would provide a way to export any data if needed (though if you have your keys, you already have what you need mostly). For courtesy, you might manually shut down nodes and download backups before cancelation to ensure nothing is lost.
  * **Trial Cancelation:** If in the trial, make sure to cancel before trial ends if you don’t want to be charged for the next period.
  * **Flexible Node Count (Scale plan):** On the Scale plan, you’re not locked into a number of nodes; it adjusts by usage. So “changing plan” there might mean moving to a fixed plan if you decided you prefer that, or vice versa. Thunderstack likely lets you switch to Scale if you outgrow Growth, or to Growth if you realize a fixed plan suits you better.
  * **Temporary Scaling:** If you have an occasional need for more nodes or calls, you might be able to upgrade for a month then downgrade. Check with Thunderstack if they have any minimum commitment, but generally monthly SaaS means you have that flexibility.
  * The platform likely has a billing page showing your current plan, usage, and a button to change plan or cancel. If not, an email to support can achieve the same.
*   **How does billing work for the RPC service (Thunder RPC)?**\
    There are a few possibilities:

    * It could be included as part of the node plans (like Starter includes 250k API calls which could be used for either node API or RPC calls). In that case, no separate billing – it’s just part of your plan’s call quota.
    * Or they might have separate usage-based pricing especially if someone only wants RPC access without ThunderCloud etc. For instance, maybe a free tier for RPC calls and then pay per CU beyond that.
    * Given the docs talk about daily quotas and such, it sounds like the RPC usage is included and managed by quotas, not by a separate invoice each time. So likely, if you hit your daily CU limit, the service might throttle or stop until next day. If you consistently hit it, that’s a sign to get a higher plan.
    * For enterprise usage of RPC (like extremely high volume), they might custom price it (like $X per million calls).

    It’s best to consult Thunderstack’s pricing page or ask sales for clarity if your primary interest is the RPC service. They may simplify it by saying “the node plans also give you RPC access to all supported networks” as a bundled offering.
*   **Are there any hidden fees (e.g., for bandwidth or storage)?**\
    Thunderstack’s pricing seems straightforward for what’s listed. Some considerations:

    * **On-chain fees:** If Thunderstack performs on-chain operations for you (opening channels, closing channels, posting Bitcoin transactions in rollups), those on-chain fees are typically passed to you (either you pay in sats or they count it separately). For example, ThunderFlow channel open fee includes the actual Bitcoin tx fee【37†L123-L127】; Thunderstack doesn’t cover that for you.
    * **Storage:** If you have massive data (like terabytes of backup in ThunderSafe), there might be fair use limits. But typical Lightning/RGB data is small. If they did have limits, they would inform in the plan (e.g., backup retention or size limits). None are mentioned, so likely not an issue.
    * **Bandwidth:** Your usage of their APIs is effectively bandwidth. The quotas (CUs) cover how much you can call. There’s no separate bandwidth charge like cloud hosting might have. If you do something extreme (like download entire blockchain via RPC), you’ll hit call limits first anyway.
    * **Additional Services:** If later they add new services (like some analytics or premium support beyond what’s in plan), those might cost extra. But you opt into those explicitly.
    * **Lightning Routing Fees:** If you use Thunderstack’s node to send payments (like via ThunderEngine), you or your end user will incur normal Lightning routing fees which go to the network (other nodes), not Thunderstack. Not hidden, but just to recall that not everything is free just because it’s off-chain.
    * **Taxes:** Depending on jurisdiction, if Thunderstack is a company, they might charge VAT or sales tax on the subscription. E.g., EU customers might have VAT added. The pricing likely is listed without taxes. The billing system will account that based on your billing info.

    In summary, fees are transparent: subscription covers core service, usage fees for channels or similar are clearly defined, and Bitcoin network fees are passed as is. There shouldn’t be arbitrary extra charges.
*   **Does Thunderstack offer refunds or credits if something goes wrong?**\
    Typically, if the service fails to meet SLA (downtime, etc.), you might be eligible for credits. For example, if they promise 99.9% uptime and they only delivered 99% in a month, they might credit you a portion of that month’s fee. This would be in their SLA terms.

    * For unused time: If you cancel mid-month, usually you don’t get a pro-rated refund for the remaining days (since it’s prepaid monthly), but your service continues till period end. If you had an annual plan (not sure if they offer annual discounts), some services do partial refunds, some don’t.
    * **Trial:** During free trial you’re not charged, so no refund needed there. After trial, if you forgot to cancel and got charged, you could appeal to their support – some companies might refund if it’s very soon after and the usage was low, as goodwill.
    * **Customer Service:** If you had a bad experience (like significant outage causing business loss), you can talk to them; reputable services will often work with you, maybe giving a free month or extra resources to compensate.
    * **No lock-in:** The best ‘refund’ is that you’re not locked long term. If it doesn’t suit you, you can leave after your current period. And with non-custodial design, you’re not locked out of your funds or data either.

    Always check the official refund policy or ask support if concerned. But generally, issues can be resolved via support channels with minimal financial impact to customers.
*   **How does Thunderstack accept payments (credit card, BTC, etc.)?**\
    While not explicitly stated:

    * Likely **Fiat (Credit Card):** They probably use standard SaaS billing with credit/debit cards. Possibly via Stripe or similar. It’s the easiest for recurring billing.
    * **Bitcoin/BTC:** Given their business, it would make sense they accept Bitcoin payments for subscriptions. Maybe via Lightning (they could even dogfood their own ThunderLink to let you pay!). Or on-chain BTC. If not now, perhaps in roadmap. It might be manual though (like invoice in email). But I wouldn’t be surprised if you can pay with BTC or stablecoins.
    * **Other Crypto:** If they accept BTC, they might accept other crypto like USDT on Lightning or Liquid, etc., since they are in that space. But BTC is the primary likely.
    * **Wire/Invoice:** For enterprise customers, they might accept wire transfers or bank payments (especially if someone wants an annual invoice).
    * During signup or in the account portal, you’d see payment methods. Many services allow adding a card or selecting crypto. If paying by card, ensure international transaction is enabled (if they are not domestic).
    * If paying in BTC, watch for volatility or stable sats amounts. They might price in USD and convert at payment time to an equivalent BTC amount for the invoice.

    It’s actually a good sign if they accept Lightning for their service, it shows confidence in the tech. Possibly the free trial flows into a Lightning invoice or something. Check their site’s billing section for what options are presented.

This concludes the comprehensive Thunderstack FAQ. We covered each product—ThunderCloud, ThunderEngine, ThunderFlow, ThunderSafe, ThunderLink—under Bitcoin-Native Infrastructure, and Thunderstack RPC (with rollups and BitVM) under Bitcoin Sidechains, as well as general, support, and billing questions. With this, you should have a solid understanding of Thunderstack’s offerings and how to leverage them for your Bitcoin Layer-2 projects. Happy building on Bitcoin’s layers!

##

* **How does ThunderLink ensure security and trust for payments?**\
  Security considerations:
  * **Secure Invoices:** The Lightning invoices generated are standard and include hashes that ensure an invoice can’t be modified undetectably. Thunderstack’s infrastructure generates them and will only consider an invoice paid when the exact correct amount is received to the exact secret hash.
  * **No Middleman Custody of Funds:** The payments go directly to Thunderstack’s Lightning node (which is effectively your payment processor here). Thunderstack then will settle with you (probably they custody the received funds for you in a Thunderstack wallet or forward them on-chain to an address you control periodically). They operate similarly to how a payment processor would, but you can withdraw your funds at any time. Since Thunderstack’s nodes are well-managed and secured, this is generally safe, but you should periodically sweep out your earnings to cold storage if large.
  * **Webhook Verification:** Webhook calls to your server can be secured with HMAC signatures or secret tokens to ensure the notification truly comes from ThunderLink and not an attacker. The integration guides cover validating these signatures.
  * **Fraud Prevention:** Lightning inherently has no chargebacks (once paid, it’s final), which reduces fraud for you as a merchant. ThunderLink makes use of that. They also ensure an invoice can only be paid once, etc. If a user somehow overpays or pays after expiry, that payment won’t be applied to an order (and might be refunded or credited manually). All these edge cases are handled to keep accounting clean.
  * **Privacy:** Users paying you via ThunderLink do not expose any more info than a normal Lightning payment. Thunderstack will know the details of the payment (amount, asset, maybe a memo if provided) and your account receiving it. They won’t necessarily know the end customer’s identity (unless you encode something in the invoice). This is similar to how any payment processor works, but it’s relatively privacy-preserving for the customer compared to credit cards.
  * **Reliability:** ThunderLink being built on Thunderstack means the Lightning nodes have high uptime. However, if a rare event occurs (like Thunderstack’s Lightning node goes down), invoices might temporarily not be payable. Thunderstack likely has redundancies (multiple nodes or automatic failover for generating invoices). In practice this means the service is reliable for day-to-day operations.
* **Can ThunderLink be automated for subscriptions or repeated payments?**\
  Yes, to an extent:
  * **Recurring Payments:** Lightning itself doesn’t have a native recurring payment pull mechanism (no automatic pulls like a credit card subscription). But you can implement subscriptions by generating a new invoice every period and prompting the user or auto-paying if the user’s wallet supports something like **AMP or scheduled payments**. With ThunderLink’s API, you can easily generate these repeat invoices on schedule.
  * **Bulk Invoices:** If you need to create many invoices (say one for each user each month), you can script calls to ThunderLink’s API. It can handle volume as it’s a cloud service.
  * **Notification Management:** The webhook system allows your backend to automatically process payments as they come without manual checks. This means you can fully automate digital service delivery (user pays via ThunderLink, webhook triggers code to upgrade their account).
  * **Integration with ThunderFlow:** If you as a merchant frequently receive large payments in RGB assets via ThunderLink, you might also leverage ThunderFlow to ensure your inbound capacity remains sufficient. ThunderLink presumably handles that under the hood (it likely uses ThunderFlow or its own liquidity to always be able to receive).
  * **Payouts:** While ThunderLink is for receiving payments, if you needed to also send payouts (like refunds or rewards to users), you might use ThunderEngine or direct Lightning for that. Thunderstack’s ecosystem covers sending as well, but ThunderLink itself is focused on incoming payments (the “link” from user to merchant).
* **What kind of interface or API does ThunderLink provide?**\
  ThunderLink offers:
  * **REST API:** Endpoints to create and manage payment requests. Possibly endpoints to check the status of a payment (if you don’t want to rely solely on webhooks).
  * **JavaScript Widget:** They might provide a JS snippet you can drop in your checkout page that handles opening the ThunderLink widget/iframe. This would simplify integration on the front-end.
  * **Dashboard:** A merchant dashboard where you can see a list of payments, filter by status (paid/unpaid), manually mark items if needed, and get payout info. This helps non-developers on your team (like accounting) to verify payments.
  * **Integration Guides:** Extensive docs and maybe even plugins for popular platforms. For instance, a WordPress/WooCommerce plugin or a Shopify integration could be offered, making it plug-and-play to accept Lightning via ThunderLink on those platforms.



