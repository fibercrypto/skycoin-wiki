# Forming the network

The Skycoin client uses the [Mainline DHT](https://en.wikipedia.org/wiki/Mainline_DHT) to bootstrap peer discovery.  The string `"skycoin-skycoin-skycoin-skycoin-skycoin-skycoin-skycoin"` is treated as the info field to be hashed and encoded for identifying Skycoin peers.

Once the initial peers are bootstrapped, Skycoin uses a peer exchange mechanism ([PEX](#peer-exchange-pex)) to round out the network.

## Peer Exchange (PEX)

There are 2 messages used in PEX, GETP and GIVP.  GETP is a request for more peers, and GIVP is the reply.  Up to 20 peers randomly selected from the peerlist are sent in response.  The receiver of GIVP updates their peer pool with these peers, marking them as seen.  Peers that have not been seen in over a week are discarded.  When interacting with a peer listed in the peerlist, if they violate any conditions, they are blacklisted for that condition's blacklist period.  Blacklists are not exchanged at this time.  Unsolicited connections from an unknown peer cause that peer to be added to the peerlist.

# Blacklist Conditions

TODO