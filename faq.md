# FAQ

## How the field "saved ether" on the dashboard is calculated ?

We always try to execute your transactions with a bit less gas price than the actual gas price. It's safe to try has our replay mechanism will take over if the transaction is stuck. The amount of "ether saved" is simply the sum of ether saved by these "underpriced" transactions.





