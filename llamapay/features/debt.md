# Debt

### Problem

One of the issues encountered with current stream payment platforms happens when you forget to top up your balance and run out.&#x20;

With SuperFluid, their bot sends a transaction that will cancel your streams and takes a part of your money (no refunds).&#x20;

To get your stream working again you would have to:

* Create all streams from scratch again
* Calculated the money recipients lost while the streams are down and send them manually
* Accept your losses

This defeats the point of automatic payments and increases your workload instead of reducing it.

### Solution

LlamaPay solves this issue by introducing the debt feature.&#x20;

When your balance gets depleted, instead of losing money and having to go through the work of restarting the streams, you incur debt. The next time you deposit, the debt is paid and streams keep working as usual. If the payer really meant to stop the streams, they can just not deposit and the payee will be able to withdraw the money they received up to when the payer's balance was depleted. They can also cancel individual streams which will remove their debt.

Once the payer deposits, the money cannot be removed and can only be withdrawn to the payee's wallet.

LlamaPay gives the option to the payer to just resume streams and repay debt easily, greatly simplifying the process in case they forgot or couldn't top up in time.
