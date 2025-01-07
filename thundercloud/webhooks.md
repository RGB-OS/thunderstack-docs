---
description: >-
  The ThunderStack Platform supports webhooks to notify users about events in
  their nodes. This document will explain how to use ThunderStack webhooks as
  well as provide example payloads.
icon: webhook
---

# Webhooks

#### Adding a Webhook Endpoint

You can add a webhook endpoint to your node by editing the node's settings. First, navigate to your node and click **Settings**. Once your settings are expanded, there is a field named **Webhook URL**. Fill in this field with the endpoint that you'd like ThunderStack to make requests to. While you're in the settings, make sure to note down your public key, as it will be used to verify the requests.

**Adding a Webhook When Creating a Node:**

When creating a new node through API, you can  provide a **Webhook URL in settings** . Enter the endpoint where you would like ThunderStack to send webhook notifications. You can update this URL later through the node's settings if needed.

**URL to Retrieve Public Key**\
`https://cloud-api.thunderstack.org/api/webhook-public-key`

This public key will be used to validate incoming requests from ThunderStack by verifying the `X-ThunderStack-Signature` header included in each webhook event.

#### How it Works

When an event is triggered, ThunderStack will make a `POST` request to your webhook endpoint. This request will include a JSON payload with details about the event. Additionally, the request will contain a header named `X-ThunderStack-Signature`, which includes a digital signature created with our private key. When receiving the webhook event, you should verify the `X-ThunderStack-Signature` header using the public key.

\
You should always validate the `X-ThunderStack-Signature` header and the JSON payload. This ensures the request was made from ThunderStack and is associated with the expected node or endpoint.

#### Payload Schema

The JSON payload sent to your webhook endpoint will have the following structure:

```json
{
  "eventType": "NODE_STATUS_UPDATED", // Indicates the type of event
  "eventTimestamp": "2024-11-06T12:00:00Z", // ISO 8601 format timestamp
  "details": {
    "nodeId": "your-node-id", // Unique identifier for the node
    "status": "RUNNING" // Current status of the node (one of the predefined statuses)
  },
  "nodeId": "your-node-id" // Unique identifier for the node
}
```

#### Status Values

The `status` field within `details` can have one of the following values:

* **RUNNING**: The node is running.
* **STARTING**: The node is starting.
* **PAUSED**: The node is paused.
* **FAILED**: The node has failed.
* **IN\_PROGRESS**: An operation is currently in progress.

#### Security Considerations

* **HTTPS**: Always use HTTPS for your webhook endpoint to ensure data security during transit.
* **Signature Verification**: Validate the `X-ThunderStack-Signature` header using the public key.
* **API Validation**: Check the `nodeId` field to ensure it matches the expected node.

This approach ensures secure, reliable, and real-time notifications for your nodes using the ThunderStack webhook system.

**Example Webhook Handler**

```javascript
import * as crypto from 'crypto';

// Main function to handle webhook events
export async function handleWebhookEvent(request: Request): Promise<any> {
        // Extract the signature and body
        const receivedSignature = request.headers?.['x-thunderstack-signature'] || request.headers?.['X-ThunderStack-Signature'];
        const body = typeof request.body === 'string' ? request.body : JSON.stringify(request.body);

        // Retrieve the public key for signature verification
        const publicKey = 'YOUR_PUBLIC_KEY_HERE'

        // Create a verifier to validate the signature using the public key
        const verify = crypto.createVerify('RSA-SHA256');
        verify.update(body);
        verify.end();

        // Validate the signature
        const isValid = verify.verify(publicKey, receivedSignature, 'base64');
        if (!isValid) {
            console.error('Invalid signature');
            // Example Handle Invalid Signature Error
             return {
                statusCode: 403,
                body: JSON.stringify({ message: 'Forbidden - Invalid Signature' })
            };
             
        }
        // Handle the validated webhook event here 
}


```
