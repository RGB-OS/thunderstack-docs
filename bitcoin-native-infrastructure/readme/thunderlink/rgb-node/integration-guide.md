# Integration Guide

## Receive Lightning Invoice Payment Flow

This guide shows how to **create a Lightning invoice** (BTC or RGB asset), **pay it** (via UTEXO API), and **verify the updated balance** using `rgb-sdk`.

***

### Prerequisites

* **RGB SDK repository:** [https://github.com/RGB-OS/rgb-sdk/tree/alpha](https://github.com/RGB-OS/rgb-sdk/tree/alpha)
* **UTEXO API endpoint:** [https://rgb-node-demo.test.thunderstack.org](https://rgb-node-demo.test.thunderstack.org)
* **Install SDK (alpha tag):**

```bash
npm install rgb-sdk@alpha
```

#### Test Keys

Use these keys for testing purposes:

```ts
const keys = {
  mnemonic: "split capable text loop dream live outside suspect eight carry fine relax",
  account_xpub_vanilla:
    "tpubDCRtZV7Pyz1HCM2wkthBybjx3VQyz1sBdjQNZtCSh8yc1zhDWpUDBiLzsGsLMNPh48YSmWiEoXLF9tzQAyVkuKBoPngTEU83yM1xuG3gKdM",
  account_xpub_colored:
    "tpubDDgW33tfESzvQd2wtZeqTGqcmtUVSCBNGMUTMTFkUTNTnq65TZigYuCptsePDgvzSqAHfA7D1MnPLkBHYQ11VQV25H9faFxHGC25Pzrb7xB",
  master_fingerprint: "1f5bd7df",
};
```

#### Test Asset Id

```txt
rgb:tpk3SgP4-mRdSCNI-Pu~EDie-eBjfn5P-4HQdIKi-SUtqbe8
```

***

### Step 1. Initialize and Register Wallet

> **Important:** Wallet must be **uninitialized** before first use, then registered.

```ts
import { WalletManager } from "rgb-sdk";

const UTEXO_ENDPOINT = "https://node-api.thunderstack.org";
const NETWORK = "regtest"; // set your network

const wallet = new WalletManager({
  xpub_van: keys.account_xpub_vanilla,
  xpub_col: keys.account_xpub_colored,
  master_fingerprint: keys.master_fingerprint,
  mnemonic: keys.mnemonic,
  network: NETWORK,
  rgb_node_endpoint: UTEXO_ENDPOINT,
});

console.log("WalletManager initialized");

const registerResult = await wallet.registerWallet();
console.log("Wallet registered", registerResult);
```

***

### Step 2. Get On-chain Receive Address and Invoice

Use `onchainReceive()` to get a BTC address and asset invoice for on-chain deposits.

```javascript
const depositAddress = await wallet.onchainReceive();
console.log("BTC Address:", depositAddress.btc_address);
console.log("Asset Invoice:", depositAddress.asset_invoice);
```

**Example response:**

```json
{
  "btc_address": "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
  "asset_invoice": "rgb:1234567890abcdef...",
  "expires_at": "2026-01-08T12:00:00Z"
}
```

### Step 3. Check Balances (Optional)

`getBalance()` returns BTC and asset balances.

* **BTC Lightning receive** updates:
  * `balance.offchain_balance.total`
* **Asset Lightning receive** updates:
  * `asset_balances[].balance.offchain_outbound`

```ts
const balance = await wallet.getBalance();
console.log(balance);
```

Example response:

```json
{
  "balance": {
    "vanilla": { "settled": 0, "future": 0, "spendable": 0 },
    "colored": { "settled": 0, "future": 0, "spendable": 0 },
    "offchain_balance": {
      "total": 0,
      "details": []
    }
  },
  "asset_balances": [
    {
      "asset_id": "rgb:tpk3SgP4-mRdSCNI-Pu~EDie-eBjfn5P-4HQdIKi-SUtqbe8",
      "ticker": "TESTUSD",
      "precision": 2,
      "balance": {
        "settled": 0,
        "future": 0,
        "spendable": 0,
        "offchain_outbound": 0
      }
    }
  ]
}
```

***

### Step 4. Create a Lightning Invoice

#### Create a BTC Lightning invoice

```ts
const btcInvoice = await wallet.createLightningInvoice({
  amount_sats: 5000,
  expiry_seconds: 3600,
});

console.log("Invoice:", btcInvoice);
```

#### Create an Asset Lightning invoice

```ts
const assetInvoice = await wallet.createLightningInvoice({
  asset: {
    asset_id: "rgb:tpk3SgP4-mRdSCNI-Pu~EDie-eBjfn5P-4HQdIKi-SUtqbe8",
    amount: 10,
  },
  expiry_seconds: 3600,
});

console.log("Invoice:", assetInvoice);
```

Example response:

```json
{
  "id": "a743ed8d-2003-4c51-aa59-2faf07fb245c",
  "invoice": "lnbcrt100u1p54u9kxdqud3jxktt5w46x7unfv9kz6mn0v3jsnp4q27mxf0g6kkd0vdsrldhn0e4vs0hhq82pypqtqre49dqh84ekpqqypp5pw2utz5m2v9d3vlm0w0v9l6t4euemlms5kdc6xk6mzhc2vnldgaqsp5stu2h8v6vvs6sj4v2ad2dye4q2pag8ul6qj9lzvpnqk0y6v9f62s9qyysgqcqpcxqrgvcmwfs8akcqv3x9n88aq4nql2extpu075e6nl5v0ukw2eg25ucdngkvxtp2gw78h8up00avlf47dxgrz840ntftuxunwhyw5seqt627eqp3zy48x",
  "status": "OPEN",
  "payment_type": "BTC",
  "amount_sats": 10000,
  "asset": null,
  "created_at": "2026-01-07T08:18:14.841427Z"
}
```

***

### Step 5. Pay the Invoice (Test via cURL)

Replace `<invoice>` with the invoice string returned above.

```bash
curl -X 'POST' \
  'https://node-api.thunderstack.org/c17bc5d0-80b1-7050-5af5-dfd8a67834f1/c7f1ae1277124b5aab911ccf68321fee/sendpayment' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer EnYKDBgDIggKBggGEgIYDRIkCAASIF1J86DmEmBmcRSph4SegHX-xN5GSU12pK94w4yBavQeGkBq-ikpXXy2Ox2kpAwcYAcrVN9CNjOWXQJ7huxtx-GRfzVwijQqWHF3tzTKAIyTu9dLqbV1jXKzeubMEphQ098EIiIKIMns1a0RC71sE9DCRDpZ5BfhR4TrqXihQ-TDvAXBuF4F' \
  -d '{
    "invoice": "<invoice>"
  }'
```

***

### Step 6. Check Invoice Status

Use the invoice request `id` from **Step 3**.

```ts
const receiveStatus = await wallet.getLightningReceiveRequest(
  "a743ed8d-2003-4c51-aa59-2faf07fb245c"
);

console.log(receiveStatus);
```

Example response:

```json
{
  "id": "a743ed8d-2003-4c51-aa59-2faf07fb245c",
  "invoice": "lnbcrt100u1p54u9kxdqud3jxktt5w46x7unfv9kz6mn0v3jsnp4q27mxf0g6kkd0vdsrldhn0e4vs0hhq82pypqtqre49dqh84ekpqqypp5pw2utz5m2v9d3vlm0w0v9l6t4euemlms5kdc6xk6mzhc2vnldgaqsp5stu2h8v6vvs6sj4v2ad2dye4q2pag8ul6qj9lzvpnqk0y6v9f62s9qyysgqcqpcxqrgvcmwfs8akcqv3x9n88aq4nql2extpu075e6nl5v0ukw2eg25ucdngkvxtp2gw78h8up00avlf47dxgrz840ntftuxunwhyw5seqt627eqp3zy48x",
  "status": "SETTLED",
  "payment_type": "BTC",
  "amount_sats": 10000,
  "asset": null,
  "created_at": "2026-01-07T08:18:14.841427Z"
}
```

***

### Step 7. Confirm Balance Updated

After the invoice is settled, BTC Lightning balance should be updated.

```ts
const updatedBalance = await wallet.getBalance();
console.log(updatedBalance.balance.offchain_balance);
```

Example:

```json
{
  "total": 10000,
  "details": []
}
```

***

### Step 8. List Lightning Payments

Use `listLightningPayments()` to retrieve all Lightning payments (both inbound and outbound).

```javascript
const payments = await wallet.listLightningPayments();
console.log("Payments:", payments.payments);
```

**Example response:**

```json
{
  "payments": [
    {
      "amt_msat": 3000000,
      "asset_amount": 42,
      "asset_id": "rgb:tpk3SgP4-mRdSCNI-Pu~EDie-eBjfn5P-4HQdIKi-SUtqbe8",
      "payment_hash": "3febfae1e68b190c15461f4c2a3290f9af1dae63fd7d620d2bd61601869026cd",
      "inbound": true,
      "status": "Pending",
      "created_at": 1691160765,
      "updated_at": 1691162674,
      "payee_pubkey": "03b79a4bc1ec365524b4fab9a39eb133753646babb5a1da5c4bc94c53110b7795d"
    },
    {
      "amt_msat": 1000000,
      "asset_amount": null,
      "asset_id": null,
      "payment_hash": "abc123def456...",
      "inbound": false,
      "status": "Succeeded",
      "created_at": 1691160800,
      "updated_at": 1691162850,
      "payee_pubkey": "02abc123..."
    }
  ]
}
```

**Payment fields:**

* `amt_msat`: Amount in millisatoshis
* `asset_amount`: Asset amount (if asset payment)
* `asset_id`: Asset ID (if asset payment)
* `payment_hash`: Payment hash identifier
* `inbound`: `true` for received payments, `false` for sent payments
* `status`: Payment status (`Pending`, `Succeeded`, `Failed`)
* `created_at`: Timestamp when payment was created
* `updated_at`: Timestamp when payment was last updated
* `payee_pubkey`: Public key of the payee (recipient)

***

### Notes

* For **BTC Lightning receive**, check `balance.offchain_balance.total`.
* For **Asset Lightning receive**, check `asset_balances[].balance.offchain_outbound`.
* `getLightningReceiveRequest(id)` is the source of truth for invoice status: `OPEN`, `SETTLED`, `EXPIRED`, `FAILED`.

## Pay Lightning Invoice Flow

This guide shows how to **pay a Lightning invoice** (BTC or RGB asset) using `rgb-sdk`.

***

### Prerequisites

#### Create a test invoice (BTC)

```bash
curl -X 'POST' \
  'https://node-api.thunderstack.org/c17bc5d0-80b1-7050-5af5-dfd8a67834f1/c7f1ae1277124b5aab911ccf68321fee/lninvoice' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer EnYKDBgDIggKBggGEgIYDRIkCAASIF1J86DmEmBmcRSph4SegHX-xN5GSU12pK94w4yBavQeGkBq-ikpXXy2Ox2kpAwcYAcrVN9CNjOWXQJ7huxtx-GRfzVwijQqWHF3tzTKAIyTu9dLqbV1jXKzeubMEphQ098EIiIKIMns1a0RC71sE9DCRDpZ5BfhR4TrqXihQ-TDvAXBuF4F' \
  -d '{
    "amt_msat": 10000000,
    "expiry_sec": 420
  }'
```

#### Create a test invoice (asset)

> **Note:** Test liquidity for asset Lightning payments is limited. For testing, use amounts **below 80**.

```bash
curl -X 'POST' \
  'https://node-api.thunderstack.org/c17bc5d0-80b1-7050-5af5-dfd8a67834f1/c7f1ae1277124b5aab911ccf68321fee/lninvoice' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer EnYKDBgDIggKBggGEgIYDRIkCAASIF1J86DmEmBmcRSph4SegHX-xN5GSU12pK94w4yBavQeGkBq-ikpXXy2Ox2kpAwcYAcrVN9CNjOWXQJ7huxtx-GRfzVwijQqWHF3tzTKAIyTu9dLqbV1jXKzeubMEphQ098EIiIKIMns1a0RC71sE9DCRDpZ5BfhR4TrqXihQ-TDvAXBuF4F' \
  -d '{
    "asset_id": "rgb:tpk3SgP4-mRdSCNI-Pu~EDie-eBjfn5P-4HQdIKi-SUtqbe8",
    "amount": "10",
    "expiry_sec": 420
  }'
```

Example response:

```json
{
  "invoice": "lnbcrt100u1p54l4hddqud3jxktt5w46x7unfv9kz6mn0v3jsnp4qvfsflxyq4vq2nuwe23glrjde3y7rh0utgxx93n7fkk0h49l734t7pp5vxqqjne4mr6293n9jg34ywhahp6x8lv4epvxj3kpgax26v5dpzkssp5hs86lftqq0dl2lfda69nptkk4l7aqkx2d6kmtscze9xf9jf82akq9qyysgqcqpcxqzdye8uucd8ra8r2urmh9v79uyel4xdfd2l533l327jh76a28suxfvlj366ncagm6mx32drj6vmzdr0ky5nlxw6d4uk6q78448s3whc485qq9qzrtv"
}
```

***

### Step 1. Pay Lightning Invoice

This flow uses:

* `payLightningInvoiceBegin` — returns a **PSBT**
* `signPsbt` — sign PSBT using user private keys
* `payLightningInvoiceEnd` — broadcast the payment

Two reference implementations of `signPsbt`:

* [https://github.com/RGB-OS/rgb-sdk-rn/blob/main/src/crypto/signer.ts#L195](https://github.com/RGB-OS/rgb-sdk-rn/blob/main/src/crypto/signer.ts#L195)
* [https://github.com/RGB-OS/rgb-sdk/blob/main/src/crypto/signer.ts#L357](https://github.com/RGB-OS/rgb-sdk/blob/main/src/crypto/signer.ts#L357)

```ts
const base64Psbt = await wallet.payLightningInvoiceBegin({
  invoice:
    "lnbcrt100u1p54l4hddqud3jxktt5w46x7unfv9kz6mn0v3jsnp4qvfsflxyq4vq2nuwe23glrjde3y7rh0utgxx93n7fkk0h49l734t7pp5vxqqjne4mr6293n9jg34ywhahp6x8lv4epvxj3kpgax26v5dpzkssp5hs86lftqq0dl2lfda69nptkk4l7aqkx2d6kmtscze9xf9jf82akq9qyysgqcqpcxqzdye8uucd8ra8r2urmh9v79uyel4xdfd2l533l327jh76a28suxfvlj366ncagm6mx32drj6vmzdr0ky5nlxw6d4uk6q78448s3whc485qq9qzrtv",
});

console.log("Base64 PSBT:", base64Psbt);

const signedPsbt = await wallet.signPsbt(base64Psbt);

const payResult = await wallet.payLightningInvoiceEnd({
  signed_psbt: signedPsbt,
});

console.log("Pay result:", payResult);
```

Example response:

```json
{
  "id": "850a63d43aeee74d8766ff12ce6a67ca3ead36d2990b3bc39d07de736e5a34f0",
  "status": "PENDING",
  "payment_type": "BTC",
  "amount_sats": 5000,
  "asset": null,
  "fee_sats": null,
  "created_at": "2026-01-08T16:46:02.847838Z"
}
```

***

### Step 2. Check Send Request Status

Use the send request `id` returned in **Step 1**.

```ts
const sendStatus = await wallet.getLightningSendRequest(
  "850a63d43aeee74d8766ff12ce6a67ca3ead36d2990b3bc39d07de736e5a34f0"
);

console.log("Send status:", sendStatus);
```

Example response:

```json
{
  "id": "850a63d43aeee74d8766ff12ce6a67ca3ead36d2990b3bc39d07de736e5a34f0",
  "status": "SUCCEEDED",
  "payment_type": "BTC",
  "amount_sats": 5000,
  "asset": null,
  "fee_sats": null,
  "created_at": "2026-01-08T16:46:02Z"
}
```

***

### Step 3. Send On-chain from UTEXO

This flow uses:

* `onchainSendBegin` — returns a PSBT
* `signPsbt` — sign PSBT using user private keys
* `onchainSendEnd` — broadcast the on-chain send

Two reference implementations of `signPsbt`:

* https://github.com/RGB-OS/rgb-sdk-rn/blob/main/src/crypto/signer.ts#L195
* https://github.com/RGB-OS/rgb-sdk/blob/main/src/crypto/signer.ts#L357

```javascript
// Send BTC on-chain
const base64Psbt = await wallet.onchainSendBegin({
  address_or_rgbinvoice: "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
  amount_sats: 100000,
  fee_rate: 1
});

console.log("Base64 PSBT:", base64Psbt);

const signedPsbt = await wallet.signPsbt(base64Psbt);

const sendResult = await wallet.onchainSendEnd({
  signed_psbt: signedPsbt,
});

console.log("Send result:", sendResult);
```

**Example response:**

```json
{
  "send_id": "4334615b-6c53-4af5-a70a-8b5fc38614bf",
  "txid": "abc123def456..."
}
```

**Send Asset on-chain:**

```javascript
const base64Psbt = await wallet.onchainSendBegin({
  address_or_rgbinvoice: "rgb:1234567890abcdef...",
  fee_rate: 1,
  asset: {
    asset_id: "rgb:le3Ky3LQ-gM7hQKS-PW~zf2q-tBZxj7N-iaw5hbt-UoMQvAg",
    amount: 10
  }
});

const signedPsbt = await wallet.signPsbt(base64Psbt);

const assetSendResult = await wallet.onchainSendEnd({
  signed_psbt: signedPsbt,
});

console.log("Asset send result:", assetSendResult);
```

***

### Step 4. Check On-chain Send Status

Use the send ID returned in Step 3.

```javascript
const sendStatus = await wallet.getOnchainSendStatus(
  "4334615b-6c53-4af5-a70a-8b5fc38614bf"
);

console.log("On-chain send status:", sendStatus);
```

**Example response:**

```json
{
  "send_id": "4334615b-6c53-4af5-a70a-8b5fc38614bf",
  "status": "pending",
  "address_or_rgbinvoice": "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
  "amount_sats_requested": 100000,
  "amount_sats_sent": 99500,
  "asset": null,
  "close_txids": [],
  "sweep_txid": null,
  "fee_sats": 500,
  "timestamps": {
    "created": 1691160765,
    "completed": 1691162674
  },
  "error_code": null,
  "error_message": null,
  "retryable": false
}
```

***

### Step 5. List On-chain Transfers

Use `listOnchainTransfers()` to retrieve all on-chain transfers for a specific asset.

```javascript
const transfers = await wallet.listOnchainTransfers(
  "rgb:le3Ky3LQ-gM7hQKS-PW~zf2q-tBZxj7N-iaw5hbt-UoMQvAg"
);

console.log("On-chain transfers:", transfers);
```

**Example response:**

```json
[
  {
    "idx": 0,
    "batch_transfer_idx": 1,
    "created_at": 1691160765,
    "updated_at": 1691162674,
    "status": 2,
    "amount": 1000,
    "kind": 3,
    "txid": "abc123def456...",
    "recipient_id": "rgb:recipient...",
    "receive_utxo": {
      "txid": "def456...",
      "vout": 0
    },
    "change_utxo": null,
    "expiration": 1691164365,
    "transport_endpoints": []
  }
]
```

**Transfer fields:**

* `idx`: Transfer index
* `batch_transfer_idx`: Batch transfer index
* `created_at`: Timestamp when transfer was created
* `updated_at`: Timestamp when transfer was last updated
* `status`: Transfer status
* `amount`: Transfer amount
* `kind`: Transfer kind/type
* `txid`: Transaction ID
* `recipient_id`: Recipient identifier
* `receive_utxo`: Receive UTXO details (txid, vout)
* `change_utxo`: Change UTXO details (if any)
* `expiration`: Expiration timestamp
* `transport_endpoints`: Transport endpoints array

### Notes

* `payLightningInvoiceBegin()` returns a **base64 PSBT** that must be signed by the user.
* `getLightningSendRequest(id)` is the source of truth for payment status.<br>

## Alternative: Use UTEXO API Directly (Without RGB-SDK)

This page shows how to interact with UTEXO directly over HTTP, without using `rgb-sdk`.

***

### API References

* **Base URL:** `http://api.utexo.com`

#### Common Headers

All endpoints below require the following auth headers:

* `xpub-van` _(string, required)_
* `xpub-col` _(string, required)_
* `master-fingerprint` _(string, required)_

> **Tip:** Keep these headers consistent across requests. They identify the wallet context.

***

### GET /lightning/balance

#### Overview

Get lightning balance including BTC balance and asset balances.

#### Method

`GET`

#### Endpoint

`/lightning/balance`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/lightning/balance' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "balance": {
    "vanilla": {
      "settled": 1000000,
      "future": 500000,
      "spendable": 500000
    },
    "colored": {
      "settled": 500000,
      "future": 0,
      "spendable": 500000
    },
    "offchain_balance": {
      "total": 10000,
      "details": [
        {
          "expiry_time": "2024-01-02T12:00:00Z",
          "amount": 5000
        },
        {
          "expiry_time": "2024-01-03T12:00:00Z",
          "amount": 5000
        }
      ]
    }
  },
  "asset_balances": [
    {
      "asset_id": "rgb:1234567890abcdef...",
      "ticker": "USDT",
      "precision": 8,
      "balance": {
        "settled": 1000000,
        "future": 0,
        "spendable": 1000000,
        "offchain_outbound": 0
      }
    }
  ]
}
```

***

### POST /lightning/create-invoice

#### Overview

Creates a Lightning invoice for receiving BTC or asset payments over the Lightning Network.

#### Method

`POST`

#### Endpoint

`/lightning/create-invoice`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_
* `Content-Type: application/json`

#### Example Request (BTC)

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/create-invoice' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "amount_sats": 10000,
    "expiry_seconds": 3600
  }'
```

#### Example Request (Asset)

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/create-invoice' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "asset": {
      "asset_id": "rgb:1234567890abcdef...",
      "amount": 100
    },
    "expiry_seconds": 7200
  }'
