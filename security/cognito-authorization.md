# Cognito Authorization

**Overview**

This document serves as a guide to the authorization module used by Thunderstack.org, detailing the user authentication flow and system components involved. Thunderstack.org leverages Amazon Cognito for robust and secure user management and authentication.

**System Components**

1. **Amazon Cognito**: Provides user identity and data synchronization services, allowing for secure user authentication and management.
2. **Hosted UI**: A customizable user interface provided by Amazon Cognito for user sign-up and sign-in operations.
3. **AWS Lambda**: Serverless compute service that runs backend logic in response to events such as user authentication requests.
4. **API Gateway**: Manages secure access to APIs, interfacing between users and backend services like Lambda.

**Authentication Flow**

1. **User Interaction**:
   * Users start at the login page on [https://www.thunderstack.org/](https://www.thunderstack.org/).
   * Upon clicking the 'Login' or 'Sign Up' button, the user is redirected to the Amazon Cognito Hosted UI.
2. **Amazon Cognito Hosted UI**:
   * **Username and Password Login**:
     * Users can choose to log in using their username and password.
   * **Google Login**:
     * Alternatively, users can log in through their Google account.
   * Upon successful authentication, the Hosted UI redirects the user to a specified callback page.
3. **Authorization Code and Token Exchange**:
   * The redirection URL includes an authorization code.
   * A backend AWS Lambda function is triggered, which calls the Amazon Cognito `/oauth2/token` endpoint.
   * The authorization code is validated and exchanged for a token.
4.  **Access to Serverless APIs**:

    * The obtained token is then returned to the user.
    * This token is used to authenticate requests made to various serverless APIs hosted on AWS API Gateway.
    * AWS API Gateway, protected by Cognito Authorizer, ensures that the APIs can only be accessed with a valid token, providing secure access to the application's resources.

