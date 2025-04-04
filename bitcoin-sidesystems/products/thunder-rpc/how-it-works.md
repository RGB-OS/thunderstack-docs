# ⚙️ How It Works?

## ⚙️How It Works?

For every API key a user creates, ThunderStack RPC launches an eRPC proxy that manages incoming requests and redirects them to the appropriate blockchain node. This proxy acts as a lightweight intermediary, ensuring that your requests are routed efficiently to the correct Bitcoin EVM-compatible layers endpoint.

Each endpoint is secured using the **Secret auth strategy**. This strategy is activated based on the request payload; for instance, if the query string includes a `token` parameter, the secret strategy is engaged.

#### Secret Auth Strategy <a href="#secret-auth-strategy" id="secret-auth-strategy"></a>

The secret strategy is a simple yet effective method for authenticating requests. It verifies that the secret value defined during API key creation matches the token provided by the client. The token can be supplied in one of two ways:

*   **Query String:**

    Copy

    ```
    curl -X POST "https://rpc-<userId>.thunderstack.org/main/evm/<chainId>?token=<apiKey>"
    ```
*   **HTTP Header:**

    Copy

    ```
    curl -X POST "https://rpc-<userId>.thunderstack.org/main/evm/<chainId>" \
      -H "X-ERPC-Secret-Token:<apiKey>"
    ```

In both cases, the eRPC proxy validates the provided token against the secret associated with the API key. If the validation succeeds, the request is forwarded to the relevant blockchain node; if not, the request is rejected.

This design ensures that every API call is securely authenticated, providing a robust access layer to the Bitcoin EVM-compatible layers while maintaining ease of integration for developers.