```

#### Example Response

```json
{
  "id": "c3b6538c-79fd-4b03-b5dd-d00f42e893cb",
  "invoice": "lnbc1234567890...",
  "status": "OPEN",
  "payment_type": "BTC",
  "amount_sats": 10000,
  "created_at": "2024-01-01T12:00:00Z"
}
```

***

### GET /lightning/receive-request/{request\_id}

#### Overview

Returns the status of a Lightning invoice created with `create-invoice`. Supports both BTC and asset invoices.

#### Method

`GET`

#### Endpoint

`/lightning/receive-request/{request_id}`

#### Path Parameters

* `request_id` _(string, required)_ — The request ID of the Lightning invoice

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/lightning/receive-request/c3b6538c-79fd-4b03-b5dd-d00f42e893cb' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "id": "c3b6538c-79fd-4b03-b5dd-d00f42e893cb",
  "invoice": "lnbc1234567890...",
  "status": "SETTLED",
  "payment_type": "BTC",
  "amount_sats": 10000,
  "created_at": "2024-01-01T12:00:00Z"
}
```

***

### GET /lightning/send-request/{request\_id}

#### Overview

Returns the current status of a Lightning payment initiated with the pay-invoice flow. Works for both BTC and asset payments.

#### Method

`GET`

#### Endpoint

