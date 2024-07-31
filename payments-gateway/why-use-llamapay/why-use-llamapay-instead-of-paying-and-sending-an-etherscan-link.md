# Why use LlamaPay instead of paying and sending an etherscan link?

Subscriptions require our contracts, but for simple one-off stablecoin payments you might wonder why use LlamaPay instead of posting an address and having your clients send money there and send you the etherscan link aftewards, since that's free and easy to implement.

LlamaPay improves upon these simple payments significantly in attribution, automation and convenience, with no downsides since costs stay exactly the same!

### Attribution

Imagine you run a telegram bot that requires payment for access, and a user pays and sends you the etherscan link of the transaction.

How do you know if the user who sent you that link is the one who actually sent the transaction? Someone else could have seen that transaction and sent it to you. And how do you link that payment with a telegram username to give access to?

If the payment is done through LlamaPay you'll be able to tell exactly who sent that payment and get their telegram handle directly (or any other internal user representation you use), because you'll know which payment link that payment came from, removing any possible attribution conflict. User info is either provided directly through our API before payment or by users before the payment if you're using static payment links, making it impossible to tamper with and falsely claim a payment.

### Automation

Having to manually process payments:

* takes away your time and focus from what really matters, running your business
* leads to a very slow experience for your users, who have to wait for hours/days for the payment to complete
* easily leads to mistakes such as forgetting to cancel a service when payment is over

LlamaPay completely automates handling of payments so you don't have to bother with it and provisioning for your users is instant.

### Convenience

Our payment links streamline the process of paying, guiding the user to the chains where you wish to accept payment and automatically showing them their options based on their wallet, preventing duplicated payments and user errors from pasting addresses around. All while providing a more professional experience for them.

For you, we provide analytics on all your payments and make them easy to consume through our API.

### Downsides

Stablecoin transfers have 0% fees on LlamaPay, and they're implemented using transfer() calls, which is the same that users would use when sending payments directly to your wallet, so the gas cost for users is exactly the same.

### Conclusion

By using LlamaPay you get to automate work away, completely remove attribution conflicts, reduce user errors and massively improve customer UX by reducing payment settling from hours/days to minutes, all this at the exact same cost as crudely sending payments directly, both for you and your users.
