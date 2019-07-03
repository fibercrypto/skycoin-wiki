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
* `tm`: the creation timestamp of the wallet
* `type`: Required. Can be `collection` or `deterministic`. Refers to the way keys are managed by the wallet.
* `version`: The wallet metadata version.  Current version is `0.2` which added encryption support.

## Wallet types

### Deterministic Wallets

Deterministic wallets use Skycoin's custom [[deterministic keypair generation method]].  From an initial seed, the wallet will generate a single chain of private keys indefinitely.  The deterministic generation method does not support arbitrary indexing like bip44.

Deterministic wallets can be created and managed from the web UI.

Keypairs are saved as an array in an `entries` field in the wallet JSON data. A deterministic wallet with an empty or missing `entries` array is invalid.

#### How to manage a deterministic wallet from the cli

*Note: do not perform cli operations on a wallet file while it is loaded in the Skycoin daemon or client wallet software, as this can cause data loss*

```sh
# create an empty deterministic wallet
# use `walletCreate --help` for more options
go run cmd/cli/cli.go walletCreate -t deterministic -f mywallet.wlt --encrypt
# generate more addresses
go run cmd/cli/cli.go walletAddAddresses -n 10
```

### Collection Wallets

Collection wallets are an arbitrary collection of private keys. Their type string is "collection".  A collection wallet does not have a seed and thus cannot generate new private keys automatically. Private keys can only be added manually. The lack of a seed also implies that if the wallet is encrypted and the password is lost, the private keys in the collection cannot be recovered.

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