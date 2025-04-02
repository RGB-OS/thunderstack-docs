---
icon: screwdriver-wrench
---

# RGB LSP Configuration

## LSP Configuration

This documentation outlines the key parameters and configuration used for the LSP, which is powered by an RGB Lightning node hosted on ThunderStack Cloud.

### Key Static Fields

1. **`min_channel_balance_sat`**
   * The minimum allowable size of the channel balance.
   * **Static value**: 100,000 satoshis.
2. **`min_initial_lsp_balance_sat`**
   * The minimum amount of liquidity that the LSP will provide when opening a channel.
   * **Static value**: 100,000 satoshis.
3. **`max_channel_expiry_blocks`**
   * The maximum number of blocks that a channel will remain open.
   * **Static value**: 12,960 blocks (approximately 3 months).

### Dynamic Fields

1. **`max_initial_lsp_balance_sat`**
   * This value is dynamic and based on the current balance of the node.
   * It is fetched from the `/btcbalance` API, where the `spendable` field from the response defines the actual value of `max_initial_lsp_balance_sat`.
2. **`lsp_assets`**
   * This is an array of assets supported by the LSP node.
   * The list of assets is returned by the `/listassets` API.

### Node URIs

* The `uris` field consists of the following format:\
  &#xNAN;**`pubKey@peerDNS:peerPort`**.
* These values represent the public key, DNS, and port of the Lightning node and are available in the node configuration options within ThunderStack Cloud.
