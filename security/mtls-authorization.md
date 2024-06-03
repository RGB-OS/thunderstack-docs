# mTLS Authorization

**Overview**

For enhanced security and controlled access to nodes hosted on Amazon EC2, Thunderstack.org utilizes a dual-layered authorization approach that combines the built-in mTLS capabilities of AWS API Gateway with a custom Lambda authorizer. This setup provides comprehensive certificate validation and fine-grained access control.

**Integration of API Gateway mTLS and Lambda Authorizer**

1. **API Gateway mTLS**:
   * **mTLS Configuration**: AWS API Gateway is configured to require mutual TLS (mTLS) for all incoming connections to specific API routes. This setup mandates that all clients must present a valid TLS certificate to establish a connection.
   * **Certificate Validation**: As part of the TLS handshake process, API Gateway verifies the client's certificate against a list of trusted Certificate Authorities (CAs). This ensures that only clients with certificates issued by trusted entities can initiate API calls.
2. **Custom Lambda Authorizer**:
   * **Role of Lambda Authorizer**: After the initial mTLS validation by API Gateway, a custom Lambda authorizer is invoked to perform additional security checks and access control decisions.
   * **Detailed Validation and Authorization**:
     * The Lambda function further inspects the validated certificate to extract and verify specific details such as the user ID and node ID.
     * Based on these details and predefined security policies, the Lambda authorizer decides whether to grant or deny access to the requested node resources.
     * It dynamically generates an IAM policy that either allows or denies the API request, applying this decision to the ongoing session.

**Security Workflow**

1. **Client Certificate Submission**: When a client initiates an API call, it must present a valid certificate as part of the secure TLS connection setup.
2. **Initial mTLS Verification by API Gateway**: The certificate is first checked by API Gateway to confirm its validity and trustworthiness against known CAs.
3. **Further Validation by Lambda Authorizer**:
   * Upon successful initial validation, the Lambda authorizer examines the certificate for specific attributes critical for accessing the node.
   * The authorizer assesses the integrity and relevance of these attributes and ensures they align with the security requirements of the node being accessed.
4. **Access Decision and Enforcement**:
   * If all checks pass, the Lambda authorizer issues an IAM policy that permits the request, allowing the client to interact with the node.
   * If the certificate or its attributes fail to meet the necessary criteria, the request is denied, thereby blocking any potential unauthorized access.
