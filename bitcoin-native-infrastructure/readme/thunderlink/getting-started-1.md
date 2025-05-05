# Getting Started

Before integrating the ThunderLink Client SDK into your frontend, you must first deploy and configure the **RGB Client Server**. This server acts as a secure intermediary between the frontend SDK and the ThunderLink Cloud infrastructure. It handles private key access, transaction signing, and UTXO management.

#### Step 1: Deploy the RGB Client Server

The RGB Client Server can be deployed in one of two ways:

* As a **Docker container** (recommended for most environments)
* As a **Node.js application** running directly on your infrastructure

> 📌 **Note:** This server must run on infrastructure controlled by you (the merchant or exchange), as it holds or has access to sensitive data such as signing keys and authorization logic.

#### Step 2: Configure Environment Variables

The server is configured via environment variables to match your operational and security requirements. Key options include:

| Variable               | Description                                                               |
| ---------------------- | ------------------------------------------------------------------------- |
| `MNEMONIC`             | Mnemonic phrase for deriving Bitcoin keys (used for signing transactions) |
| `XPUB`                 | Extended public key used by ThunderLink RGB Manager for address tracking  |
| `UNSETTLED_UTXO_LIMIT` | Minimum number of available UTXOs to maintain                             |
| `ENABLE_WATCHERS`      | Whether background watchers should run (`true` or `false`)                |
| `WATCHER_INTERVAL_MS`  | Polling interval for checking transfer status (in milliseconds)           |
| `PORT`                 | The local port to expose SDK endpoints                                    |

> 🔐 **Custom Auth:** The server includes a pluggable authentication interface. You can implement your own logic to authorize SDK calls (e.g., using sessions, API keys, or JWTs).

#### Step 3: Start the Server

Run the service using your preferred method using Docker or Node.js

## Client SDK Usage Instructions

This directory contains a powerful RGB payment processing library that allows your application to accept RGB asset payments.

### Installation

After publishing the package to npm, you can install it in your project:

```bash
npm install rgb-connect-sdk
# or
yarn add rgb-connect-sdk
# or
pnpm add rgb-connect-sdk
```

### Basic Usage

#### 1. Wrap your application with `RgbPaymentProvider`

```jsx
import { RgbPaymentProvider } from 'rgb-connect-sdk';

function App() {
  return (
    <RgbPaymentProvider baseUrl="http://your-rgb-server.com">
      {/* Your application components */}
    </RgbPaymentProvider>
  );
}
```

#### 2. Use the `RgbPaymentButton` component

```jsx
import { RgbPaymentButton } from 'rgb-connect-sdk';

function ProductPage() {
  const handlePaymentSuccess = () => {
    console.log('Payment successful!');
    // Handle success (e.g., show success message, redirect to thank you page)
  };
  
  const handlePaymentFailure = () => {
    console.log('Payment failed');
    // Handle failure (e.g., show error message, allow retry)
  };

  return (
    <div>
      <h1>Product: RGB Widget</h1>
      <p>Price: 100 RGB tokens</p>
      
      <RgbPaymentButton 
        amount={100}
        onPaymentSuccess={handlePaymentSuccess}
        onPaymentFailure={handlePaymentFailure}
        buttonText="Buy Now with RGB" // Optional: customize button text
      />
    </div>
  );
}
```

### Advanced Usage

#### Using with ShadCN UI or other component libraries

The package can use your app's UI components for better integration:

```jsx
import { PaymentModal } from 'rgb-connect-sdk';
import { Button } from '@/components/ui/button';
import { Dialog, DialogContent, DialogTitle } from '@/components/ui/dialog';
import { toast } from '@/hooks/use-toast';
import { useState } from 'react';

function CheckoutPage() {
  const [isOpen, setIsOpen] = useState(false);
  
  return (
    <>
      <Button onClick={() => setIsOpen(true)}>
        Pay Now
      </Button>
      
      <PaymentModal
        open={isOpen}
        onOpenChange={setIsOpen}
        amount={100}
        // Pass your UI components
        Dialog={Dialog}
        DialogContent={DialogContent}
        DialogTitle={DialogTitle}
        toast={toast}
        onPaymentSuccess={() => {
          toast({
            title: "Payment Successful",
            description: "Your RGB payment has been completed."
          });
        }}
      />
    </>
  );
}
```

#### Performance Optimizations

The `RgbPaymentProvider` automatically caches available assets to prevent unnecessary API calls when opening payment modals. The assets are fetched once when the provider initializes and are available for all components that need them.

```jsx
const { assets, refreshAssets } = useRgbPayment();

// If you need to manually refresh the assets list
const handleRefresh = () => {
  refreshAssets();
};

// Access the cached assets directly
console.log(`Available assets: ${assets.length}`);
```

#### Using the API directly

If you need more control, you can use the API client directly:

```jsx
import { useRgbPayment } from 'rgb-connect-sdk';

function CustomPaymentComponent() {
  const { api } = useRgbPayment();
  
  const handleCreateInvoice = async () => {
    try {
      // Use the cached assets instead of making a new request
      const { assets } = useRgbPayment();
      console.log('Available assets:', assets);
      
      // Choose an asset and create an invoice
      if (assets.length > 0) {
        const assetId = assets[0].asset_id;
        const invoice = await api.createInvoice(assetId, 100);
        console.log('Invoice created:', invoice);
        // Do something with the invoice
      }
    } catch (error) {
      console.error('Error:', error);
    }
  };
  
  return (
    <button onClick={handleCreateInvoice}>
      Create Custom Invoice
    </button>
  );
}
```

#### User Interface Improvements

The library provides a streamlined user interface that:

1. Shows QR codes for easy scanning with RGB wallet apps
2. Displays concise transaction information without unnecessary details
3. Uses input fields for invoice strings to save space and provide a better copy experience
4. Shows a minimal payment status view with relevant information only

#### Tracking Payment Status

You can use the `usePaymentStatus` hook for tracking payment status:

```jsx
import { usePaymentStatus } from 'rgb-connect-sdk';
import { useEffect } from 'react';

function PaymentTracker({ invoice }) {
  const { transfer, timeLeft, startStatusPolling } = usePaymentStatus({
    invoice,
    onPaymentSuccess: () => console.log('Payment successful!'),
    onPaymentFailure: () => console.log('Payment failed'),
  });
  
  useEffect(() => {
    if (invoice) {
      startStatusPolling(invoice);
    }
  }, [invoice, startStatusPolling]);
  
  // Render status UI
  return (
    <div>
      <p>Payment Status: {transfer ? transfer.status : 'Waiting for payment'}</p>
      <p>Time left: {timeLeft}</p>
    </div>
  );
}
```

### Server Integration

To fully implement RGB payments, you'll need a server that handles:

1. Invoice generation
2. Payment processing
3. Status verification
4. Asset listing

Ensure your server exposes these endpoints:

* `POST /api/invoice/create` - Create a new invoice
* `POST /api/invoice/status` - Check payment status
* `POST /api/wallet/listassets` - Get list of available RGB assets

The package expects the server responses to match the interface types defined in the library.
