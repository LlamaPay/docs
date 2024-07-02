# Create a new payment

<mark style="color:green;">`POST`</mark> `https://api.llamapay.io/charges`

#### **Headers**

| Name          | Value                      |
| ------------- | -------------------------- |
| Authorization | `llamapay_sk_YOUR_API_KEY` |

#### **Body parameters**

<table><thead><tr><th width="154">Key</th><th>Value</th><th>Example</th></tr></thead><tbody><tr><td>pricing_type</td><td>Can be either:<br>- <code>fixed_price</code> for simple one-time payments<br>- <code>no_price</code> for payments where user picks amount, eg donations<br>- <code>subscription</code> for recurring subscriptions</td><td>"fixed_price"</td></tr><tr><td>local_price</td><td>Amount to charge user.<br>If <code>pricing_type</code> is <code>subscription</code>, this is the monthly cost</td><td>{<br>  "amount": "10.00",<br>  "currency": "USD"<br>}</td></tr><tr><td>metadata</td><td>Arbitrary JSON object, it will be returned on webhooks for the payment.<br>Object must be smaller than 20kB</td><td>{ "userId": "0xngmi" }</td></tr><tr><td>redirect_url</td><td>URL to redirect user to after successful payment</td><td>"https://mypage.com/success"</td></tr><tr><td>cancel_url</td><td>URL to redirect user to if they cancel the payment</td><td>"https://mypage.com/failure"</td></tr></tbody></table>

#### **Example body**

Charge the user a one-time payment of 10$:

```json
{
	"pricing_type": "fixed_price",
	"local_price": {
		"amount": "10.00",
		"currency": "USD"
	}
}
```

#### **Response**

```json
{
  "data": {
    ...
    "hosted_url": "https://checkout.llamapay.io/pay/fd533947-6889-4970-8fb9-6441342dc07d",
    ...
  }
}
```
