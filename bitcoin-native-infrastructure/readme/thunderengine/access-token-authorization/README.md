# 🏗️ Access Token Authorization

#### **Access Token Usage for API Authentication**

Access tokens generated on the platform are critical for securely authenticating API requests. These tokens grant access to specific resources or actions, as defined by the permissions assigned to them.

For security reasons, the platform **does not store the token itself**. Instead, it maintains metadata associated with the token, such as its name, creation date, and expiration date, ensuring secure token management.

***

**Access Token Utilization**

Access tokens can be used to authorize interactions with the platform’s APIs in a secure and streamlined manner. The following header must be included in API requests to authenticate them:

```css
Authorization: Bearer {accessToken}
```

This ensures that:

* Only authorized users or applications can access the specified API resources.
* All API requests are securely authenticated and aligned with the token's permissions.
