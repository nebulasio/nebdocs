# Crypto Design Doc

Similar to Bitcoin and Ethereum, Nebulas also adopted an elliptic curve algorithm as its basic encryption algorithm for Nebulas transactions. Users' private keys will be encrypted with their passphrases and stored in a keystore.

## Hash

Support generic hash functions, like sha256, sha3256 and ripemd160 etc.

## Keystore

The Nebulas Keystore is designed to manage users' keys.

### Key

The Key interface is designed to support various keys, including symmetric keys and asymmetric keys.

### Provider

The Keystore provides different methods to save keys, such as `memory_provider` and `persistence_provider`. Before storage, the key has been encrypted in the keystore.

* `memory provider`: This type of provider keep keys in memory. After the key has been encrypted with the passphrase when the user setkey or loads, it is cached in memory provider.
* `persistence provider`: This type of provider serializes the encrypted key to the file. The file is compatible with Ethereum's keystore file. Users can back up the address with its privatekey in it.

### Signature

The Signature interface is used to provide applications with the functionality of a digital signature algorithm. A Signature object can be used to generate and verify digital signatures.

There are two phases, in order to use a Signature object for signing data :

* Initialization: with a private key, which initializes the signature for signing \(see initSign\(\) in the source code of go-nebulas\).
* Signing of all input bytes.

A Signature object can recover the public key with a signature and the plain text that was signed \(see function RecoverSignerFromSignature in go-nebulas\). So just comparing the from address and the address derived from the public key can verify a transaction

> Similar to the [Android Keystore](https://developer.android.com/training/articles/keystore.html), TPM, TEE and hardware low level security protection will be supported as a provider later.

