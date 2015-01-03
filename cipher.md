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

## Example Application For Pubkey Key Cryptography 



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

##

This is how the package is used

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

  fmt.
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
