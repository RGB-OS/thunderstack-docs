# Webhook

### Webhook Integration: Triggering Transfer Completion

The **RGB Client Server** supports incoming webhook calls that enable external systems (such as payment verifiers, order processors, or off-chain KYC platforms) to **programmatically trigger the finalization of RGB asset transfers**.

#### Purpose

Webhooks are used to notify the Client Server that a transfer is approved and ready to be completed. Upon receiving a valid webhook request, the server will invoke `wallet.send(...)`, which finalizes the transfer by signing and broadcasting the transaction.

***

#### Webhook Authentication

To protect against unauthorized access, the webhook endpoint requires a valid secret to be included in the request header:

* **Header**: `x-webhook-secret`
* **Value**: Must match the `WEBHOOK_SECRET` environment variable defined in the server.

***

#### &#x20;Endpoint

```http
POST /webhook/transfer-confirmed
```

#### Request Body

```json
{
  "recipient_id": "bcrt:utxob:abc123...",
  "asset_id": "rgb:XYZ456...",
  "amount": 1234
}
```

| Field          | Description                                 |
| -------------- | ------------------------------------------- |
| `recipient_id` | Blinded UTXO to receive the RGB asset       |
| `asset_id`     | The RGB asset identifier                    |
| `amount`       | Transfer amount (in smallest unit of asset) |

***
