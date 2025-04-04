# 📖 Getting Started

#### Navigate to [ThunderStack RPC App](https://rpc.thunderstack.org/) <a href="#navigate-to-thunderstack-rpc-app" id="navigate-to-thunderstack-rpc-app"></a>

#### 1. Sign Up / Sign In <a href="#id-1.-sign-up-sign-in" id="id-1.-sign-up-sign-in"></a>

Begin by creating an account or logging in. ThunderStack RPC supports two authentication methods:

* **Username and Password:** Use your standard login credentials.
* **Google Sign-In:** Quickly access your account using your Google account.

![](https://thunderstack.gitbook.io/~gitbook/image?url=https%3A%2F%2F1923892169-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FIRjGMOvUzARhCLXZ8Xyh%252Fuploads%252Fbjyp2kqRYwD4Dk5MxVgk%252Fimage.png%3Falt%3Dmedia%26token%3D97cf6c89-f806-46ce-9719-9ebdb4862a21\&width=768\&dpr=4\&quality=100\&sign=e77c0cac\&sv=2)

#### 2. Create an API Key <a href="#id-2.-create-an-api-key" id="id-2.-create-an-api-key"></a>

After signing in, you'll need to create an API key to interact with the RPC service:

* **Navigate to `/api-keys`:** Go to the API Keys management page.
* **Click "Create API Key":** Start the process of generating a new key.

![](https://thunderstack.gitbook.io/~gitbook/image?url=https%3A%2F%2F1923892169-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FIRjGMOvUzARhCLXZ8Xyh%252Fuploads%252F62FKrfkTmUPytb2B1hOP%252Fimage.png%3Falt%3Dmedia%26token%3Dcb1dda4c-1806-4198-8e45-5f96a6e12cdc\&width=768\&dpr=4\&quality=100\&sign=1de50de2\&sv=2)![](https://thunderstack.gitbook.io/~gitbook/image?url=https%3A%2F%2F1923892169-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FIRjGMOvUzARhCLXZ8Xyh%252Fuploads%252FnWXloRJJX6y5BGO641Ds%252Fimage.png%3Falt%3Dmedia%26token%3Dc8b622fd-7a23-4456-ac3d-53f7edf0b6a7\&width=768\&dpr=4\&quality=100\&sign=8a803c69\&sv=2)

* **Access Your Key:** Once created, you will be redirected to a page displaying your API key along with the supported networks.

**Please note** that it may take a few minutes for the RPC service to become fully operational.

#### 3. Select Your Supported Network <a href="#id-3.-select-your-supported-network" id="id-3.-select-your-supported-network"></a>

On the API key page, you'll find a list of all supported blockchain networks. Click on the network (chain) you wish to work with to obtain the corresponding endpoint URL.

![](https://thunderstack.gitbook.io/~gitbook/image?url=https%3A%2F%2F1923892169-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FIRjGMOvUzARhCLXZ8Xyh%252Fuploads%252FZYgQJmTJPtQ6IF3K6SMs%252Fimage.png%3Falt%3Dmedia%26token%3Ddf6e27b1-a557-4877-afe6-291e21c32260\&width=768\&dpr=4\&quality=100\&sign=dabb4e7b\&sv=2)![](https://thunderstack.gitbook.io/~gitbook/image?url=https%3A%2F%2F1923892169-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FIRjGMOvUzARhCLXZ8Xyh%252Fuploads%252FZJvM38C8jx73kfAzKAq0%252Fimage.png%3Falt%3Dmedia%26token%3D3c5e4550-17b5-4bba-b59e-48227d422878\&width=768\&dpr=4\&quality=100\&sign=6d882c5e\&sv=2)

#### 4. Example Request <a href="#id-4.-example-request" id="id-4.-example-request"></a>

With your API key and endpoint URL in hand, you can now make a request. Here’s an example using cURL:

Using the query string method:

Copy

```
curl "https://rpc-<userId>.thunderstack.org/main/evm/<chainId>?token=<apiKey>" \
--data '{
    "method": "eth_getBlockByNumber",
    "params": ["latest", false],
    "id": 9199,
    "jsonrpc": "2.0"
}'
```

Using the HTTP header method:

Copy

```
curl -X POST "https://rpc-<userId>.thunderstack.org/main/evm/<chainId>" \
  -H "X-ERPC-Secret-Token:<apiKey>"
--data '{
    "method": "eth_getBlockByNumber",
    "params": ["latest", false],
    "id": 9199,
    "jsonrpc": "2.0"
}'
```

Replace `<userId>`, `<chainId>`, and `<apiKey>` with your actual user ID, chain identifier, and API key.

This setup will allow you to start making secure JSON-RPC calls to your chosen Bitcoin EVM-compatible layer seamlessly. Enjoy building with ThunderStack RPC!

[\
](https://thunderstack.gitbook.io/thunderstuck-rpc-docs/how-it-works)
