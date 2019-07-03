Skycoin has a custom deterministic keypair generation method.  Wallets which use this method have the `deterministic` type.  The method is described below:

## Initial seed

The initial seed is any arbitrary byte array.  By default, skycoin uses the byte array representation of a bip39 mnemonic string. *Note: this is NOT a [bip39 seed](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki#From_mnemonic_to_seed), it is just the mnemomic as bytes*.  This seed is stored in the wallet file metadata as a string (if using arbitrary bytes, you should hex encode the string, but note that the seed will become the hex-encoded bytes, and not the decoded bytes).

## Iterative step

The deterministic iteration step is defined as follows:

Define a function `findSecretKey`:

0. Compute `k = sha256(b)`, where `b` is an sha256 digest
0. If `k` is an invalid secret key, compute `k = sha256(k)`
0. Repeat the previous step until `k` is a valid secret key
0. Return `k`

Define a function `secp256k1Hash`:

0. Compute `h = sha256(seed)`, where seed is a byte array with sufficient entropy
0. Compute `k = findSecretKey(h)`
0. Compute `p = pubkeyOf(findSecretKey(sha256(h)))`. Note that `sha256(h)` is usually equal to `k`, but not if `sha256(h)` is not a valid secret key.
0. Compute `e = ecmult(p, k)`, where `ecmult` is the [elliptic curve point multiplication operation](https://en.wikipedia.org/wiki/Elliptic_curve_point_multiplication) (i.e. `e = p * k mod G`)
0. Compute `d = sha256(h || e)`
0. Return `d`

Using the previous definitions, define the generation method `X` as:

0. Compute `a = secp256k1Hash(seed)`, where seed is a byte array with sufficient entropy
0. Compute `b = sha256(seed || a)`
0. Compute `k = findSecretKey(b)`
0. Return `k` as the secret key and `a` as the seed for the next iteration

Pseudocode to generate a chain of keys from a seed:

```
# initial seed can be any arbitrary bytes
# use something with higher entropy
seed = sha256("foo bar baz quz qux")  
var secKeys[10]
for i = 0; i < 10; i++ {
  secKeys[i], seed = X(seed)
}
```