---
icon: magnifying-glass
---

# Node UI Actions

### **RGB Lightning Node Actions on UI**

On the **Node Page** (`/node/{nodeId}`), users have access to several interactive actions: `/init`, `/lock`, `/unlock`, and `/nodeInfo`. These actions are facilitated through dedicated API Gateway routes specific to each node.

***

#### **Available Actions for Running Nodes**

Users managing their running nodes through the UI can utilize a range of actions to control node operations. The interface provides clearly labeled buttons and forms, accompanied by input fields and prompts (e.g., password fields for locking and unlocking). These intuitive elements guide users through each process.

**1. /nodeInfo**

* **Functionality**: Automatically retrieves and displays the node's current status each time the node page is opened.
* **Error Handling**: If the node is locked or encounters other issues, the UI displays an appropriate error message to inform the user.
* **Use Case**: Provides real-time visibility into the node's status and helps diagnose any operational issues.

***

**2. /init**

* **Functionality**: Initializes the node after its creation, making it ready for operation.
* **Process**:
  * During initialization, the system generates a **mnemonic** and provides it to the user.
  * The mnemonic is a critical security feature and must be securely saved by the user.
* **Importance**: The mnemonic is required for:
  * Recovering the node in case of future issues.
  * Ensuring secure access in all subsequent interactions.
* **Use Case**: Mandatory step after node creation to enable further actions.

***

**3. /lock**

* **Functionality**: Secures the node by locking it, preventing unauthorized access.
* **Process**:
  * The user enters a password to lock the node.
  * The node remains locked until the correct password is provided to unlock it.
* **Use Case**: Protects sensitive node operations when the node is not actively in use.

***

**4. /unlock**

* **Functionality**: Unlocks the node, allowing it to resume normal operations.
* **Process**:
  * The user enters the correct password to reverse the lock status.
  * Once unlocked, the node becomes fully operational.
* **Use Case**: Enables the user to access and manage the node after it has been secured.