`/lightning/send-request/{request_id}`

#### Path Parameters

* `request_id` _(string, required)_ — The request ID of the Lightning send request

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/lightning/send-request/abc123-def456-ghi789' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "id": "abc123-def456-ghi789",
  "status": "SUCCEEDED",
  "payment_type": "BTC",
  "amount_sats": 10000,
  "fee_sats": 50,
  "created_at": "2024-01-01T12:00:00Z"
}
```

***

### POST /lightning/fee-estimate

#### Overview

Estimates the routing fee required to pay a Lightning invoice. For asset payments, the returned fee is always denominated in satoshis.

#### Method

`POST`

#### Endpoint

`/lightning/fee-estimate`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_
* `Content-Type: application/json`

#### Example Request (BTC)

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/fee-estimate' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "invoice": "lnbc1234567890..."
  }'
```

#### Example Request (Asset)

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/fee-estimate' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "invoice": "lnbc1234567890...",
    "asset": {
      "asset_id": "rgb:1234567890abcdef...",
      "amount": 100
    }
  }'
```

#### Example Response

```json
50
```

***

### POST /lightning/pay-invoice-begin

#### Overview

Begins a Lightning invoice payment process. Returns the PSBT (should be signed by user).

#### Method

`POST`

#### Endpoint

`/lightning/pay-invoice-begin`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_
* `Content-Type: application/json`

#### Example Request

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/pay-invoice-begin' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "invoice": "lnbc1234567890...",
    "max_fee_sats": 1000
  }'
```

