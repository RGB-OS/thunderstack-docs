---
description: Authorization Methods in ThunderCloud
---

# 🔒 Security

At **Thunderstack.org**, we prioritize the security of your interactions with nodes hosted on AWS through ThunderCloud. To ensure robust protection, we provide two advanced methods of authorization for secure communication: **Cognito Authorization** and **mTLS Authorization**. These methods meet stringent security standards, safeguarding both user interactions via our interface&#x20;

**1. Cognito Authorization**

**Cognito Authorization** is designed for users interacting with nodes through the **ThunderCloud UI**. It ensures secure access and streamlined authentication for HTTPS requests.

* **Key Features**:
  * Leverages **Amazon Cognito** to manage user identities.
  * Handles **authentication** and **token issuance** securely.
  * Allows users to perform authorized actions seamlessly via the ThunderCloud user interface.
* **Use Case**: Ideal for users accessing nodes through the ThunderCloud UI for day-to-day operations and HTTPS API requests.
* **Benefits**:
  * Centralized identity management.
  * Automated token handling for improved user experience.
  * Secure interaction with ThunderCloud nodes.

***

**2. mTLS Authorization**

**mTLS Authorization** (Mutual TLS) is tailored for developers and advanced users who need to interact with nodes directly, such as through custom applications or third-party clients.

* **How it Works**:
  * Both the client and server authenticate each other using **TLS certificates**.
  * Clients must present a **valid certificate** issued by a trusted Certificate Authority (CA).
  * Enforced via **AWS API Gateway** to ensure secure communications.
* **Key Features**:
  * **Mutual Authentication**: Verifies the identity of both parties in the communication.
  * **Certificate Management**: Requires proper configuration of client certificates for secure API access.
  * Supports direct, high-security API interactions.
* **Use Case**: Ideal for developers and applications requiring secure, direct communication with nodes outside of the ThunderCloud UI.
* **Benefits**:
  * Maximum security for API interactions.
  * Flexibility for integrating ThunderCloud with custom-built solutions.
  * Trust-based access via Certificate Authority validation.

***

#### **Why We Choose this Authorization Methods for ThunderCloud?**

* **End-to-End Encryption**: Ensures data integrity and confidentiality during node communication.
* **Flexibility**: Choose between UI-driven Cognito Authorization or API-focused mTLS Authorization based on your needs.
* **Reliability**: Backed by AWS infrastructure and industry-leading security standards.



