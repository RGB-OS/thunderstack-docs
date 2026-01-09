---
icon: wallet
---

# Lumen

## Lumen – RGB Lightning Node Connector

**Lumen** is a browser extension that connects your browser to an [RGB Lightning Node (RLN)](https://github.com/RGB-Tools/rgb-lightning-node).\
It works as a **bridge** between your node and any dApp that supports the [RGB WebLN interface](https://thunderstack.gitbook.io/rgbln).

Lumen is not a wallet itself — it does not hold your keys or sensitive data.\
Instead, it lets you use your own RGB LN as a wallet backend, either self-hosted or running on **ThunderStack Cloud**.

***

### Features

* Connect to self-hosted or ThunderStack Cloud RGB Lightning Node via HTTP API.
* Exposes node functionality via RGB WebLN.
* Manage RGB assets through your browser.
* Pay and receive invoices.
* Sign messages.
* Use WebLN calls in dApps (e.g. `rgbInvoice`, `sendAsset`).
* Non-custodial: your node manages all private state and signing.

***

### Getting Started

### Installation

The extension will be available on the Chrome Web Store (link to be added).

For development builds, clone the repository and load the extension manually in your browser.

#### 2. Set Up an RGB LN

You have two options:

* **Run your own node**: Install RLN locally, initialize it, and unlock it.
* **Use ThunderStack Cloud**: Simplest option — create a node online, which comes with Biscuit authentication enabled by default.

Guide: [Create a node on ThunderStack Cloud](getting-started-with-thunderstack-rgb-cloud/general/create-rln-node/)

#### 3. Get Connection Parameters

Once your node is ready and unlocked, go to the **Connect** tab in your node dashboard.

<figure><img src="../../.gitbook/assets/Screenshot (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot (8).png" alt=""><figcaption></figcaption></figure>

\
You’ll need:

* **Node API endpoint** (API URL)
* **Node token** (authentication token or Biscuit)



#### 4. Connect in Lumen

1.  Open the Lumen extension.<br>

    <figure><img src="../../.gitbook/assets/Знімок екрана 2025-09-22 о 12.38.45 (1).png" alt=""><figcaption></figcaption></figure>
2. Enter the API endpoint and token.
3. Connect.

If successful, your node will show as connected and ready.\
![](<../../.gitbook/assets/Знімок екрана 2025-09-22 о 12.41.07 (1).png>)