#### Example Response

```json
"cHNidP8BAHwBAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
```

***

### POST /lightning/pay-invoice-end

#### Overview

Completes a Lightning invoice payment using signed PSBT. Works the same as pay-invoice but uses `signed_psbt` instead of invoice.

#### Method

`POST`

#### Endpoint

`/lightning/pay-invoice-end`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_
* `Content-Type: application/json`

#### Example Request

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/pay-invoice-end' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "signed_psbt": "cHNidP8BAHwBAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
  }'
```

#### Example Response

```json
{
  "id": "abc123-def456-ghi789",
  "status": "PENDING",
  "payment_type": "BTC",
  "amount_sats": 10000,
  "fee_sats": 50,
  "created_at": "2024-01-01T12:00:00Z"
}
```

***

### GET /lightning/listpayments

#### Overview

Lists Lightning payments.

#### Method

`GET`

#### Endpoint

`/lightning/listpayments`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/lightning/listpayments' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "payments": [
    {
      "amt_msat": 3000000,
      "asset_amount": 42,
      "asset_id": "rgb:CJkb4YZw-jRiz2sk-~PARPio-wtVYI1c-XAEYCqO-wTfvRZ8",
      "payment_hash": "3febfae1e68b190c15461f4c2a3290f9af1dae63fd7d620d2bd61601869026cd",
      "inbound": true,
      "status": "Pending",
      "created_at": 1691160765,
      "updated_at": 1691162674,
      "payee_pubkey": "03b79a4bc1ec365524b4fab9a39eb133753646babb5a1da5c4bc94c53110b7795d"
    }
  ]
}
```

