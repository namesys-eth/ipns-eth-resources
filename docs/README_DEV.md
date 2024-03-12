![](https://raw.githubusercontent.com/namesys-eth/ipns-eth-resources/main/graphics/png/logo-small_dev.png)
&nbsp;

# `IPNS.dev`: Keyless Pinning Service

### Pin Content to IPNS Trustlessly and Securely!

##### By: [`sshmatrix`](@sshmatrix) & [`0xc0de4c0ffee`](@0xc0de4c0ffee) for NameSys

**Client `v1.0-beta`:** [`ipns.dev`](https://ipns.dev) | [`pin.namesys.xyz`](https://pin.namesys.xyz)

**GitHub:** [`@namesys/ipns-client`](https://github.com/namesys-eth/)

## Abstract
`IPNS.dev` is a no-nonsense, free, open-source, autonomous, trustless, keyless and secure IPNS Pinning Service. Users of `IPNS.dev` do not need to share their IPNS keys with service providers, and can securely and 'keylessly' publish to IPFS network with anonymity. To our knowledge, this is the first such service and public good in existence.

## Motivation
It's autonomous, trustless, keyless and secure, and it is the first of its kind. Users of IPNS have so far required to either share their private keys with service providers such as [1W3.io](https://1w3.io) or [dWebServices.xyz](https://dWebServices.xyz), or publish privately from their IPFS nodes. `IPNS.dev` is the first service which removes this severe security flaw and accessibility issue by employing a "keyless" interface to the IPFS network. Users' IPNS keys are generated deterministically from their Ethereum wallet signatures **only during an update**, and the content is then pinned and published on NameSys's fork of `w3name` publishing infrastructure deployed on Cloudflare.

&nbsp;
![](https://raw.githubusercontent.com/namesys-eth/ipns-eth-resources/main/graphics/png/keygen_dev.png)

## Specification
### a) Keyname
`keyname` is an identifier for an IPNS key, such that
```js
let keyname = 'keyname'
```

### b) Password
`password` is an optional `string` value used to salt the key derivation function (HKDF),
```js
let password = "horse staple battery"
```

### c) Chain-agnostic Identifiers
Chain-agnostic [CAIP-02: Blockchain ID Specification](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) and [CAIP-10: Account ID Specification](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) schemes are used to generate blockchain and address identifiers `caip02` and `caip10` respectively,
```js
let caip02 =
      `eip155:<EVM_CHAINID>` ||
      `cosmos:<HUB_ID_NAME>` ||
      `bip122:<16BYTE_HASH>`;

let caip10 = `${caip02}:<ADDR_CHECKSUM>`;
```

### d) Info
`info` is CAIP-10 and Keyname string formatted as:
 ```js
 let info = `${caip10}:${keyname}`;
 ```

### e) Message
**Deterministic** `message` to be signed by the wallet provider,
```js
let message = `Requesting Signature To Generate IPNS Key\n\nOrigin: ${keyname}\nKey Type: ed25519\nExtradata: ${extradata}\nSigned By: ${caip10}`
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

### f) Signature
[RFC-6979](https://datatracker.ietf.org/doc/html/rfc6979) compatible (ECDSA) deterministic `signature` calculated by the wallet provider using native keypair,
```js
let signature = wallet.signMessage(message);
```

### g) Salt
`salt` is SHA-256 hash of the `info`, optional password and last **32 bytes** of signature string formatted as:

```js
let salt = await sha256(`${info}:${password?password:""}:${signature.slice(68)}`);
```
where, `signature.slice(68)` are the last 32 bytes of the deterministic ECDSA-derived Ethereum signature.

### h) Key Derivation Function (KDF)
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
   The resulting `privKey` and `pubKey` is the `ed25519` deterministic keypair that can interact with IPFS network. 

## Implementation Requirements

- Connected Ethereum wallet Signer **MUST** be EIP-191 and RFC-6979 compatible.
- The `message` **MUST** be string formatted as
```
Requesting Signature To Generate IPNS Key\n\nOrigin: ${keyname}\nKey Type: ed25519\nExtradata: ${extradata}\nSigned By: ${caip10}
```
- HKDF `inputKey` **MUST** be generated as the SHA-256 hash of 65 bytes long signature.
- HKDF `salt` **MUST** be generated as SHA-256 hash of string
```
${info}:${password?password:""}:${signature.slice(68)}
```
- HKDF Derived Key Length (`dkLen`) **MUST** be 42.
- HKDF `info` **MUST** be string formatted as
```
${caip10}:${keyname}
```

## TS Example
```ts
import * as ed25519 from '@noble/ed25519'
import {hkdf} from '@noble/hashes/hkdf'
import {sha256} from '@noble/hashes/sha256'
import {ethers} from 'ethers'

let wallet = new ethers.Wallet(PRIVATE_KEY, provider)
let keyname = "keyname"
let chainId = wallet.getChainId(); // get ChainID from connected wallet
let address = wallet.getAddress(); // get Address from wallet
let caip10 = `eip155:${chainId}:${address}`;
let message = `Requesting Signature To Generate IPNS Key\n\nOrigin: ${keyname}\nKey Type: ed25519\nExtradata: ${extradata}\nSigned By: ${caip10}`
let signature = wallet.signMessage(message); // request Signature from wallet
let password = "horse staple battery"

/**
 * @param   keyname Key identifier
 * @param    caip10 CAIP identifier for the blockchain account
 * @param signature Deterministic signature from X-wallet provider
 * @param  password Optional password
 * @returns Deterministic private/public keypairs as hex strings
 * Hex-encoded
 * [ed25519.priv, ed25519.pub]
 */
export async function KEYGEN(
  keyname: string,
  caip10: string,
  signature: string,
  password: string | undefined
): Promise<[
  string, string
]> {
  if (signature.length < 64)
    throw new Error('SIGNATURE TOO SHORT; LENGTH SHOULD BE 65 BYTES')
  let inputKey = sha256(
    ed25519.utils.hexToBytes(
      signature.toLowerCase().startsWith('0x') ? signature.slice(2) : signature
    )
  )
  let info = `${caip10}:${keyname}`
  let salt = sha256(`${info}:${password ? password : ''}:${signature.slice(-64)}`)
  let hashKey = hkdf(sha256, inputKey, salt, info, 42)
  let ed25519priv = ed25519.utils.hashToPrivateScalar(hashKey).toString(16).padStart(64, "0") // ed25519 Private Key
  let ed25519pub = ed25519.utils.bytesToHex(await ed25519.getPublicKey(ed25519priv)) // ed25519 Public Key
  return [ // Hex-encoded [ed25519.priv, ed25519.pub]
    ed25519priv, ed25519pub
  ]
}

```

## Security Considerations

- Users **SHOULD** always verify the integrity and authenticity of their client before signing the message.
- Users **SHOULD** ensure that they only input their `keyname` and `password` in trusted and secure clients.

## References

- [RFC-6979: Deterministic Usage of the DSA and ECDSA](https://datatracker.ietf.org/doc/html/rfc6979)
- [RFC-5869: HKDF (HMAC-based Extract-and-Expand Key Derivation Function)](https://datatracker.ietf.org/doc/html/rfc5869)
- [CAIP-02: Blockchain ID Specification](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md)
- [CAIP-10: Account ID Specification](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md)
- [Digital Signature Standard (DSS), FIPS 186-4 B.4.1](https://csrc.nist.gov/publications/detail/fips/186/4/final)
- [ERC-191: Signed Data Standard](https://eips.ethereum.org/EIPS/eip-191)
- [EIP-155: Simple Replay Attack Protection](https://eips.ethereum.org/EIPS/eip-155)
- [ECDSA Signature Standard](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.38.8014)
- [@noble/hashes](https://github.com/paulmillr/noble-hashes)
- [@noble/secp256k1](https://github.com/paulmillr/noble-secp256k1)