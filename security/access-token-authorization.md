# Access Token Authorization

Users can use the generated access token to authenticate API requests. This token grants access to specified resources or actions, depending on the permissions it carries. \
The platform **does NOT store** the token itself for security reasons, only the metadata associated with the token such as its name, creation date, etc.

**Access Token Utilization for API Authorization:** Users can employ the access tokens generated on the platform to authorize interactions with two distinct types of APIs available.\
&#x20;\
`Authorization: Bearer {accessToken}` - This authorization header ensures that the API requests are securely authenticated.\
\
For more information, see:

1. [**Creating an API Token**](../general/getting-started-with-thunderstack-rgb-cloud/create-api-token.md)
2. [**Thunderstrack  API**](../developer-api/thunderstack-api.md)
3. [**RGB Lightning Node API**](../developer-api/rgb-lightning-node-api.md)
