# mTLS Certificate Generation

**AWS Services and Setup**

* **AWS ACM PCA (AWS Certificate Manager Private Certificate Authority)**: This service is tasked with the issuance of certificates. The Lambda function interacts directly with ACM PCA to request and retrieve certificates tailored for each user node interaction.
* **AWS Lambda**: Executes the necessary logic to generate a new certificate for each API request, ensuring secure communication to user nodes.
* **API Gateway with Amazon Cognito Protection**: The `/generate-cert` API route is securely protected using Amazon Cognito. This setup ensures that all requests to generate certificates are authenticated and authorized through Cognito before reaching the Lambda function. This layer of security safeguards the certificate issuance process, ensuring that only authenticated users can initiate requests.

#### Description of Certificate Generation

To ensure secure access to user nodes via API calls, Thunderstack.org utilizes a system where individual mTLS certificates are dynamically generated for each node. This certificate generation is managed by a dedicated AWS Lambda function. Here is an in-depth overview of this process:

1. **AWS Lambda Function**: A specialized Lambda function is responsible for the entire lifecycle of certificate generation, tailored to individual node requirements.
2. **Dynamic Generation**: Each request for node access triggers the creation of a unique mTLS certificate, ensuring that communications are secured specifically for each node interaction.
3. **Process Breakdown**:
   * **Event Parsing**: The Lambda function starts by analyzing the incoming API request to extract essential parameters such as `userId` and `nodeId`.
   * **Key Generation**: Using cryptographic libraries, the function generates a robust RSA key pair.
   * **Certificate Signing Request (CSR) Creation**: A CSR is constructed with the generated keys, and user-specific details are embedded into the CSR fields.
   * **CSR Submission**: The CSR is securely sent to AWS ACM PCA, which acts as the certificate authority.
   * **Certificate Issuance**: Upon validation, AWS ACM PCA issues the certificate, which the Lambda function then retrieves.
   * **Response Preparation**: The newly created certificate and its associated keys are packaged and sent back to the requesting user or system.

This structured approach ensures that each node is equipped with a certificate specifically generated for its secure operation, enhancing the overall security posture of the system.

