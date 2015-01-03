Cipher is the cryptography library for Skycoin. It is designed to make cryptography easy. The library was designed to be the foundation of multiple applications and for usage beyond the coin implementation.

## Background on Public Key Cryptography

Public key cryptography allows you to generate a private key and public key
- the private key is secret and only you know the private key
- the public key can be shared

There are two operations
- signatures: You can sign data with your private key and anyone can verify that only the person knowing the private key could have produced the signature. This is how Bitcoin/Skycoin transactions are authorized. The person who knows the private key for an address, "owns" the Bitcoin, in that they are able to authorize transactions by signing them.
- encryption: Anyone who knows your public key can encrypt data, so that only the person knowing the secret key can read it.

## Summary of Functionality

Cipher is based on secp256k1 and SHA256. It uses the same encryption algorithm as Bitcoin.

The package contains
- public/private key generation
- deterministic public/private key generation from a passphrase or seed
- serialization and deserialization of public keys to hex strings
- generation of Skycoin addresses from a pubkey.
- ability to sign hashes with private keys and verify signatures
- curve multiplication operation for encrypting data to a pub key.
- hashing operations (SHA256 and binary merkle-tree)

future functionality:
- secp256k1-ChaCha20 will be supported (default encryption options)
- Bitcoin, Litecoin, Dogecoin address generation
- ability to encrypt and decrypt directories and files

## This is how the package is used

```go
package main

import (
  "github.com/skycoin/skycoin/src/cipher" //import cipher package
  "fmt"     
)

func main() {
  var pub cipher.PubKey // a public key
  var sec cipher.SecKey // a secret key
  
  pub, sec := cipher.GenerateKeyPair() //generate a keypair

  //now you can sign messages, encrypt things or generate addresses
}
```

## Generating Addresses and Key Pairs

```go
//Generate a keypair randomly
pub,sec := cipher.GenerateKeyPair()
```

```go
//Generate a keypair from a pass phrase
pub,sec := cipher.GenerateDeterministicKeyPair([]byte("Password")) 
```

```go

var seckey cipher.SecKey
var pubkey cipher.PubKey
var addr cipher.Address

//Generate a skycoin address for a given public key
pub,sec = GenerateDeterministicKeyPair([]byte("Password")) 
addr = cipher.AddressFromPubKey(pubkey) //this is the address
//get base58 encoded form of address
addr_str := addr.String()

//print address and pubkey
log.Printf("pubkey= %s, address= %s", pubkey.String(), addr.String())
```

```go
//load address from base58 string
addr2 := cipher.MustDecodeBase58Address("WyPXrQpAJ7bL6kXZ9ZB6c1p3yUMhBMF7u8")

//load private key from hex string
seckey := MustSecKeyFromHex("f399bd1b78792da9cc49b1157c73016450c949df565ce3ddbf2f9d65fd8f0dac")

//load public key from hex string
pubkey := MustPubKeyFromHex("03e56ab0597167882813864bd71305660edc128d45ed41ff583b15a44e4e95233f")

```

## Deterministic Wallet Addresses

To generate N addresses deterministicly from a wallet passphrase or seed, do
```go
N := 16 //generate 16 addresses
secret_passphrase = []byte("Secret Wallet Passphrase")
seckeys := cipher.GenerateDeterministicKeyPairs(secret_passphrase)

//lets print out the addresses for out our private keys in deterministic wallet
for index, seckey in range seckeys {
  pubkey := cipher.PubKeyFromSecKey(seckey)
  addr := cipher.AddressFromPubKey(pubkey)

  fmt.Printf("num= %s, address= %s, public_key= %s, secret_key= %s", index, addr.String(), pubkey.Hex(), seckey.Hex())
}
```

## Sending an Encrypted Message to Someone

To send an encrypted message
1. Get their public key
2. Generate a throw-away ephemeral key with, pubkey2,seckey2 := GenerateKeyPair()
3. Encrypt the message with AES using key, cipher.ECDH(pubkey, seckey2), where pubkey is their key
4. Append pubkey2 to front of encrypted message so they can decrypt it.
5. Deliver the message

To decrypt an encrypted message
1. Take the first 33 bytes off the front of the message to get pubkey2.
2. Compute cipher.ECDH(pubkey2, seckey) with your seckey and the pubkey they sent
3. Decrypt the rest of the message with AES, using cipher.ECDH(pubkey2, seckey) as the key
4. read the message

Future version will have a simple to use encryption/decryption wrapper. There will also be a gui and it will be easier to use than PGP.

