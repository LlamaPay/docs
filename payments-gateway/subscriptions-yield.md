# Subscriptions Yield

Money that's sitting in our contracts waiting to pay a subscription is deposited into AAVE, where it earns interest, this interest then effectively reduces the cost of the subscription, potentially making it free if deposit is big enough.

Let's say a user wants to pay a 5$/mo subscription and deposits 600$ into llamapay, this money will earn 5% currently on AAVE, so a total of 30$ yearly. After a year, user will have paid a total of 5\*12 = 60$ for the subscription, but because he has earned 30$, the total cost is only 30$, thus effectively his subscription is only 2.5$/mo.

If that same user had deposited 1.2k$ instead, then yearly yield would be 60$ and thus after a year he would have lost 60$ and earned 60$, thus staying flat. In this case the subscription will be completely free, because at any moment he can withdraw and get back all the money he put in,

All this while the receiver always receives the full subscription amount, so receiver can be getting paid 5$/mo while payer only loses 2.5$/mo or even nothing.

This is how LlamaPay enables free subscriptions, and even if users doesn't deposit enough money to fully offset costs they still benefit from this feature because their subscription costs will still be partially reduced.
