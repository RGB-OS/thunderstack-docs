# ⚙️ RGB LSPS1 APIs

## API

The RGB LSPS1 APIs enable management of channels and liquidity services following the Lightning Service Provider Specification (LSPS1). This section provides details on available endpoints and their usage.



#### 1 Get Info

```
https://lsp.thunderstack.org/get_info
```

Request No parameters needed. Response

```json
{
  
    "max_channel_expiry_blocks": 12960,
    "min_initial_lsp_balance_sat": 100000,
    "max_initial_lsp_balance_sat":100000000,
    "min_channel_balance_sat": 100000,
    "max_channel_balance_sat": 100000, 
    "lsp_assets": {
        "nia": [],
        "uda": [],
        "cfa": []
    },
    "uris":[
        "030e5245c3d4ad7a6ed15da5994ee436ebc42fc7114854fa722e737f9b8f23f832@db3ae3932fb94bc6820f8274da66749c.f71a077ba41a4c5589e1362a03dbe72f.peers.thunderstack.org:12198"
    ],
}
```

* `max_channel_expiry_blocks <uint32>` The maximum number of blocks a channel will remain open.
  * MUST be 1 or greater.
  * This value determines how long the channel will be kept open, approximately 13,140 blocks correspond to 3 months.
* `min_initial_lsp_balance_sat` Minimum number of satoshis the LSP will provide to the channel.
  * MUST be 0 or greater.
  * This sets the minimum liquidity the LSP offers when opening a channel.
* `max_initial_lsp_balance_sat <sat>` Maximum number of satoshis the LSP will provide to the channel.
  * MUST be 0 or greater.
  * This is the upper limit of liquidity the LSP can offer, in this case, 1 BTC (100,000,000 sats).
* `min_channel_balance_sat <sat>` Minimum allowable size for the channel balance.
  * MUST be 0 or greater.
  * Defines the smallest possible channel that can be opened, typically around 100,000 sats.
* `max_channel_balance_sat <sat>` Maximum allowable size for the channel balance.
  * MUST be 0 or greater.
  * Defines the largest possible channel size, set to 100,000 sats in this case.
* `lsp_assets <array>` A list of supported assets for the LSP.
  * MAY be empty if no specific assets are defined.
  * This array allows support for various cryptocurrencies or tokenized assets.
* `uris <array>` URIs of the LSP nodes used for clients to connect.
  * This contains the node's public key, domain name or IP address, and the port number.
  * Client MUST connect to one of these URIs.

Every `min/max` options pair MUST ensure that `min <= max`.

## 2 Create Order

```
HTTP POST
MAINNET: https://lsp.thunderstack.org/create_order
TESTNET: https://lsp.test.thunderstack.org/create_order
Request:
```

```json
 {
"lsp_balance_sat": "number",
    "public_key": "string",
    "channel_expiry_blocks": "number",
    "asset_amount": "number", // Optional
    "asset_id": "string", // Optional
    "refund_on_chain_address": "string" //Optional
}
```

### Request Fields

* `lsp_balance_sat <sat>`\
  The size of the channel requested by the client in satoshis.
  * **Required**
  * MUST be equal to or greater than `min_initial_lsp_balance_sat` and less than or equal to the amount available (`max_channel_balance_sat`).
  * MUST be 100,000 sats or greater (as recommended).
  * Example: `100000` (for a channel of 100,000 sats).
* `public_key <string>`\
  The public key of the client's node. This public key will be used by the LSP to open the channel to the node.
  * **Required**
  * The client must already be connected to the LSP node before sending the request.
  * Example: `"030e5245c3d4ad7a6ed15da5994ee436ebc42fc7114854fa722e737f9b8f23f832"`
* `channel_expiry_blocks <uint32>`\
  The number of blocks for which the channel will remain open.
  * **Required**
  * MUST be less than or equal to `max_channel_expiry_blocks` from the `/get_info` response.
  * Example: `12960` (for a channel open for 90 days).
* `asset_amount <sat>`\
  The amount of a specific asset (if applicable) requested for the channel.
  * **Optional**
  * If provided, `asset_id` is also required.
  * Example: `50000` (for an asset amount of 50,000 satoshis).
* `asset_id <string>`\
  The ID of the asset being used for the channel.
  * **Optional**
  * Required if `asset_amount` is provided.
  * Example: `"rgb:25w4w77-sk7mfxMJC-cqiZmmZrx-hzrvcADto-R3z6em592-88fsXVV"`
