# 📀 CUPS (Rate Limit)

#### Why Rate Limit? <a href="#why-rate-limit" id="why-rate-limit"></a>

ThunderStack RPC enabled rate limits to ensure customers utilize our service fairly, in order to provide a more stable and sustainable service. The rate limits will be implemented based on each second, that means in most cases, as long as you've handled retries when receiving rate limits error, the requests will go through in the following second, and it will not affect your experience.

We use CUPS to measure and implement the rate limit, and provide different CUPS for different tiers of ThunderStack RPC users.

#### What is CUPS? <a href="#what-is-cups" id="what-is-cups"></a>

Compute Unit Per Second (CUPS) is a measure of the number of [Compute Units (CUs)](https://thunderstack.gitbook.io/thunderstuck-rpc-docs/compute-units-cus) used per second when making requests. The weight of each RPC method is different, it will be more efficient than just calculating the number of requests sent in a second in different use cases.

See below the CUPS for different tiers of users

### User Plan Breakdown

* **Custom Enterprise**: custom
* **Starter**: 1500 CUPS
* **Developer**: 300 CUPS
* **Core Tier**: 150 CUPS

If your account reached the CUPS capacity, you will see the below error msg in the API response. You will need to enable retries to send the requests again. If you need higher rate limit, please upgrade to Developer or Starter tier.

Text

Copy

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "error": {
        "code": -32005,
        "message": "Your account has exceeded its Compute Units Per Second capacity."
    }
}
```
