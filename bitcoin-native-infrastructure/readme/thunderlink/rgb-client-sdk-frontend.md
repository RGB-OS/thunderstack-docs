# RGB Client SDK (Frontend)

The Client SDK is a React library that developers include in their web application or e-commerce site. It provides an easy way to integrate ThunderLink’s functionality into the user interface – this is analogous to Stripe’s Checkout JS, PayPal Buttons, or Coinbase Commerce’s web integration. It handles displaying payment widgets, collecting necessary info from ThunderLink services, and updating the UI based on transaction status.

UI/UX &#x20;

The ThunderLink SDK provides a complete set of UI components designed to offer a seamless and intuitive RGB asset payment experience. These components are fully customizable and easy to integrate into existing merchant checkout flows.

**1 ."Pay with RGB"**&#x20;

The Pay with RGB button serves as the entry point for initiating the ThunderLink payment flow. It is designed to be visually recognizable and easily embeddable into checkout pages, product listings, or in-app purchase flows.

**Key Features:**

* **Customizable Appearance:** The button's size, color, and label can be tailored to match the merchant's branding while maintaining visual consistency across implementations.
* **Branding Options:** Supports inclusion of the RGB logo or a Bitcoin symbol alongside the label (e.g., "Pay with RGB", "Buy with RGB Token").
* **Usage Pattern:** On click, the SDK launches the payment request widget (modal or embedded view) and begins the transaction flow with ThunderLink.
* Payment Confirmation and Feedback: Once the user sends the payment from their wallet, the widget will update in real-time.

**2.Payment Request Widget (Modal/Popup)**

The payment widget is responsible for presenting the invoice and guiding the user through the RGB payment process. It can be displayed as a modal popup or embedded inline within the application.

**Core Functionality:**

* **Invoice Summary:** Displays the amount due in the RGB asset, along with an optional fiat or BTC equivalent for user clarity.
* **QR Code & Payment Data:** Renders a QR code and/or invoice string (bech32, blinded UTXO, or address) that can be scanned or copied by the user. The format depends on the RGB protocol implementation.
* **Asset Details:** Clearly shows the asset name (e.g., "50 XYZ Tokens") and associated metadata.
* **Expiration Timer:** If the invoice is time-limited, a countdown is shown to prompt the user to complete the payment in a defined window (e.g., "Please pay within 15 minutes").
* **Status Feedback:** Optionally includes visual feedback for “waiting for payment,” “confirmed,” or “expired” states.

\
