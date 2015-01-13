The Skycoin Project is building the new internet. Figure out what needs to be done and do it.

There are three active projects which the main development team has committed resources to finishing for the next milestone. 
- developing new blockchain, /src/coin
- developing post-mining distributed consensus 
- developing low latency darknet suitable for meshnet operation, which allow community ownership of the networking infrastructure

Skycoin is developing a series of core-libraries and infrastructure. The Skycoin Project consists of modules. Many of the libraries are designed to be used by multiple applications. Writing a well written library module with a clean interface that can be used by multiple applications is ten times the effort and work as writing a module that merely works.

Implemented:
- cipher, the Skycoin Project cryptography library, [Cipher Tutorial](cipher). This standardizes hashing and cryptography across all modules in the Skycoin Project. This makes it easy to generate public/private key pairs from passwords (deterministic wallet generation), create signatures, verify signatures, encrypt data using public key cryptography and hash public keys into 20 byte addresses.
- Skywire/Daemon, the Skycoin meshnet/darknet/networking library. This is a next generation low latency crypto-VPN/Tor 2.0 project. This lets you run a "node" and route traffic. IP addresses are replaced by public key hashes (similar to Bitcoin addresses, instead of an address representing a destination for coins it represents a communication end-point). Applications need to be able to TCP in and begin communicating with other programs by the public key hash. Right now the library can connect over TCP/ip to IPv4 and IPv6 addresses and exchange length prefixed messages.
- Encoder, This is a utility library for serialization/deserialization of data structures
- Coin, This is the blockchain parser. It is done.
- Visor, This runs the blockchain as a service and exposes JSON APIs. The Visor API is moving towards a flexible Darkwallet type API
- Obelisk?, name may change. This is Skycoin's distributed consensus algorithm implementation, based upon a randomized ben-ors decision process over a social graph. This is currently being tested, benchmarked in Python before being rewritten into Golang and integrated. 
- GUI/Wallet/Interface. This is HTML/Javascript/Golang for users to interact with their wallets. This is in Angular and Node.js. It is a local web-wallet. This is done, but features like CoinJoin and transaction history, blockchain explorer are missing.

Planned/Prototyped/Needs to be Started. These are good projects to start working on.

- Merkle-DAG, this is the Skycoin distributed file-system. It is very simple. Based on IPFS and git. This uses Skywire for networking and uses the cipher module for cryptography. This needs to be implemented.
- DHT, The Skycoin Project needs a new type of DHT to implement other project infrastructure. This is basicly a distributed DNS system, for broadcasting information needed by different applications (such as routing information required for the darknet). This is related to Kadmelia, Bitmessage, Distributed DNS, Bitorrent DHT. Normal DHT is Key:Value, where Key is hash of value. Skycoin DHT element is a slotted structure. The key is a 20 byte hash of the public key publishing the data. The person performs a small proof-of-work to prevent spam, signs the hash of the entry with their private key and publishes the update. The DHT will by fully replicated Bitmessage style, with a gossip protocol to simplify implementation at start. The value field is a slotted data structure, so the protocol is extensible. If someone gives you an address for a node, you use this system to retrieve the entry published by the node which gives hints on routing information to the node. This is a very small library but very innovative and very important.

### Purpose of infrastructure components

Cipher - allows standardized, easy to use public key cryptographic and hashing operations across project. This is a major achievement already.

Skycoin - Skycoin is the token system that prevents leaching in the darknet and ensures users contribute as much back to network as they consume in resources. Skycoin funds the project development and funds meshnet deployment. Skycoin was written from scratch and designed over four years to realize the ideal of Bitcoin and represents the apex of cryptocurrency design. Skycoin is not designed to add features to Bitcoin, but rather improves Bitcoin by increasing simplicity, security and stripping out everything non-essential.

Some people have hyped the Skycoin Project as leading into "Bitcoin 3.0". The coin itself is not "Bitcoin 3.0", but is rather "Bitcoin 1.0". Bitcoin is a prototype crypto-coin. Skycoin was designed to be what Bitcoin would look like if it were built from scratch, to remedy the rough edges in the Bitcoin design.
- no duplicate coin-base outputs
- enforced checks for hash collisions
- simple deterministic wallets
- no transaction malleability
- no signature malleability
- clean, simple, modern API (based upon Darkwallet/Libbitcoin)
- removal of the scripting language
- indistinguishably between CoinJoin and normal transactions
- local HTML web-wallet
- improved usability and intuitive behavior that matches what users expect (such as not losing coins if you load a wallet from backup)
- a clean, modular software architecture with unit test coverage
- elimination of edge-cases that prevent independent node implementations.
- 10 second transaction times
- elimination of need for mining to achieve blockchain consensus

Skycoin consensus is the only "advance" that Skycoin makes over Bitcoin. Everything else is merely an attempt at doing well, the parts where Bitcoin fails. Solving unsolved problems, increases project technical risk, which must be minimized. We must make achievable commitments and be very selective about where we attempt "innovation".

Each "innovation" requires a full and indefinite commitment. The consensus algorithm is the one place where the risk of project failure was offset by the benefit. The consensus algorithm took over four years to develop, has evolved significantly in the past six months and is still undergoing testing and improvement.