---
description: RGB Lightning Node API
---

# ⚙️ RGB Lightning Node API

Once node are running, they can be operated via REST JSON APIs.

For example, using curl:

```
curl -X POST -H "Content-type: application/json" \
    -d '{"ticker": "USDT", "name": "Tether", "amounts": [666], "precision": 0}' \
    http://localhost:3001/issueasset
```

The node currently exposes the following APIs:

* `/address` (POST)
* `/assetbalance` (POST)
* `/assetmetadata` (POST)
* `/backup` (POST)
* `/btcbalance` (POST)
* `/changepassword` (POST)
* `/checkindexerurl` (POST)
* `/checkproxyendpoint` (POST)
* `/closechannel` (POST)
* `/connectpeer` (POST)
* `/createutxos` (POST)
* `/decodelninvoice` (POST)
* `/decodergbinvoice` (POST)
* `/disconnectpeer` (POST)
* `/estimatefee` (POST)
* `/failtransfers` (POST)
* `/getassetmedia` (POST)
* `/getchannelid` (POST)
* `/init` (POST)
* `/invoicestatus` (POST)
* `/issueassetcfa` (POST)
* `/issueassetnia` (POST)
* `/issueassetuda` (POST)
* `/keysend` (POST)
* `/listassets` (POST)
* `/listchannels` (GET)
* `/listpayments` (GET)
* `/listpeers` (GET)
* `/listswaps` (GET)
* `/listtransactions` (POST)
* `/listtransfers` (POST)
* `/listunspents` (POST)
* `/lninvoice` (POST)
* `/lock` (POST)
* `/makerexecute` (POST)
* `/makerinit` (POST)
* `/networkinfo` (GET)
* `/nodeinfo` (GET)
* `/openchannel` (POST)
* `/postassetmedia` (POST)
* `/refreshtransfers` (POST)
* `/restore` (POST)
* `/rgbinvoice` (POST)
* `/sendasset` (POST)
* `/sendbtc` (POST)
* `/sendonionmessage` (POST)
* `/sendpayment` (POST)
* `/shutdown` (POST)
* `/signmessage` (POST)
* `/sync` (POST)
* `/taker` (POST)
* `/unlock` (POST)

To get more details about the available APIs see the [OpenAPI specification](https://github.com/RGB-OS/rgb-lightning-node/blob/master/openapi.yaml). A Swagger UI for the `master` branch is generated from the specification and available at [https://rgb-tools.github.io/rgb-lightning-node](https://rgb-tools.github.io/rgb-lightning-node)
