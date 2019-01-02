### What are coin hours?

Coin hours are a parallel currency within the Skycoin blockchain, alongside skycoins.  Coin hours are used to pay transaction fees in the network.  In the future, coin hours will be used in other applications beyond the Skycoin cryptocurrency, such as [Skywire](github.com/skycoin/skywire).

Coin hours are generated based upon the age of coins in a transaction output.

### What is a transaction output?

A Skycoin transaction consumes one or more outputs and creates one or more outputs.  Each output has an owner address, an amount of coins and an amount of hours.

When you send coins to someone, you destroy some outputs that are owned by addresses in your wallet and you create some new outputs.

### How are coin hours generated?

For every 1 coin in an output, 1 coin hour is created after 1 hour elapses.  Time is measured between the blockchain's current head block timestamp and the timestamp of the block in which the output was created.

Internally, the value is calculated as "coin seconds", then rounded down to the nearest whole coin hour. This implies that:

* A 0.1 skycoin output generates 1 coin hour every 10 hours (36000 seconds)
* A 1 skycoin output generates 1 coin hour every 1 hour (3600 seconds)
* A 10 skycoin output generates 1 coin hour every 6 minutes (360 seconds)

Generated coin hours are calculated at the time that the output is spent, and are added to the output's initial coin hours.  The elapsed time is the timestamp of the previous block, not a computer's clock time.

### How are coin hours used for transaction fees?

A transaction must destroy a percent of its total input coin hours in order to be valid.  It can optionally destroy more, to prioritize the transaction in a congested network.  Currently, the percent that must be destroyed is 50%. The value is always rounded up (i.e., if there are 5 input coin hours, 3 must be destroyed).  A transaction must destroy at least 1 coin hour.

Transactions are prioritized for inclusion in a block by their coin hour burn fee per byte.  Larger transactions (measured in bytes) require a higher amount of coin hours burned to achieve a higher priority.  