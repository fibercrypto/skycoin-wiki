## Sizes of various transactions

* Genesis transaction: 86 bytes
* Genesis to distribution transaction: 3846 bytes
* Smallest possible non-genesis transaction (1 input, 1 output): 183 bytes
* Transaction with 1 input, 2 outputs (send to one with change): 220 bytes
* Transaction with 2 inputs, 2 outputs: 317 bytes

Each additional output in the transaction adds 37 bytes.
Each additional input in the transaction adds 97 bytes.

## Block size

The block size is configurable. It is currently set to 32786 bytes (32kB).  The total size of transactions in a block cannot exceed this amount.  A single block can hold up to 179 transactions, if they are all the smallest possible transaction (1 input, 1 output).

## Block rate

The block rate is configurable. It is current set at 10 seconds.  Every 10 seconds, pending transactions from the unconfirmed transaction pool are chosen to be placed into a block, based upon a priority criteria.

## Transaction rate

Using the configured defaults of 32kB block size and a 10 second block rate, and the smallest possible transaction size of 183 bytes, the maximum transaction rate is 17.9 transactions per second or 64,440 transactions per hour.  Since the block size and block rate are adjustable, the transaction rate can be changed to accomodate the needs of the network.

## Transaction prioritization

Transactions are chosen for block placement by their coin hour fee per byte, which is the number of coin hours destroyed by the transaction divided by the size of the transaction in bytes.  In the case of a tie, the transaction hashes are compared lexicogprahically with the first one being chosen.  This provides determinism in transaction selection.

## Blockchain growth

At 10 blocks per second, 32kB per block, the maximum blockchain growth rate is 283.1MB/day or 103.3GB/yr.  Actual size of data on disk is larger due to the need to index this data, which adds overhead.