***

### GET /lightning/onchain-receive

#### Overview

Get an on-chain receive address for receiving assets. Returns a BTC address and asset invoice that can be used for on-chain deposits.

#### Method

`GET`

#### Endpoint

`/lightning/onchain-receive`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/lightning/onchain-receive' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "btc_address": "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
  "asset_invoice": "rgb:1234567890abcdef...",
  "expires_at": "2024-01-02T12:00:00Z"
}
```

***

### GET /lightning/balance

#### Overview

Get wallet balance including BTC balance and asset balances.

#### Method

`GET`

#### Endpoint

`/lightning/balance`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/lightning/balance' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "balance": {
    "vanilla": {
      "settled": 1000000,
      "future": 500000,
      "spendable": 500000
    },
    "colored": {
      "settled": 500000,
      "future": 0,
      "spendable": 500000
    },
    "offchain_balance": {
      "total": 10000,
      "details": [
        {
          "expiry_time": "2024-01-02T12:00:00Z",
          "amount": 5000
        },
        {
          "expiry_time": "2024-01-03T12:00:00Z",
          "amount": 5000
        }
      ]
    }
  },
  "asset_balances": [
    {
      "asset_id": "rgb:1234567890abcdef...",
      "ticker": "USDT",
      "precision": 8,
      "balance": {
        "settled": 1000000,
        "future": 0,
        "spendable": 1000000,
        "offchain_outbound": 0
      }
    }
  ]
}
```

