# `IPNS.eth`: Keyless Pinning Service by NameSys

### Pin Content to IPNS Trustlessly and Securely!

##### By : [`sshmatrix.eth`](@sshmatrix) & [`freetib.eth`](@0xc0de4c0ffee) for NameSys

## Abstract

No-nonsense, autonomous, trustless, keyless and secure IPNS Pinning Service.

## Motivation

It's autonomous, trustless, keyless and secure. It is first of its kind. That's plenty of motivation.

&nbsp;
![](https://raw.githubusercontent.com/namesys-eth/ipns-eth-resources/main/graphics/png/keygen.png)

## Specification

### a) Origin

`origin` is associated with a blockchain account `ETH_ADDR` deriving IPNS keys, such that

```js
let origin = `eth:<ETH_ADDR_CHECKSUM>` || `btc:<BITCOIN_ADDR>` || `sol:<SOLANA_ADDR>`
```

### b) Keyname
`keyname` is a identifier for an IPNS key, such that

```js
let keyname = 'keyname'
```

### c) Password
`password` is an optional `string` value used to salt the key derivation function (HKDF),
```js
let password = "horse staple battery"
```

### d) Chain-agnostic Identifiers
Chain-agnostic [CAIP-02: Blockchain ID Specification](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) and [CAIP-10: Account ID Specification](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) schemes are used to generate blockchain and address identifiers `caip02` and `caip10` respectively,
```js
let caip02 =
      `eip155:<EVM_CHAINID>` ||
      `cosmos:<HUB_ID_NAME>` ||
      `bip122:<16BYTE_HASH>`;

let caip10 = `${caip02}:<ADDR_CHECKSUM>`;
```

### e) Info
`info` is CAIP-10 and Keyname string formatted as:
 ```js
 let info = `${caip10}:${keyname}`;
 ```

### f) Message
Deterministic `message` to be signed by the wallet provider,
```js
let message = `Requesting Signature To Generate IPNS Key\n\nOrigin: ${origin}\nKey Type: ed25519\nExtradata: ${extradata}\nSigned By: ${caip10}`
```

such that

```solidity
// EXTRADATA
bytes32 extradata = keccak256(
            abi.encodePacked(
              keccak256(
                abi.encodePacked(password)
              ),
              ETH_ADDR_CHECKSUM
              )
            );
```

### g) Signature
[RFC-6979](https://datatracker.ietf.org/doc/html/rfc6979) compatible (ECDSA) deterministic `signature` calculated by the wallet provider using native keypair,
```js
let signature = wallet.signMessage(message);
```

### h) Salt
`salt` is SHA-256 hash of the `info`, optional password and last **32 bytes** of signature string formatted as:

```js
let salt = await sha256(`${info}:${password?password:""}:${signature.slice(68)}`);
```
where, `signature.slice(68)` are the last 32 bytes of the deterministic ECDSA-derived Ethereum signature.

### i) Key Derivation Function (KDF)
HMAC-Based KDF `hkdf(sha256, inputKey, salt, info, dkLen = 42)` is used to derive the **42 bytes** long **hashkey** with inputs,

- `inputKey` is SHA-256 hash of signature bytes,
   ```js
   let inputKey = await sha256(hexToBytes(signature.slice(2)));
   ```

- `info` is same as defined before, i.e.
   ```js
   let info = `${caip10}:${keyname}`;
   ```

- `salt` is same as defined before, i.e.
   ```js
   let salt = await sha256(`${info}:${password?password:""}:${signature.slice(68)}`);
   ```

- `dkLen` (Derived Key Length) is set to `42`,
   ```js
   let dkLen = 42;
   ```
   [FIPS 186-4 B.4.1](https://csrc.nist.gov/publications/detail/fips/186/4/final) requires hashkey length to be `>= n + 8`, where `n = 32` is the **bytelength** of the final `ed25519` private key, such that `42 >= 32 + 8`.

- `hashToPrivateScalar()` function is FIPS 186-4 B.4.1 implementation to convert HKDF-derived hashkey to valid `ed25519` keypair. This function is implemented in JavaScript library `@noble/ed25519` as `hashToPrivateScalar()`.

   ```js
   let hashKey = hkdf(sha256, inputKey, salt, info, dkLen = 42);
   let privKey = ed25519.utils.hashToPrivateScalar(hashKey);
   let pubKey = ed25519.utils.getPublicKey(privKey);
   ```