---
icon: circle-xmark
---

# Destroy your Node

**Destruction Process**

1.  **Initiate Node Destruction**:

    * To begin the destruction process, navigate to the specific node's page by accessing `/nodes/{nodeId}`.

    <figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
2. **Check Node Status**:
   * Ensure that the node is in a `RUNNING` status. Nodes in this state are eligible for destruction.
3. **Destroy the Node**:
   * Click the "Destroy" button available on the page. This action will initiate the destruction process, and the node’s status will change to `IN_PROGRESS` indicate that the destruction is underway.
4. **Final Status Update**:
   * Once the node has been successfully destroyed, the status will update to `DESTROYED`. This confirms that the node has been decommissioned and all associated resources have been properly cleaned up.
