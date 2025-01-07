# 🔒 Security

Thunderstack.org provides two robust methods of authorization to secure communication with nodes hosted on AWS EC2. These methods ensure that both interactions through our user interface and direct API communications meet strict security standards.

1. [ **Cognito Authorization**](cognito-authorization.md)
   * Cognito Authorization is used for users interacting with nodes via the Thunderstack.org user interface and can be used for HTTP requests. This approach leverages Amazon Cognito for managing user identities, handling authentication, and issuing tokens that authorize users to perform actions.
2. [**mTLS Authorization**](mtls-certificate-generation.md)
   * mTLS Authorization caters to users who wish to communicate with nodes directly from their own interfaces, such as third-party applications or custom-developed clients. This method requires mutual TLS (mTLS), wherein both the client and the server authenticate each other using TLS certificates. Enforced through the AWS API Gateway, it mandates that clients present a valid certificate issued by a trusted Certificate Authority (CA) as part of the secure communication process. This is suitable for developers and applications that require direct and secure API interactions with nodes and involves managing certificates to ensure proper configuration for secure communications.
3.  [**Access Token Authorization**](access-token-authorization.md)