* `refund_on_chain_address <string>`\
  The on-chain Bitcoin address where refunds will be sent if the channel opening fails.
  * **Optional** if the client intends to pay with an on-chain Bitcoin payment.
  * Example: `"bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh"`

### Response

```json
{
    "order_id": "66e42dba6afb292692ade4e7",
    "status": "Pending",
    "created_at": "2024-09-13T12:19:06.991Z",
    "updated_at": "2024-09-13T12:19:06.991Z",
    "lsp_balance_sat": 100000,
    "channel_expiry_blocks": 12960,
    "refund_on_chain_address": "02c9d1afd383fb651d93975d2a2e727aca5e6dd4fa2f090fded50102a149969ce8",
    "public_key": "02c9d1afd383fb651d93975d2a2e727aca5e6dd4fa2f090fded50102a149969ce8@3dc0a5a72c2748e0bcd3b6c8fea9db9c.80c65b6183a949d1a642f5dd64a4f5e9.peers.thunderstack.org:12187",
    "payment": {
        "ln": {
            "order_id": "67ab289d69711ad1f44aa8e6",
            "type": "ln",
            "status": "Pending",
            "fee": 251515,
            "invoice": "lnbcrt2515150n1pn6k2y7dqud3jxktt5w46x7unfv9kz6mn0v3jsnp4qdh55vg4n3pgt2a635zue44w2tzl00ast96ks3jelxkq873zl3ljcpp5ft2zj96v64tya0c08evjq5dg5jxkkfwy4m5mnrg7p4j4tjk0znxssp530hy83x7gr6ldrawk0xe3rphz0uc9v7x8cpfequgnuq493t57uuq9qyysgqcqpcxqrpcgwnwlpmaq2lm24mjsef5sqtycpydl543a00jw2sthjtwgzdshfeunjs20uuj356ks6lhzrutcjyu9x6q38d244aafwpvxqc34jawxyzspxxn6zw",
            "updated_at": "2025-02-11T10:38:22.138Z",
            "created_at": "2025-02-11T10:38:22.138Z",
            "expires_at": "2025-02-11T11:08:22.138Z"
        },
        "btc_onchain": {
            "order_id": "67ab289d69711ad1f44aa8e6",
            "type": "btc_onchain",
            "status": "Pending",
            "fee": 251515,
            "invoice": "bitcoin:bcrt1q36xg65c3wdj969f92f2wd0za30z3lzq0envx88?amount=0.00251515&label=67ab289d69711ad1f44aa8e6&message=Payment%20for%20order%3A%2067ab289d69711ad1f44aa8e6",
            "updated_at": "2025-02-11T10:38:22.390Z",
            "created_at": "2025-02-11T10:38:22.390Z",
            "expires_at": "2025-02-11T11:38:22.390Z"
        }
    },
    "channel": null
}
```

### Description

When an order is created, the client must already be connected to the LSP. The response contains the parameters provided by the client, which are necessary to open a channel. In the `payment` field, two payment methods are available:

1. **`ln`** – an invoice for payment in satoshis.
2. **btc\_onchain** – an onchain invoice for payment in satoshis.

To complete the order and create a channel, the client must connect to the node and pay the invoice. Once the invoice is paid, the channel with the requested liquidity will be created.

The `channel: null` field indicates that the channel has not yet been created.

#### Order Statuses

An order can have the following statuses:

#### Channel Statuses

* **Pending**: Order has been created but not yet completed.
* **Succeeded**: Invoice has been paid, and the channel is created.
* **Failed**: Channel creation or payment failed.
* **Expired**: The order or invoice has expired.

## 3 Get Order

The `get_order` endpoint returns the current status of an order.

```
HTTP GET
MAINNET: https://lsp.thunderstack.org/get_order?order_id=<order_id>
TESTNET: https://lsp.test.thunderstack.org/get_order?order_id=<order_id>

```

### Response