It will also be possible to securely message private keys through Skywire.

## Signature Operations


Sign SHA256 hash of message with private key, return signature 

```go
signature := SignHash(hash, seckey)
```

Verify the signature for the hash

```go
err := VerifySignedHash(sig, hash)
if err != nil {
  log.Printf("Signature invalid!")
}
```

We use compressed signatures, so the public key can be recovered from the signature itself.

## Darkwallet Addresses

A darkwallet address allows someone with your public key to generate infinite unused addresses which you can receive coins to.

For a darkwallet address, you give someone your public key. They generate a throwaway ephemeral key which is published somewhere (in the blockchain). You both do an operation and get a shared secret. The secret is used to generate a private key which only you two know. The coins are sent to the address for this private key. Both you and the sender have access to the coins and so you then move the coins to a new place.

```go
//this is your private/public key pair. You give them the public key
pubkey1, seckey1 := GenerateDeterministicKeyPair([]byte("Password"))
//This is their key pair
pubkey2, seckey2:= GenerateKeyPair()

//They communicate pubkey2, by for instance using it as the first destination for coins in a transaction
//You know pubkey2, and seckey1/pubkey1
//They know your pubkey, pubkey1 and know their seckey seckey2/pubkey2

//You computer 
//use your private key and the pubkey they gave you
secret1 := cipher.ECDH(pubkey2, seckey1) 

//Their computer
//They use your pubkey and their private key
secret2 := cipher.ECDH(pubkey2,seckey1)

//secret1 and secret2 are equal!
//now you may compute an address from the them
pubkey, seckey := GenerateDeterministicKeyPair(secret1)
address := cipher.AddressFromPubkey() //send coins here
```

To review:
- you send them your public key
- they generate a new public key and send you that
- you compute ECDH with their public key and your private key
- they compute ECDH with their private key and your public key
- the values computed are equal! You now have a shared secret.
- the shared secret is used to generate an address and only you two know the private key for this address

Dark Wallet In Practice
- you give someone your public key. They can now generate infinite numbers of unused addresses, to send you coins with.
- To send you coin they generate a new pubkey, send the coins they want to send you to the address of that pubkey
- then they spend the coins from that address into the shared secret address (the dark wallet address)
- then you look for transactions with one input, one output. You use the public key of input address for ephemeral key.
- you compute the ECDH for your pubkey against each of these transactions and see if it yields the private-key for the output
- if the private key works, you claim the coins by moving the coins into your wallet
- if no one claims the coins after a certain period, the person may spend the coins in the shared address, back into their wallet

This achieves dark wallet transactions in three transactions, but guarantees no information is leaked. To achieve it in two transactions, you can choose a convention such as
- using the pubkey of the address for the first input to the transaction as the ephemeral key
- or using the public key of the Nth input for the Nth output, as the potential ephemeral key

However for this you must ensure that the same public key is not reused as the ephemeral key for the same pubkey/dark wallet address or the same address will be generated and information will leak. One solution, is to salt the secret with a hash, such as salting the ECDH secret by the hash of the unspent output consumed.

Another consideration is that dark wallet transactions should be coinjoin compatible. One minimum leakage two-transaction dark wallet protocol is as follows
- You send public key
- They create transaction with N inputs M outputs. One or more of the outputs is to your darkwallet address.
- for each unspent output consumed in the transaction, the potential ephemeral pubkeys are the pubkey of the address spending the output.
- to generate the secret, the ECDH secret is hashed with the unspent output hash. This generates a unique address always. Even if the address the coins are spent from and destination dark-wallet address are jointly repeated.
- For each transaction with N inputs, you compute the N possible addresses from the ECDH operation plus hash and check the outputs for resulting dark addresses.
- Without your private key, no one can even determine if dark-wallet transactions even exist within the transaction.
- The order of the inputs and outputs in the transaction do not matter and therefore it is coinjoin compatible without the coinjoin server doing anything.
- If there are more dark-outputs than inputs, then we can hash an existing secret to generate a chain of secrets. Then one input can be split into five outputs to dark wallets.

For instance:
- a user may take 1 input and split it into 3 outputs to your dark wallet address and 2 change outputs (sending coins back to himself)
- the transaction may be executed in a coin-join transaction so the transactions are mixed in with transactions from other users
- You now have to move the coins from the 3 shared dark-wallet output addresses into a private wallet

