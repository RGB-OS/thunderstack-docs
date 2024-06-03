# Node Actions

#### RGB Lightning Node Actions on UI

On the node page /node/{nodeId} users are provided with several interactive actions—/init, /lock, /unlock, and /nodeInfo. These functions are enabled through dedicated API Gateway routes established for each node.

**Available Actions for Runner Nodes**

Users who access their running nodes on UI can utilize several actions to manage node operations. Users can interact with these functions through clearly labeled buttons or forms on the node’s specific page. Input fields and prompts, such as password fields for locking and unlocking, guide the user through each process:

* **/nodeInfo**: Automatically retrieves and displays the node's current status each time the node page is opened. If there are any issues, such as the node being locked or other errors, the UI will show an appropriate error message.
* **/init**: This function is used to initialize the node. It is necessary to call this function after the creation of the node to work with it. During initialization, a mnemonic is generated and provided to the user. This mnemonic is a crucial security feature that must be securely saved, as it is required for recovery operations and to ensure secure access in future interactions with the node.
* **/lock**: Secures the node by locking it, requiring a password to prevent unauthorized access. The node remains locked until the correct password is entered to unlock it.
* **/unlock**: Reverses the lock status, allowing the node to resume normal operations once the correct password is entered.
