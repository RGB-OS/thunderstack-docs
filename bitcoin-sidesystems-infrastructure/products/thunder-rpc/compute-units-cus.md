# 💿 Compute Units (CUs)

Compute Unit is a way to measure your RPC request usage on ThunderStack RPC. We provide each user a daily quota of usage, and each method will consume a cost based on the resource required for execution. You can think of this as you have a balance in your account and each method has its own price. This will be deducted from your account upon submission of RPC requests. The price of each RPC method is called Compute Unit. Based on this calculation, we enable our users to utilize the daily request quota more efficiently. In short, you only pay for what you use.

> **For different subscription tiers, there may have different daily usage quotas.**

#### CUs of RPC Method <a href="#cus-of-rpc-method" id="cus-of-rpc-method"></a>

Below are the CUs for each method. We define the CUs based on the resource required for execution.

**Supported Method**

| Method                                   | CUs  |
| ---------------------------------------- | ---- |
| eth\_accounts                            | 5    |
| eth\_blockNumber                         | 5    |
| eth\_chainId                             | 5    |
| eth\_syncing                             | 5    |
| net\_listening                           | 5    |
| net\_version                             | 5    |
| web3\_clientVersion                      | 5    |
| eth\_subscribe                           | 10   |
| eth\_uninstallFilter                     | 10   |
| eth\_unsubscribe                         | 10   |
| web3\_sha3                               | 10   |
| eth\_signTransaction                     | 10   |
| net\_peerCount                           | 10   |
| eth\_gasPrice                            | 15   |
| eth\_getBalance                          | 15   |
| eth\_getBlockByNumber                    | 15   |
| eth\_getCode                             | 15   |
| eth\_getStorageAt                        | 15   |
| eth\_getTransactionByBlockHashAndIndex   | 15   |
| eth\_getTransactionByBlockNumberAndIndex | 15   |
| eth\_getTransactionByHash                | 15   |
| eth\_getTransactionReceipt               | 15   |
| eth\_getBlockByHash                      | 18   |
| eth\_getBlockTransactionCountByHash      | 18   |
| eth\_getBlockTransactionCountByNumber    | 18   |
| eth\_getFilterChanges                    | 18   |
| eth\_newBlockFilter                      | 18   |
| eth\_newFilter                           | 18   |
| eth\_newPendingTransactionFilter         | 18   |
| eth\_call                                | 20   |
| eth\_getTransactionCount                 | 25   |
| eth\_getFilterLogs                       | 50   |
| eth\_getLogs                             | 50   |
| eth\_estimateGas                         | 75   |
| eth\_sendRawTransaction                  | 150  |
| debug\_traceTransaction                  | 280  |
| debug\_traceCall                         | 280  |
| debug\_traceBlockByNumber                | 1800 |
| debug\_traceBlockByHash                  | 1800 |





**Unsupported Method**

If ThunderStack RPC received requests using unsupported RPC method, the CUs are **2** for each request.

**How does ThunderStack RPC calculate my daily CU quota?**

We accumulate the daily usage by account. Regardless of how many API Keys you created in your account, we calculate the usage by summing the entire usage across all API Keys.

**What happens if I run out of** daily **CU quota?**

If you keep sending RPC request even though you've reached the daily CU quota, the request will fail and you will receive http 429 error and an error code: -**32005**

Copy

```
{
   "jsonrpc":"2.0",
   "id":1,
   "error":{
      "code":-32005,
      "message":"ran out of cu"
   }
}
```

You may consider a subscription plan upgrade [https://rpc.thunderstack.org/billing](https://rpc.thunderstack.org/billing)

**What happens if I send a request with incorrect payload?**

If you're sending a RPC request with an incorrect payload, it will cost:

* **If incorrect method**: it will cost you 2 usage for incorrect or unsupported RPC method.
* **If other parameters are incorrect, or execution failed due to the wrong payload**: it will consume the original cost of each method.

**How much** daily **quota do I have?**

We provide ThunderStack RPC users different tiers of daily quota according to their pricing plan. Please refer to our [pricing plan](https://thunderstack.gitbook.io/thunderstuck-rpc-docs/pricing).
