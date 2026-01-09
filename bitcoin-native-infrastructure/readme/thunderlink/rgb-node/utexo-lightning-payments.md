# UTEXO Lightning Payments

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
