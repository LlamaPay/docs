# Importing CSV for Vesting



<div align="left">

<figure><img src="../../.gitbook/assets/Screenshot from 2022-09-26 19-09-02.png" alt=""><figcaption></figcaption></figure>

</div>

### Format for CSV

Amounts should all be in the normal denomination of the token, ignoring the decimals. For example if you want to vest 100 DAI you'd write 100, not 100000000000000000000 (18 decimals). All time-related fields are denominated in months.

The following is the structure of the table along with a few sample rows:

<table><thead><tr><th width="155">Recipient Address</th><th>Amount To Vest </th><th>Time Vested (Month)</th><th>Cliff Time (Month)</th><th>Start Date (YYYY-MM-DD)</th></tr></thead><tbody><tr><td>0x08a3c2A819E3de7ACa384c798269B3Ce1CD0e437</td><td>1000</td><td>12</td><td></td><td></td></tr><tr><td>0x08a3c2A819E3de7ACa384c798269B3Ce1CD0e437</td><td>500</td><td>5</td><td>1</td><td></td></tr><tr><td>0xF6B171B1D778194b4bdE6af91Ce0CDEB01825A9B</td><td>1500</td><td>24</td><td>12</td><td>2022-10-05</td></tr></tbody></table>

You can find an example of a csv on [https://llamapay.io/csvs/vesting.csv](https://llamapay.io/csvs/vesting.csv)
