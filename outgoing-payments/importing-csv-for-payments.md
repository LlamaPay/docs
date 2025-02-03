# Importing CSV for Payments

## CSV Format

Amounts should all be in the normal denomination of the token, ignoring the decimals. For example if you want to vest 100 DAI you'd write 100, not 100000000000000000000 (18 decimals). All time-related fields are denominated in months.

The following is the structure of the table along with a few sample rows:

<table><thead><tr><th width="155">Token Address</th><th>Recipient Address</th><th width="146">Token Amount</th><th>Release Date (YYYY-MM-DD)</th></tr></thead><tbody><tr><td>0x6b175474e89094c44da98b954eedeac495271d0f</td><td>0x08a3c2A819E3de7ACa384c798269B3Ce1CD0e437</td><td>1000</td><td>2023-11-25</td></tr><tr><td>0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2</td><td>0xF6B171B1D778194b4bdE6af91Ce0CDEB01825A9B</td><td>3</td><td>2025-01-05</td></tr><tr><td>0x9f8f72aa9304c8b593d555f12ef6589cc3a579a2</td><td>0xF6B171B1D778194b4bdE6af91Ce0CDEB01825A9B</td><td>505</td><td>2025-06-20</td></tr></tbody></table>

You can find an example of a csv on [https://llamapay.io/csvs/payments.csv](https://llamapay.io/csvs/payments.csv)

If any date is in the past the payment will be instantly sent as soon as it's scheduled.
