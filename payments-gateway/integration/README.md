# Integration

## No code

1. Create a company by clicking **Get Started** on [https://checkout.llamapay.io/](https://checkout.llamapay.io/)
2. Create a payment link
3. Add the payment link to your site or send it to customers directly

## Code

{% hint style="info" %}
Have you integrated Coinbase Commerce already? Our API is fully backwards compatible with it, so you can switch to LlamaPay by simply replacing endpoint urls, api key, and webhook secret. It's a drop-in replacement!
{% endhint %}

1. Create a company by clicking **Get Started** on [https://checkout.llamapay.io/](https://checkout.llamapay.io/)
2. Set up a server with a webhook endpoint to get notified when the payment is paid.
3. Go to the Developer tab in LlamaPay UI, set your webhook URL and copy your API Key and webhook secret.
4. To create a new payment, make a <mark style="color:green;">`POST`</mark> request to`https://api.llamapay.io/charges`, then take `hosted_url` from the response and send it to your users to pay.

Here's a basic express server that implements the system above:

```javascript
const { createHmac } = require("crypto");
const express = require('express')
const app = express()
app.use(express.raw({ type: '*/*', limit: '10mb' }));

const PORT = 3000
const LLAMAPAY_API_KEY = "llamapay_sk_YOUR_API_KEY"
const LLAMAPAY_WEBHOOK_SECRET = "llamapay_webhook_secret_XXXX"

app.post('/webhook', (req, res) => {
  const hmac = createHmac('sha256', LLAMAPAY_WEBHOOK_SECRET)
    .update(req.body).digest('hex');
  if(req.headers['x-cc-webhook-signature'] !== hmac){
    return res.sendStatus(401)
  }
  const body = JSON.parse(req.body);
  if(body.type === "charge:pending"){
    console.log("User paid:", body.data.metadata)
  }
  res.sendStatus(200)
})

app.post('/new-payment/:userHandle', async (req, res) => {
  const payment = await fetch("https://api.llamapay.io/charges", {
    method: "POST",
    headers:{
        Authorization: LLAMAPAY_API_KEY
    },
    body: JSON.stringify({
        "pricing_type": "fixed_price",
        "local_price": {
            "amount": "1.00",
            "currency": "USD"
        },
        "metadata": {
            "userHandle": req.params.userHandle
        }
    })
  }).then(r=>r.json())
  res.send(payment.data.hosted_url)
})

app.listen(PORT, () => {
  console.log(`Listening on port ${PORT}`)
})
```
