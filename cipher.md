Cipher is the cryptography library for Skycoin. It is designed to make cryptography easy. The library was designed to be the foundation of multiple applications and for usage beyond the coin implementation.

## Background on Public Key Cryptography

Public key cryptography allows you to generate a private key and public key
- the private key is secret and only you know the private key
- the public key can be shared

There are two operations
- signatures: You can sign data with your private key and anyone can verify that only the person knowing the private key could have produced the signature. This is how Bitcoin/Skycoin transactions are authorized. The person who knows the private key for an address, "owns" the Bitcoin, in that they are able to authorize transactions by signing them.
- encryption: Anyone who knows your public key can encrypt data, so that only the person knowing the secret key can read it.

## Summary of Functionality

Cipher is currently based on secp256k1 and SHA256

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

  //now you can sign messages or encrypt things
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

//load address from base58 string, panic on failure
addr2, err := cipher.MustDecodeBase58Address(""WyPXrQpAJ7bL6kXZ9ZB6c1p3yUMhBMF7u8")
```

## Darkwallet Addresses

A darkwallet address allows someone with your public key to generate infinite unused addresses which you can receive coins to.

For a darkwallet address, you give someone your public key. They generate a throwaway ephemeral key which is published somewhere (in the blockchain). You both do an operation and get the same secret. The secret is used to generate a private key which only you two know. The coins are sent to the address for this private key. Both you and the sender have access to the coins and so you then move the coins to a new place.

```go
//this is your private/public key pair. You give them the public key
seckey1, pubkey1 := GenerateDeterministicKeyPair([]byte("Password"))
//This is their key pair
seckey2, pubkey2 := GenerateKeyPair()

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
- you compute EDCH with their public key and your private key
- they compute EDCH with their private key and your public key
- the values computed are equal! You now have a shared secret.
- the shared secret is used to generate an address and only you two know the private key for this address

Dark Wallet In Practice
- you give someone your public key. They can now generate infinite numbers of unused addresses, to send you coins with.
- To send you coin they generate a new pubkey, send the coins they want to send you to the address of that pubkey
- then they spend the coins from that address into the shared secret address (the dark wallet address)
- then you look for transactions with one input, one output
- you compute the ECDH for your pubkey against each of these transactions and see if it yields the private-key for the output
- if the private key works, you claim the coins by moving the coins into your wallet
- if no one claims the coins after a certain period, the person may spend the coins in the shared address, back into their wallet



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
