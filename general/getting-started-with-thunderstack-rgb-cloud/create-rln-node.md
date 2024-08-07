# Create RLN Node

**Step 1: Initiate RLN Node Creation**

1.  **Access the Nodes Page**:

    * Navigate to the `/nodes` page on Thunderstack.org. Here, you'll find an overview of your current nodes and the option to create new ones.

    <figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
2. **Create a New Node**:
   * Click on the "Create Node" button. This action will redirect you to a new page where you can begin the node creation process.

**Step 2: Configure Your RLN Node**

1. **Fill Out the Node Creation Form**:
   * On the redirected page, you will see a form requiring you to enter the name for your new node. This name will help you identify the node in your dashboard and should be unique to each node you create.
2. **Submit the Form**:
   *   After entering a name, click the "Create" button to submit the form and start the node creation process.

       <figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

**Step 3: Monitor Node Creation Status**

1.  **Check Initial Status**:

    * Upon successful submission of the form, a new item will be added to the nodes table on the `/nodes` page. Initially, this new node will display a status of `IN_PROGRESS`. During this phase, the node is being built, and you will not be able to perform any operations with it.  Also, users can navigate to the individual node page by visiting `/nodes/{nodeId}`. On this page, you can view detailed information about the node, including its current status, build progress, endpoint details, and other configurable options. This allows you to monitor the node’s setup process closely and access specific features once the node status changes to `RUNNING`.

    <figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
2. **Understanding Statuses**:
   * **IN\_PROGRESS**: This status indicates that the node is currently under construction. No interactions can be made with the node during this time.
   * **RUNNING**: Once the node has been successfully built, its status will change from `IN_PROGRESS` to `RUNNING`. At this point, the node is fully operational, and you can start using it for its intended functions.

**Step 4: Final Verification**

Once the status changes to `RUNNING`, verify that the node appears correctly in your dashboard and that you can access its features and settings.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

