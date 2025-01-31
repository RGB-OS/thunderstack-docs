# ▶️ Getting Started

#### Prerequisites

1. An active Lightning Network node.
2. Support for RGB assets in your wallet or application.
3. API access to ThunderFlow (Request demo via our [website](https://www.thunderstack.org/?demo=true)).

#### How to Request Liquidity

1. **Register Your Node:** Use the ThunderFlow API to register your node.
2. **Request Liquidity:** Send a request specifying the amount, asset type, client node peer URL, and channel duration.

<figure><img src="../.gitbook/assets/Screenshot (4).png" alt=""><figcaption></figcaption></figure>

* **Pushed Amount (msat)** – The initial amount (in milli-satoshis) that will be sent to the counterparty upon channel creation. Default - 0
* **Channel Expiry (Blocks)** – The number of blocks until the channel expires.&#x20;
* **Node Peer URL** –  The Peer URL of the client's node. This peer will be used by the LSP to open the channel to the node (require).

<figure><img src="../.gitbook/assets/Screenshot (3).png" alt=""><figcaption></figcaption></figure>

* **Asset Amount** – The amount of a specific asset requested for the channel. This is require field.&#x20;
* **Asset ID** – The ID of the asset being used for the channel.
* **Refund Address (Optional)** – A Bitcoin address where funds should be refunded in case the channel is not opened or the order fails. If left empty, refunds may require manual handling.



1.  **Confirm Payment:** Pay using BTC or RGB assets.\


    <figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
2.  **Channel Opening:** The channel will be provisioned and monitored in real time.

    <figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>