***

### POST /lightning/settle

#### Overview

Settle balances in the wallet.

#### Method

`POST`

#### Endpoint

`/lightning/settle`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/settle' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "result": "success"
}
```

***

### POST /lightning/onchain-send-begin

#### Overview

Begins an on-chain send process from UTEXO. Returns the request encoded as base64 (mock PSBT). Later this should construct and return a real base64 PSBT.

#### Method

`POST`

#### Endpoint

`/lightning/onchain-send-begin`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_
* `Content-Type: application/json`

#### Example Request (BTC)

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/onchain-send-begin' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "address_or_rgbinvoice": "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
    "amount_sats": 100000,
    "fee_rate": 1
  }'
```

#### Example Request (Asset)

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/onchain-send-begin' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "address_or_rgbinvoice": "rgb:1234567890abcdef...",
    "fee_rate": 1,
    "asset": {
      "asset_id": "rgb:1234567890abcdef...",
      "amount": 100
    }
  }'
```

#### Example Response

```json
"cHNidP8BAHwBAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
```

***

### POST /lightning/onchain-send-end

#### Overview

Completes an on-chain send from UTEXO using signed PSBT.

#### Method

`POST`

#### Endpoint

`/lightning/onchain-send-end`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_
* `Content-Type: application/json`

#### Example Request

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/onchain-send-end' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "signed_psbt": "cHNidP8BAHwBAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
  }'
```

