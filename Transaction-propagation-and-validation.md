When a user broadcasts a transaction, the following happens:

* `GIVT` (`GiveTransactionsMessage`) is sent to all connected peers. This message contains the transaction in full.
* Peers perform validation on the transaction.
* * If the transaction is valid, it is injected into their unconfirmed pool and announced to peers
* * If the transaction is soft-invalid, it is injected into the unconfirmed pool. For clients v0.24.1 and earlier, the txn is not announced. For clients v0.25.0 and later, the txn is announced.
* * If the transaction is hard-invalid, it is ignored.

Transactions in the unconfirmed pool are periodically checked for validity. If they transition from soft-invalid to valid (e.g. if the coinhours grow enough to satisfy the coinhour burn fee), they are announced to peers. If they transition from any state to invalid, they are removed from the pool (e.g., if one of the inputs was spent by another transaction).

Valid transactions in the unconfirmed pool are periodically announced to peers.

Transactions are announced by sending an `ANNT` (`AnnounceTransactionsMessage`) to a peer. This packet contains transaction hashes. The receiving peer will reply with a `GETT` (`GetTransactionsMessage`) for any txid it does not recognize. The originating peer will reply with a `GIVT` (`GiveTransactionsMessage`) with the full transaction for each txid in `GETT`.

Soft-invalid transactions are transactions that violate "soft" parameters, reasons are:

* Too many decimal places
* Coinhour burn fee too low
* Exceeding max transaction size

Hard-invalid transactions are transactions that violate "hard" parameters, reasons are:

* Transaction binary data does not deserialize to a `coin.Transaction` object
* Transaction inputs do not exist or have already been spent
* Total number of input coins does not match total number of output coins
* Total number of output coins or output hours overflows a uint64
* Total number of output hours exceeds total number of input hours (this could be a soft-invalid parameter in the future, to allow transactions to unlock over time, once the coinhours grew)
* Creates an output with zero coins
* Invalid input signature
* Number of signatures does not match number of outputs
* Transaction creates multiple outputs with the same hash (this means a single transaction cannot create more than one output sending the same number of coins and hours to the same address)
* Transaction creates an output that has the same hash as an existing output (this would be a hash collision)