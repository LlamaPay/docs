# How are gas fees so cheap?

The main issue a payments gateway has to solve is how to connect a transfer with a payment request, so if an app requests a 100$ payment from multiple users and later there's a 100$ transfer, how do you know which user did the payment?

### Previous solutions

The most common solution that payment gateways use is they create a new address for each payment, so if money ends up at that address then you know for sure which user sent it. However this means that the payment gateway needs to later send that money again to aggregate all payments, which costs extra transaction fees, and it also needs to take custody of funds, which exposes it to hacks.

Another solution is to do the transfer through a contract where a unique id is attached to the transaction, allowing identification of which payment this transaction is paying. The issue with this is that adding a contract to the transaction increases gas costs and number of transactions needed, as first you need to approve the token against the contract.

### Our solution

Before making a payment, users send a request to our server claiming the payment, such as "I will pay payment ABCDE from address 0x123", then when a transaction comes from address 0x123 we know that it must be paying ABCDE.

But what happens if two different users claim the same address for different payments? In that case we add 0.000001$ to the second payment such that it's possible to perfectly differentiate between the two potential transactions. However this should almost never happen, as it should only happen in the case where an attacker predicts a future transfer perfectly and tries to grief the user.

Because the transaction is just a simple transfer, it achieves the minimum gas cost possible and, because all wallets have logic for parsing transfers, it's very easy for all users to verify exactly what the transaction is doing and check it's not malicious, which improves UX compared to systems where you have to approve other contracts, interact with them and sign messages.

In the worse case scenario where there's a huge DDoS attack which could increase transaction amount significantly, then we fall back to transacting through a contract, so it's impossible for users to pay more than 0.005$ extra.

## Subscriptions

#### Constant costs

Gas costs for retrieving subscription payments in our contracts are independent of the number of payers. This is important because other systems rely on pulling tokens from payers' wallets directly or on a contract that needs to perform actions for each payer, thus the gas costs grow linearly with the number of payers.

Because of that, gas costs can end ballooning significantly when you reach hundreds or thousands of players, while adding unpredictable liabilities because you don't know how much those gas fees will cost you in the future. Our system keeps costs constant so as you grow costs will always remain cheap.

#### Yield buffer

On top of that, our contracts have a yield buffer that significantly reduces costs when users deposit money, making costs costs equivalent to those you'd get if there was no yield farming involved at all.

LlamaPay subscriptions generate yield by depositing funds on AAVE, but doing so can increase 2-3x the gas costs of deposit transactions. To avoid that, users deposit only to our contract, and once enough deposits have accumulated, we have a bot that triggers a deposit to AAVE, amortizing costs.

To illustrate, imagine the contract has 1m deposited on aave and there are users depositing money for 5$/mo subscriptions for a year (so 60$ per user). Instead of depositing into aave on each transaction, we would wait till there are 2.1k$ on the contract and then trigger a deposit for everyone, thus lowering the total gas costs by 35x (there's only 1 aave deposit instead of 35, one for each transaction).

This introduces economies of scale since it's shared among all the companies that use llamapay, and it also improves security because if there was some bug around aave deposits an attacker wouldn't be able to trigger it at will within their attack transaction.