#### Example Response

```json
{
  "send_id": "4334615b-6c53-4af5-a70a-8b5fc38614bf",
  "txid": "abc123def456..."
}
```

***

### GET /onchain-send/{send\_id}

#### Overview

Gets the status of an on-chain send by send ID.

#### Method

`GET`

#### Endpoint

`/onchain-send/{send_id}`

#### Path Parameters

* `send_id` _(string, required)_ — The on-chain send ID

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/onchain-send/4334615b-6c53-4af5-a70a-8b5fc38614bf' \
  -H 'accept: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>'
```

#### Example Response

```json
{
  "send_id": "4334615b-6c53-4af5-a70a-8b5fc38614bf",
  "status": "pending",
  "address_or_rgbinvoice": "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
  "amount_sats_requested": 100000,
  "amount_sats_sent": 99500,
  "asset": null,
  "close_txids": [],
  "sweep_txid": null,
  "fee_sats": 500,
  "timestamps": {
    "created": 1691160765,
    "completed": 1691162674
  },
  "error_code": null,
  "error_message": null,
  "retryable": false
}
```

***

### POST /lightning/listtransfers

#### Overview

Lists on-chain transfers for a specific asset.

#### Method

`POST`

#### Endpoint

`/lightning/listtransfers`

#### Headers

* `xpub-van` _(required)_
* `xpub-col` _(required)_
* `master-fingerprint` _(required)_
* `Content-Type: application/json`

#### Example Request

```bash
curl -X 'POST' \
  'http://api.utexo.com/lightning/listtransfers' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'xpub-van: <XPUB_VAN>' \
  -H 'xpub-col: <XPUB_COL>' \
  -H 'master-fingerprint: <MASTER_FINGERPRINT>' \
  -d '{
    "asset_id": "rgb:1234567890abcdef..."
  }'
```

#### Example Response

```json
[
  {
    "idx": 0,
    "batch_transfer_idx": 1,
    "created_at": 1691160765,
    "updated_at": 1691162674,
    "status": 2,
    "amount": 1000,
    "kind": 3,
    "txid": "abc123...",
    "recipient_id": "rgb:recipient...",
    "receive_utxo": {
      "txid": "def456...",
      "vout": 0
    },
    "change_utxo": null,
    "expiration": 1691164365,
    "transport_endpoints": []
  }
]
```

***

### Notes

* `pay-invoice-begin` returns a **PSBT** that must be signed client-side.
* Use `pay-invoice-end` to submit the signed PSBT and initiate/broadcast the payment.
* Use `GET /lightning/send-request/{request_id}` as the source of truth for payment status.
* Use `GET /lightning/receive-request/{request_id}` as the source of truth for invoice status.
