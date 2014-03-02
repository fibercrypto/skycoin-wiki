This is guide for contributors. This is the road map for how you can help Skycoin.

=== skycoin/src/coin hash function tests ===

Add unit tests for SHA256 hashes
- unit test for "hello" and known strings
- unit test for hashing two SHA256 hashes together
- unit test for binary Merkle tree
 
===  scrypt hash function wrapper for golang ===

- we need it for deterministic wallet generation. Will be harder to brute force wallets than SHA256
- find a one file implementation of scrypt in C and wrap it with cgo

=== ChaCha20 Golang Wrapper ===

- we need it for the darknet project

=== golang wifi controller library ===

- only needs to work on linux
- wraps ifconfig?
- lists wifi devices
- scans for wifi networks from golang
- connecting and disconnected to networks
- connection status and speed information
- adhoc mode support?

=== gnet handshake ===

Gnet is our connection pool and packet serialization library.

https://github.com/skycoin/gnet

Currently random people are connecting to skycoin clients and sending random data. We want a handshake that says that a connection is running the correct gnet application. 

For instance, on connection, send random number. person receiving hashes the number with "skycoin" and then sends it back. Then connection is "open". Packets should not be passed through gnet until after handshake.

=== Blob Replication ===

skycoin/src/sync/blob_replicator.go

Blobs are byte arrays that are propagated with a gossip protocol.

Blobs are byte arrays. We hash the array to get a hash. When a client gets a new blob it doesnt have, it announces the hash to its peers. Peers then choose to download the blob if they dont have it. There is a callback function for validating the blob.

Blobs are used for transaction propagation across network and blobs will be used for emergency messaging. We want someone to think about blobs replication and design something better. For instance, having a request que and only have so many outstanding requests per peer.

=== Sync Algorithm ===

Given a large list of hashes on one machine, we need to be able to compare that list to a list of hashes on another machine, find the hashes that are different. Needs to use minimum amount of bandwidth and round trip packets. Something like rsync? Merkle tree?

The datastructure should be quick to update as hashes are removed and inserted.

=== BEP/RFC Needed: Address Messaging Service ===

We want drafts of secure messaging protocols for sending asynchronous  messages to pubkeys. This protocol is for machine to machine communications, but chat and email services could be built on top of it.

- want to be able to send messages to addresses or pubkeys (pubkeys are the destination address)
- asynchronous 
- May use PoW like Bitmessage to prevent spam
- do not want to replicate every messages to everyone like Bitmessage
- Messages must be stored when user is offline and user receives messages when online

Components to think about
- how to find where to send message, just using their pubkey/address
- how messages are encoded so only recipient can read them
- how messages are stored for user when they are offline

The system does not need to be completely decentralized. It can use syndicated chat servers, which communicate with each other (like Jabber).

