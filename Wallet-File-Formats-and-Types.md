## Wallet file format

Wallet files are files in the configured wallets folder (`~/.skycoin/wallets` by default) that have the extension `.wlt`.  When the daemon/client start, all `*.wlt` files from the configured wallets folder are loaded. If any wallet file is detected as invalid, the daemon/client will not start.  Wallet files **MUST NOT** be modified while the daemon/client are running, or else those modifications may be overwritten by the daemon/client when it is quit.

Wallet files contain data in JSON format. Although there can be multiple types of wallet files with different data structures, they all must contain a `"meta"` field pointing to a JSON object.

When the wallet file is loaded, the JSON data is scanned for the `meta.type` field.  Then, this type is used to determine the rest of the wallet file structure to load.

### Wallet metadata 

The wallet file's `"meta"` field object has the following fields:

* `coin`: Required. Can be `skycoin` or `bitcoin`.  Refers to the address format.  All fiber coins will be `skycoin` since they all have the same address format.  Note that `bitcoin` coin type is not supported in the daemon, but the cli has limited support for it.
* `cryptoType`: The encryption method for encrypted wallets. The only value should be `scrypt-chacha20poly1305`.
* `encrypted`: A boolean indicating if the wallet is encrypted or not
* `filename`: The filename of the wallet
* `label`: The label of the wallet, to display in the UI
* `lastSeed`: the last seed created by a `deterministic` type wallet.  Required for unencrypted `deterministic` wallets, but should be empty for any other type of wallet
* `secrets`: An encrypted JSON string, containing the wallet's secret data (seed, lastSeed, and private keys)
* `seed`: the initial seed for a `deterministic` type wallet.
* `seedPassphrase`: The [bip39 seed passphrase](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki#from-mnemonic-to-seed). Not required, but only used for `bip44` wallets.
* `tm`: the creation timestamp of the wallet
* `type`: Required. Can be `collection`, `deterministic`, `bip44` or `xpub`. Refers to the way keys are managed by the wallet.
* `version`: The wallet metadata version.  Current version is `0.2` which added encryption support.

## Wallet types

### Skycoin deterministic Wallets

Skycoin deterministic wallets have the type `deterministic` and use Skycoin's custom [[deterministic keypair generation method]].  From an initial seed, the wallet will generate a single chain of private keys indefinitely.  The seed can be any arbitrary array of bytes, but typically it is a bip39 mnemonic as bytes. The deterministic generation method does not support arbitrary indexing like bip44.

Skycoin deterministic wallets can be created and managed from the web UI.

Keypairs are saved as an array in an `entries` field in the wallet JSON data. A deterministic wallet with an empty or missing `entries` array is invalid.

A `deterministic` type wallet file with no entries is invalid and will not be loaded by the software.

#### How to manage a deterministic wallet from the cli

*Note: do not perform cli operations on a wallet file while it is loaded in the Skycoin daemon or client wallet software, as this can cause data loss*

```sh
# create a deterministic wallet
# the wallet will have 1 address by default; it cannot be empty
# use `walletCreate --help` for more options
go run cmd/cli/cli.go walletCreate -t deterministic -f mywallet.wlt --encrypt
# generate more addresses
go run cmd/cli/cli.go walletAddAddresses -f mywallet.wlt -n 10
```

### BIP44 Wallets

BIP44 wallets have the type `bip44` and implement the [bip44 HD (hierarchical deterministic) wallet spec](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki).  From a valid [BIP39 mnemonic seed](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) and an optional ["seed passphrase"](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki#from-mnemonic-to-seed), two sequences of up to 4294967295 keypairs each can be generated. The first sequence is called "external" and is intended for receiving coins publicly. The second sequence is called "change"; it is internal, and is used for change addresses. When spending from a `bip44` wallet, a new change address is generated for each transaction to avoid [address reuse](https://en.bitcoin.it/wiki/Address_reuse).

Skycoin does not implement the "account" feature of BIP44 wallets. The "account" node number is always 0.

Skycoin does not have a BIP44 `coin_type` assigned in https://github.com/satoshilabs/slips/blob/master/slip-0044.md yet, but is using `8000` while in development.

Keypairs are saved in two separate arrays in the JSON file on disk: `external_entries` and `change_entries`.  However, in APIs, the two arrays are combined into a single `entries` array. Each element in the array has `child_number`, indicating the bip44 path leaf node number, and `change`, which can be `0` or `1`, where `0` is the external chain and `1` is the `change` chain.

A `bip44` type wallet file with no entries in its external chain is invalid and will not be loaded by the software.

#### How to manage a bip44 wallet from the CLI

```sh
# create a bip44 wallet
# the wallet will have 1 address by default; its external chain cannot be empty
# use `walletCreate --help` for more options
go run cmd/cli/cli.go walletCreate -t bip44 -f mywallet.wlt --encrypt
# generate more addresses
go run cmd/cli/cli.go walletAddAddresses -f mywallet.wlt -n 10
```

### XPub Watch Wallets

XPub watch wallets have the type `xpub`. The wallet is configured with a single "xpub" (extended public) key.
An xpub key can be exported from a `bip44` wallet using the CLI command `walletKeyExport`.

`xpub` wallets can generate new addresses to receive coins or monitor balances, but cannot spend from these addresses. This enables better security for exchange and payments systems integrations, because the "hot" wallet can generate new addresses for deposits or customer payments, without exposing the private keys of that wallet on a live server.

*Warning: Avoid sharing xpub keys. See https://en.bitcoin.it/wiki/Deterministic_wallet_tools#Risks_of_Sharing_an_Extended_Public_Key_.28xpub.29*

More information about xpub keys:

* https://en.bitcoin.it/wiki/Deterministic_wallet_tools#Risks_of_Sharing_an_Extended_Public_Key_.28xpub.29
* https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki
* https://support.blockchain.com/hc/en-us/articles/360000939843-Understanding-the-xPub-and-address-generation

### Collection Wallets

Collection wallets have the type `collection` and are an arbitrary collection of private keys. Their type string is "collection".  A collection wallet does not have a seed and thus cannot generate new private keys automatically. Private keys can only be added manually. The lack of a seed also implies that if the wallet is encrypted and the password is lost, the private keys in the collection cannot be recovered.

Keypairs are saved as an array in an `entries` field in the wallet JSON data. Collection wallets may have an empty `entries` array.

Collection wallets can't be created through the UI but can be created through the cli.

#### How to manage a collection wallet from the cli

*Note: do not perform cli operations on a wallet file while it is loaded in the Skycoin daemon or client wallet software, as this can cause data loss*

```sh
# create the empty collection wallet
# use `walletCreate --help` for more options
go run cmd/cli/cli.go walletCreate -t collection -f mycollection.wlt --encrypt
# add a private key to the wallet
go run cmd/cli/cli.go addPrivateKey -f mycollection.wlt $PRIVATE_KEY
```