```json
{
    "_id": "66e42dba6afb292692ade4e7",
    "status": "Succeeded",
    "created_at": "2024-09-13T12:19:06.991Z",
    "updated_at": "2024-09-13T12:19:06.991Z",
    "lsp_balance_sat": 100000,
    "channel_expiry_blocks": 12960,
    "refund_on_chain_address": "02c9d1afd383fb651d93975d2a2e727aca5e6dd4fa2f090fded50102a149969ce8",
    "public_key": "02c9d1afd383fb651d93975d2a2e727aca5e6dd4fa2f090fded50102a149969ce8@3dc0a5a72c2748e0bcd3b6c8fea9db9c.80c65b6183a949d1a642f5dd64a4f5e9.peers.thunderstack.org:12187",
    "payment": {
        "ln": {
            "order_id": "67ab289d69711ad1f44aa8e6",
            "type": "ln",
            "status": "Pending",
            "fee": 251515,
            "invoice": "lnbcrt2515150n1pn6k2y7dqud3jxktt5w46x7unfv9kz6mn0v3jsnp4qdh55vg4n3pgt2a635zue44w2tzl00ast96ks3jelxkq873zl3ljcpp5ft2zj96v64tya0c08evjq5dg5jxkkfwy4m5mnrg7p4j4tjk0znxssp530hy83x7gr6ldrawk0xe3rphz0uc9v7x8cpfequgnuq493t57uuq9qyysgqcqpcxqrpcgwnwlpmaq2lm24mjsef5sqtycpydl543a00jw2sthjtwgzdshfeunjs20uuj356ks6lhzrutcjyu9x6q38d244aafwpvxqc34jawxyzspxxn6zw",
            "updated_at": "2025-02-11T10:38:22.138Z",
            "created_at": "2025-02-11T10:38:22.138Z",
            "expires_at": "2025-02-11T11:08:22.138Z"
        },
        "btc_onchain": {
            "order_id": "67ab289d69711ad1f44aa8e6",
            "type": "btc_onchain",
            "status": "Pending",
            "fee": 251515,
            "invoice": "bitcoin:bcrt1q36xg65c3wdj969f92f2wd0za30z3lzq0envx88?amount=0.00251515&label=67ab289d69711ad1f44aa8e6&message=Payment%20for%20order%3A%2067ab289d69711ad1f44aa8e6",
            "updated_at": "2025-02-11T10:38:22.390Z",
            "created_at": "2025-02-11T10:38:22.390Z",
            "expires_at": "2025-02-11T11:38:22.390Z"
        }
    },
    "channel": {
        "channel_id":"9e4a1140dd3a810e2807bb847f7a25219a2ce889892e59c27e7f8c928478980e",
        "temporary_channel_id":"9e4a1140dd3a810e2807bb847f7a25219a2ce889892e59c27e7f8c928478980e",
        "status":"Open",
        "created_at":"2024-08-29T14:25:38.828+00:00",
        "updated_at":"2024-08-29T14:54:45.355+00:00"
    }
}
```

#### Channel Creation Workflow

If the user pays one of the invoices (either `ln` or `ln_asset`), a channel will be created by the LSP. There are several conditions required for a successful order completion:

1. **Payment Success**:\
   Once the payment is successfully made, the LSP initiates the channel creation process.
2. **Channel Initialization**:\
   The LSP generates a `temporary_channel_id` and gathers the parameters sent by the client. These parameters are used to call the `/openchannel` API of the LSP node.
3. **Database Update**:\
   The response from the `/openchannel` API is recorded in the database, linking the `order_id` and `temporary_channel_id`. The channel is then marked with the status `PendingOpen`.
4. **Error Handling**:\
   In the event of an error during the channel creation process, the channel status is set to `FailedOpen`.

#### Re-checking Order Status via `get_order`

When the `get_order` is called again, the following steps occur:

1. **Order Retrieval**:\
   The order details are fetched from the database. The system checks the current status of the order.
2. **PendingOpen Status**:\
   If the channel status is `PendingOpen`, the `/getchannelid` API is called on the LSP node using the `temporary_channel_id`. This retrieves the `channel_id`.
3. **Channel Verification**:\
   The `/listchannels` API is then called to check whether the created channel exists in the array of open channels using the `channel_id`.
4. **Channel Found**:
   * If the channel is found, the status of the channel is updated to `Open`, and the order status is updated to `Success`.
   * The order information, including the channel details, is returned to the user in the `channel` field.
5. **Channel Not Found or Errors**:
   * If the channel is not found or an error occurs, the order status is set to `Failed`, and the channel status is set to `FailedOpen`.

#### Order Already Completed

If the channel has already been successfully created at the time of the `get_order` call, the order information along with the channel details is returned in the response.

***

#### Channel Statuses

* **PendingOpen**: The channel is being created but is not yet open.
* **Open**: The channel has been successfully created.
* **FailedOpen**: An error occurred during channel creation.
* **Success**: The order and channel creation process was successful.
* **Failed**: The order or channel creation failed.

***
