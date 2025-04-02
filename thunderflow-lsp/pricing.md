# 💳 Pricing

#### Prepaid Plan

* **Direct Payments:** Pay directly to the LSP for channel provisioning.
* **Flexible Pricing:** Prices vary based on the RGB asset, channel amount, and duration.

#### Volume-based subscription plan

### **LSP** Fee Structure

**The total fee** is calculated:

* **Base Fee** - fixed 1500 sats
* **Locked Capacity Fee** -  capacity\_sat (for now capacity\_sat is fixed 30010 sats ) x 0.05%&#x20;
* **Push Fee** - push\_msat + 0.05% (if push\_msat exist otherwise No Push Fee)
* **Asset Fee**  - based on the asset's **value in BTC.** asset amount converted to BTC x 0.05%&#x20;
* **On-Chain Fee** (Bitcoin Mempool Fee) - Transaction size in vbytes (fixed at **500 vbytes**) x current mempool fee rate

\
**Total Fee = Base Fee + Locked Capacity Fee + Push Fee + Asset Fee + On-Chain Fee**