For privacy, it is important to note the following. If you spend the 3 shared dark-wallet outputs into the same wallet in a single transaction, it says "All the inputs from this transaction belong to the same wallet/person". So it makes senses to claim the outputs by moving the outputs one at a time, over time, in separate coin-join transaction.

If you merge coins from two addresses into a single address, it also gives away that the two addresses belong to the same wallet. However, in a coin-join transaction there are inputs and outputs from multiple users and only the coin-join server can tell which input/output belong to who. So in a coin-join merge transaction with two users, where there are 4 inputs and 2 outputs it is not clear which of the two inputs belong to each user. 

No blockchain will ever provide absolute privacy or be as anonymous as cash, because the chain of all transactions is public record and can always be traced.  Governments who have access to exchange records will be easily able to determine where money is going and more importantly are able to determine which coins have been "cleaned" before they were moved onto an exchange. However, this is a significant improvement for user privacy.

We did an information theory and statistical analysis of coin transaction patterns. In Bitcoin, enough information is leaked that anyone can identity most wallets and possibly users from the public data. In a system with minimum information leakage, governments will still be able to trace transactions from the data created at exchanges interfacing the coins to the banking system. Most coins will only go through a few transaction before ending back at an exchange (or a merchant who must report the transaction data). However, people without the government data, wont be able to trace the transactions. 

In countries where coins are banned or where regulation drives people to underground exchanges, the reporting data wont exist and there will be absolute privacy. In countries with liberal policies, but who collect data at the exchange, they may not always be able to identify the full transaction path. However, they will be able to determine which users are suspiciously effective at protecting their transaction privacy and whose coins are suspiciously clean. Coinbase is already banning users for receiving coins that have gone through tumblers or have been used on websites associated with drugs.

Mandatory or default coin-join would improve privacy but create a situation, where any user could be arbitrarily targeted for raids or harassment because effectively all coins would become tainted with immoralities the recipient would otherwise take care to distance himself from. Therefore it makes sense to allow for some separation between coins used for legitimate commerce and coins which may have been tainted.

When a darknet site like Silk Road is raided, in the future instead of taking it down, they will seize the servers and continue operating the service. They will record the transactions and trace them back to Coinbase, BitPay shipping addresses and potentially do global SWAT raids on tens of thousands of people across hundreds of cities. The risk of allowing even a 30% false positive rate from coin mixing is significant.

Police are seizing Bitcoin and auctioning them off for money before trial. They do not have to charge the person to seize and sell the Bitcoin. Police have an incentive to go after the people with the most money to seize and people with the most activity. So even though privacy can be enhanced at a technical level, enabling that functionality as the default creates social problems that offset the advantages.

In the long-term, dark wallet addresses will not be necessary. Communication addresses over Skywire, will give an off-blockchain channel for communicating new addresses and receipts for each transaction.

## Encrypting Files and Directories

TODO. Not implemented yet.

## Hardware Devices

We believe that encryption/decryption and signature operations should be handled by a hardware device if possible. This ensures safety of coins and private keys, even if the computer is hacked or compromised.

In the long term, we plan to fund the development of such devices (a five dollar, 32 bit ARM processor the size of USB thumbstick). We are planning an interface to the encryption library which transparently handles signing operations from external key-storage devices.

## Example Application For Public Key Cryptography 

An example application, is that you want to save files so you can read them later but you dont want someone who steals your computer to be able to access them. For instance, you have a video camera that is producing 1 image per second and saving it to disc.

If symmetric encryption was used, the server would need to store the private key to encrypt the images. The person stealing the server would also be stealing the private key and would be able to view the images. 

- If the computer was shutdown and the private key was stored only in RAM, then the private key would be lost if the computer was unplugged. However, the private key would need to be re-entered every time the computer booted (which is a waste of time). This is how disc encryption works currently.
- However, a prepared attacker could hack the live system with a USB or firewire device and acquire the private key before seizing the server.

Using pub key encryption you would generate a public/private key deterministically from a pass phrase. Then you only store the public key on the server (on disc).
- Each image is encrypted with the public key, so that only the person with the private-key can read the images. 
- If the the server is stolen, nothing is recoverable without the private key. However, the server can still encrypt new files.
- The public key can safely be stored on disc, so the computer can be rebooted and still operate automatically.

To implement this, you would
- generate a public/private key pair
- write program that loads the public key from disc as a string and looks in a directory for images, reads in new images, encrypts them for the public key, renaming the images
- write program that takes in the private key to decrypt the files

This library is designed to make implementing a protocol like this very easy.