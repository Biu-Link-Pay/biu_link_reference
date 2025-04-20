---
icon: webhook
---

# Merchant Webhook

#### **Payment/Refund Status Webhook Documentation**

**1. Purpose**

Real-time notifications for payment or refund status changes (e.g., payment success,  refund completion).

**2.Webhook Retry Logic**

| Parameter          | Recommended Setting         | Rationale                          |
| ------------------ | --------------------------- | ---------------------------------- |
| **Initial Delay**  | Immediate (0 min)           | First attempt should be instant    |
| **Retry Interval** | 5 minutes                   | Balance between urgency & overload |
| **Max Attempts**   | 6 (including initial)       | \~30min total coverage             |
| **Timeout**        | 10 seconds per attempt      | Prevent connection hangs           |
| **Jitter**         | ±10% interval randomization | Avoid thundering herd problems     |

Terminal Conditions (Stop Retrying):

* HTTP `200` received&#x20;

**3.Request Body Example**

```
{
    "num": null,
    "status": null,
    "tradeNo": null,
    "refundTradeNo": null,
    "userMark": null,
    "orderAmount": null,
    "orderType": null,
    "refundType": null,
    "refundReason": null,
    "orderToken": null,
    "orderNetwork": null,
    "hashId": null,
    "networkFee": null
}
```

**4. Field Descriptions**

| Field Name    | Type   | Description                                                                                                                                           |
| ------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| num           | String | System OrderNum                                                                                                                                       |
| status        | String | <p>INIT<br>PROCESSING<br>PAYMENT_SUCCESS<br>PAYMENT_FAILED<br>ORDER_TIMEOUT<br>ORDER_ERROR<br>DELAY_PAYMENT_SUCCESS<br>DELAY_PAYMENT_FAILE</p> |
| tradeNo       | String | Merchant OrderNum                                                                                                                                     |
| refundTradeNo | String | Merchant OrderNum                                                                                                                                     |
| userMark      | String | User identifier (e.g., member ID/phone)                                                                                                               |
| orderAmount   | Number | OrderAmount                                                                                                                                           |
| orderType     | String | <p>IN<br>OUT</p>                                                                                                                                     |
| refundType    | String | <p>SYS_SETTLEMENT<br>MERCHANT_WITHDRAW</p>                                                                                                           |
| refundReason  | String | RefundReason                                                                                                                                          |
| orderToken    | String | Associated token (e.g., cryptocurrency contract address)                                                                                              |
| orderNetwork  | String | Blockchain network: `ETH`/`BSC`/`POLYGON` etc.                                                                                                        |
| hashId        | String | On-chain transaction hash                                                                                                                             |
| networkFee    | Number | Network fee                                                                                                                                           |
