## Skycoin Concepts Guide

### 1. Introduction

This document contains notes information for developers working with Skycoin and its products.

### 2. What is Skycoin

- Skycoin is a blockchain based cryptocurrency designed for simplicity and security.
- The Skycoin blockchain is also a platform, called Fiber.
- The Fiber platform components include the [blockchain](github.com/skycoin/skycoin), [desktop wallet](github.com/skycoin/skycoin), [web wallet](github.com/skycoin/skycoin-web), mobile wallets, an OTC distribution utility called [Teller](github.com/skycoin/teller)
- The Skycoin blockchain has a programmable smart contract language called [CX](github.com/skycoin/cx), still in development.
- The infrastructure is replicated for each coin (unlike Ethereum) which wishes to launch using the platform.  That is, each coin has their own blockchain.

### 2.1 Skycoin blockchain specifics

- There is no mining.
- Transaction fees are in coin hours, not the coin itself.  The fee is calculated as a percentage of the transaction's total input coin hours.
- The smallest allowed amount of coins is 0.000001, although it is artificially restricted to 0.001 at present, to constrain the maximum unspent output pool size.
- Coins cannot be created or destroyed in a transaction. A transaction's input coins must equal its output's coins.

### 3. Skycoin concepts

#### 3.1 Input/Output

Like Bitcoin (and unlike Ethereum) all transactions are defined by one or more inputs and one or more outputs.
Inputs are links to an output in previous transaction.
The output coins must add up to the total of the input coins.

__See:__ https://bitcoin.org/en/developer-guide#transactions

#### 3.2 Wallets

It is defined by a “.wlt” file on the client machine in the configured data folder.
It contains the seed, public and private keys and addresses of the wallets accounts.
A wallet can have multiple addresses, but they are all generated from the same seed (in sequence); this is called a deterministic wallet.