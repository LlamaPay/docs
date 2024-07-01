# Create a new payment

### Create a new payment

<mark style="color:green;">`POST`</mark> `https://api.llamapay.io/charges`

**Headers**

| Name          | Value                      |
| ------------- | -------------------------- |
| Authorization | `llamapay_sk_YOUR_API_KEY` |

**Body**

```json
{
	"pricing_type": "fixed_price",
	"local_price": {
		"amount": "1.00",
		"currency": "USD"
	}
}
```

**Response**

```json
{
  "data": {
    ...
    "hosted_url": "https://checkout.llamapay.io/pay/fd533947-6889-4970-8fb9-6441342dc07d",
    ...
  }
}
```

