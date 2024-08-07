---
description: >-
  This architecture is used to create RGB Lightning Nodes and their destruction,
  ensuring a consistent and scalable approach across the node lifecycle.
---

# Technical Architecture of RGB Lightning Node Deployment

#### AWS Services and Setup for RGB Lightning Node Deployment

**1. AWS API Gateway**

* **Purpose**: AWS API Gateway establishes a secure entry point for all client requests coming from the user interface.
* **Security**: The API Gateway is configured to authenticate and authorize requests using Amazon Cognito, ensuring that only legitimate requests from authenticated users are processed.

**2. AWS Lambda**

* **Functionality**: AWS Lambda functions are triggered by API Gateway to handle the logic required for initiating the node creation process. These functions manage the orchestration of the build process by interacting with other AWS services like AWS CodeBuild.
* **Process Integration**: Lambda functions parse incoming requests, extract necessary data, and invoke AWS CodeBuild with the appropriate parameters for node creation.

**3. AWS CodeBuild**

* **Purpose**: CodeBuild is responsible for building, destroying, and deploying the node onto an AWS EC2 instance.
* **Deployment**: CodeBuild configures and launches the EC2 instance that will host the new node, ensuring the environment meets all the specified requirements for the node’s operation.

**4. AWS EC2**

* **Role**: Once the build process is complete, AWS EC2 hosts the deployed RGB Lightning Node. It provides the compute resources necessary for running the node, managing its operation, and ensuring that it remains scalable and responsive under varying loads.

\
