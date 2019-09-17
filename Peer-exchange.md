# Forming the network

The Skycoin client has a set of hardcoded peers to bootstrap from. The client will try to maintain one connection to at most one of these peers. These peers are labelled `"trusted"` in the network status APIs.

With `-downloads-peerlist`, the client also downloads a list of peers from https://downloads.skycoin.com/blockchain/peers.txt.  All peers in this file should have their listen port open to receive connections.

Custom peers can be loaded from disk with `-custom-peers-file`.

## Peer Exchange (PEX)

Peers are shared between clients using a peer exchange (PEX) protocol.

There are 2 messages used in PEX, GETP and GIVP.  GETP is a request for more peers, and GIVP is the reply.  Up to 20 peers randomly selected from the peerlist are sent as a GIVP message.  The receiver of GIVP updates their peer pool with these peers, marking them as seen.  Unsolicited connections from an unknown peer cause that peer to be added to the peerlist.  The list of seen peers is cached in the [data directory](https://github.com/skycoin/skycoin/wiki/Data-directory-and-wallet-folder-locations) in a file `peers.json`.
