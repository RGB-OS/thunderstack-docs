# Connect Node

#### Connecting to  Node with mTLS

Follow these detailed steps to establish a secure connection to your node at Thunderstack.org, using the provided mTLS credentials:

**Step 1: Access Node Connection Details**

1.  **Navigate to the Connect Page**: Open your web browser and go to the `/connect` page on Thunderstack.org. This page is specifically designed to provide you with the connection details for your node.

    * URL: `https://www.thunderstack.org/{nodeId}/connect`

    <figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
2. **View Connection Details**: On the connect page, you will find the unique endpoint URL for your node, along with the private key and certificate required for the mTLS connection. These credentials are automatically generated for your session and are essential for establishing a secure connection.

**Step 2: Obtain Connection Credentials and URL**

1. **Download Credentials**: While on the `/connect` page, you will have the option to download two crucial files:
   * **Private Key**: A `.key` file (e.g., `privateKey.key`) containing your private key.
   * **Certificate**: A `.pem` file (e.g., `certificate.pem`) containing your public key certificate.
2. **Copy Endpoint URL**: The page will display the unique endpoint URL formatted as follows: `https://{userId}.thunderstack.org/nodes/{userId}/{nodeId}/`.&#x20;

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

**Step 3: Prepare the Request**

1. **Save the Downloaded Files**: Ensure that the private key and certificate files are saved in a secure location on your computer. You will need to reference these files when making the request.
2. **Example Request**: An example curl command will be shown on the `/connect` page. This serves as a template for how you should structure your request. Modify the example by replacing the placeholders with the actual paths to your saved key and certificate files.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

#### Connecting to  Node with API token

Follow these detailed steps to establish a secure connection to your node at Thunderstack.org, using the provided API token:

Example usage with Curl:

`export THUNDERSTACK_API_TOKEN=<user barrier token>`\
`curl -H 'Authorization: Bearer ${THUNDERSTACK_API_TOKEN}' https://node-api.thunderstack.org/<user_id>/<node_id>`\
\
Follow [https://thunderstack.gitbook.io/rgb-rln-cloud/security/access-token-authorization](https://thunderstack.gitbook.io/rgb-rln-cloud/security/access-token-authorization) to obtain `THUNDERSTACK_API_TOKEN`



