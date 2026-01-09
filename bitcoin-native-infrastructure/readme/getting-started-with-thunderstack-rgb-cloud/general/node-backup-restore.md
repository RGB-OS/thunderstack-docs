---
icon: lock-keyhole
---

# Node Backup/Restore

**Backup Creation**

1. **Access Backup Functionality**: Users can manage backups for their nodes by navigating to the specific node’s backup page at `nodes/{nodeId}/backup`.
2. **Create a Backup**:
   * If no backup has been previously created, the page will display a "Create Backup" button in the "Backup Node" section.
   * To initiate the backup process, the user simply needs to click this button.
   * Clicking the button triggers an AWS Lambda function that communicates with the RGB node's `/backup` API endpoint to start the backup process on the node.
   * Once the backup is successfully created, it is automatically uploaded to an Amazon S3 bucket. This bucket is structured to organize backups based on `userId` and `nodeId`, ensuring that backups are easy to manage and retrieve.
3. **Download**\
   A pre-signed URL is then generated and provided to the user. This URL allows the user to download the backup directly from the S3 bucket. For security purposes, this URL is time-limited and expires 30 minutes after being issued.

#### Node Restore

1. **Access Restore Functionality**: Users can manage backups for their nodes by navigating to the specific node’s backup page and find 'Restore node ' section at `nodes/{nodeId}/backup`.
2. **Uploading the Backup File :**  Click **"**&#x55;pload" - this button will generate an S3 upload URL. Once the S3 upload URL is generated, users are prompted to select the backup file from their local device. After selecting the file, the upload begins automatically, securely transferring the backup file to the designated S3 bucket.
3.  **Restoring Node:**

    After the upload is successfully completed, a "Restore" button becomes active on the interface. Users must click this button to start the restoration process. It triggers the node’s `/restore` API. The `/restore` API facilitates the restoration by taking the backup data from S3 and applying it to the node

    <br>
