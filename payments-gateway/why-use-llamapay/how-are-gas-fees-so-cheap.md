# How are gas fees so cheap?

The main issue a payments gateway has to solve is how to connect a transfer with a payment request, so if an app requests a 100$ payment from multiple users and later there's a 100$ transfer, how do you know which user did the payment?

### Previous solutions

The most common solution that payment gateways use is they create a new address for each payment, so if money ends up at that address then you know for sure which user sent it. However this means that the payment gateway needs to later send that money again to aggregate all payments, which costs extra transaction fees, and it also needs to take custody of funds, which exposes it to hacks.

Another solution is to do the transfer through a contract where a unique id is attached to the transaction, allowing identification of which payment this transaction is paying. The issue with this is that adding a contract to the transaction increases gas costs and number of transactions needed, as first you need to approve the token against the contract.

### Our solution

Before making a payment, users send a request to our server claiming the payment, such as "I will pay payment abcde from address 0x123", then when a transaction comes from address 0x123 we know that it must be paying abcde.

But what happens if two different users claim the same address for different payments? In that case we add 0.000001$ to the second payment such that it's possible to perfectly differentiate between the two potential transactions. However this should almost never happen, as it should only happen in the case where an attacker predicts a future transfer perfectly and tries to grief the user).

Because the transaction is just a simple transfer, it achieves the minimum gas cost possible and, because all wallets have logic for parsing transfers, it's very easy for all users to verify exactly what the transaction is doing and check it's not malicious, which improves UX compared to systems where you have to approve other contracts, interact with them and sign messages.

In the worse case scenario where there's a huge DDoS attack which could increase transaction amount significantly, then we fall back to transacting through a contract, so it's impossible for users to pay more than 0.005$ extra.

## Subscriptions

WIP
