# Set up webhooks

Whenever there's a new event about one of the payments you've generated, we'll send an HTTP POST request to a URL you provide, you can use this to take actions such as delivering a service when a payment has been made or cut access to a user when a subscription expires.

To receive these webhook notifications, you need to set up a webserver with https and a POST endpoint, then submit the URL of that endpoint in the Developer tab of LlamaPay dashboard.

The event received from the webhook will look like this:

```javascript
{
  event: {
    api_version: '2018-03-22', // Constant, identifies the format
    created_at: '2024-06-23T06:47:30.020Z', // Time when event was created
    data: {
      id: 'fd533947-6889-4970-8fb9-6441342dc07d', // Payment id
      pricing: { // Pricing provided when payment was created
        "local":{
          "amount":"10.0",
          "currency":"USD"
        }
      },
      metadata: { // Metadata provided when payment was created
        "userId": "0xngmi"
      },
      created_at: '2024-06-22T14:48:34.076Z', // Time when payment was created
      hosted_url: 'https://checkout.llamapay.io/pay/fd533947-6889-4970-8fb9-6441342dc07d', // URL where users can pay
      pricing_type: 'fixed_price', // Type of payment, provided at payment creation
    },
    id: 'e1e4ff60-9fcb-4a9f-b0df-fb5b0139cf2d"', // id of event, will be the same on all retries so you can use it to avoid processing same event twice
    type: 'charge:pending' // Type of event
  },
}
```

The possible event types are:

<table><thead><tr><th width="203">Type</th><th>Explanation</th></tr></thead><tbody><tr><td>charge:pending</td><td>Payment transaction has been included on chain</td></tr><tr><td>charge:confirmed</td><td>Payment transaction has been finalized on chain, this happens some time after the transaction has been included on chain and we sent the charge:pending event</td></tr><tr><td>subscription:expired</td><td>A subscription has expired because it reached it's end and there was not enough money to keep it going</td></tr></tbody></table>

### Pending vs Confirmed

As soon as a transaction has landed on-chain we send a `charge:pending` event. However, if there is a chain reorg it's possible for that transaction to be excluded from the chain, so after enough time has passed that a reorg is impossible or very unlikely we send a `charge:confirmed` event confirming that the payment has been finalized.

This is the same that you would typically see when depositing into a CEX, as soon as the transaction is onchain you can see it, but it takes some confirmations till the money is available.

We recommend applying the effects of the payment as soon as you receive a charge:pending event. This will improve UX for your users and once a charge:pending event has been sent the user can't revert the payment anymore since it's on chain. The only way for such a payment to revert would be a chain reorg along with the user sending a replacement transaction.

### Security

Attackers could figure out your endpoint and send fake requests, so you should authenticate all webhook requests by crafting a HMAC from the body of the request and the shared secret you can obtain from the Developer tab on the dashboard and comparing that against the value we send in the `X-CC-WEBHOOK-SIGNATURE` header.

Here's how to do it in javascript:

```javascript
const { createHmac } = require("crypto");

function verifyWebhook(request){
  const hmac = createHmac('sha256', "LLAMAPAY_WEBHOOK_SECRET")
    .update(request.body).digest('hex');
  if(request.headers['X-CC-WEBHOOK-SIGNATURE'] !== hmac){
    throw new Error("Invalid webhook signature")
  }
}
```

### Retries

In case the request can't reach your server or your server returns an HTTP code other than 200, we'll keep retrying the same request for 3 days with an exponential backoff delay that maxes out at 1 hour.

In other words, if your server is off or for some reason it returns an error, we'll retry the same request after 10 seconds, then if that fails again, we'll retry after 20 seconds, 40 seconds, 80 seconds... for 3 days or until we get an HTTP 200 response to this request.
