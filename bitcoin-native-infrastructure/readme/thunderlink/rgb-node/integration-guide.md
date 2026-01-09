# Integration Guide

## Receive Lightning Invoice Payment Flow

This guide shows how to **create a Lightning invoice** (BTC or RGB asset), **pay it** (via UTEXO API), and **verify the updated balance** using `rgb-sdk`.

***

### Prerequisites

* **RGB SDK repository:** [https://github.com/RGB-OS/rgb-sdk](https://github.com/RGB-OS/rgb-sdk)
* **UTEXO API endpoint:** [https://node-api.thunderstack.org](https://node-api.thunderstack.org/)
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
rgb:le3Ky3LQ-gM7hQKS-PW~zf2q-tBZxj7N-iaw5hbt-UoMQvAg
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

### Step 2. Check Balances (Optional)

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
      "asset_id": "rgb:le3Ky3LQ-gM7hQKS-PW~zf2q-tBZxj7N-iaw5hbt-UoMQvAg",
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

### Step 3. Create a Lightning Invoice

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
    asset_id: "rgb:le3Ky3LQ-gM7hQKS-PW~zf2q-tBZxj7N-iaw5hbt-UoMQvAg",
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

### Step 4. Pay the Invoice (Test via cURL)

Replace `<invoice>` with the invoice string returned above.

```bash
curl -X 'POST' \
  'https://node-api.thunderstack.org/c17bc5d0-80b1-7050-5af5-dfd8a67834f1/99fa256286d34f59b9b65551d30da878/sendpayment' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <TOKEN>' \
  -d '{
    "invoice": "<invoice>"
  }'
```

***

### Step 5. Check Invoice Status

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

### Step 6. Confirm Balance Updated

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
  'https://cognito-node-api.thunderstack.org/nodes/c17bc5d0-80b1-7050-5af5-dfd8a67834f1/99fa256286d34f59b9b65551d30da878/lninvoice' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <TOKEN>' \
  -d '{
    "amt_msat": 10000000,
    "expiry_sec": 420
  }'
```

#### Create a test invoice (asset)

> **Note:** Test liquidity for asset Lightning payments is limited. For testing, use amounts **below 80**.

```bash
curl -X 'POST' \
  'https://cognito-node-api.thunderstack.org/nodes/c17bc5d0-80b1-7050-5af5-dfd8a67834f1/99fa256286d34f59b9b65551d30da878/lninvoice' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <TOKEN>' \
  -d '{
    "asset_id": "rgb:le3Ky3LQ-gM7hQKS-PW~zf2q-tBZxj7N-iaw5hbt-UoMQvAg",
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

### GET /wallet/balance

#### Overview

Get wallet balance including BTC balance and asset balances.

#### Method

`GET`

#### Endpoint

`/wallet/balance`

#### Headers

* `xpub-van` _(string, required)_
* `xpub-col` _(string, required)_
* `master-fingerprint` _(string, required)_

#### Example Request

```bash
curl -X 'GET' \
  'http://api.utexo.com/wallet/balance' \
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

Creates a Lightning invoice for receiving **BTC** or **asset** payments over the Lightning Network.

#### Method

`POST`

#### Endpoint

`/lightning/create-invoice`

#### Headers

* `xpub-van` _(string, required)_
* `xpub-col` _(string, required)_
* `master-fingerprint` _(string, required)_
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

* `request_id` _(string, required)_ — Receive request ID returned by **create-invoice**

#### Headers

* `xpub-van` _(string, required)_
* `xpub-col` _(string, required)_
* `master-fingerprint` _(string, required)_

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

Returns the current status of a Lightning payment initiated with `pay-invoice-begin`/`pay-invoice-end`.

Works for both BTC and asset payments.

#### Method

`GET`

#### Endpoint

`/lightning/send-request/{request_id}`

#### Path Parameters

* `request_id` _(string, required)_ — Send request ID returned by **pay-invoice-end**

#### Headers

* `xpub-van` _(string, required)_
* `xpub-col` _(string, required)_
* `master-fingerprint` _(string, required)_

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

### POST /lightning/pay-invoice-begin

#### Overview

Begins a Lightning invoice payment process and returns a **PSBT** that must be signed by the user.

#### Method

`POST`

#### Endpoint

`/lightning/pay-invoice-begin`

#### Headers

* `xpub-van` _(string, required)_
* `xpub-col` _(string, required)_
* `master-fingerprint` _(string, required)_
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

```txt
cHNidP8BAHwBAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=
```

***

### POST /lightning/pay-invoice-end

#### Overview

Completes a Lightning invoice payment using a **signed PSBT**.

Works the same as a pay-invoice flow, but uses `signed_psbt` instead of the invoice.

#### Method

`POST`

#### Endpoint

`/lightning/pay-invoice-end`

#### Headers

* `xpub-van` _(string, required)_
* `xpub-col` _(string, required)_
* `master-fingerprint` _(string, required)_
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

### Notes

* `pay-invoice-begin` returns a **PSBT** that must be signed client-side.
* Use `pay-invoice-end` to submit the signed PSBT and initiate/broadcast the payment.
* Use `GET /lightning/send-request/{request_id}` as the source of truth for payment status.
* Use `GET /lightning/receive-request/{request_id}` as the source of truth for invoice status.
