# NIP-07 Encrypt / Decrypt POC

A minimal single-page proof-of-concept for **NIP-04 encryption and decryption** using a [NIP-07](https://github.com/nostr-protocol/nips/blob/master/07.md) browser signer extension.

## What it does

- Calls `window.nostr.nip04.encrypt(pubkey, plaintext)` to encrypt a message
- Calls `window.nostr.nip04.decrypt(pubkey, ciphertext)` to decrypt a message
- All cryptography happens client-side inside the browser extension — no keys ever touch this page

## Prerequisites

A **NIP-07 compatible browser extension** such as:
- [nos2x](https://github.com/nicehash/nos2x)
- [Alby](https://getalby.com)
- [Flamingo](https://www.getflamingo.org/)

The extension manages your Nostr private key and exposes `window.nostr` to web pages.

## Run locally

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

No build tools, no dependencies — just a single `index.html`.

## How NIP-04 encryption works

1. **ECDH shared secret** — Your private key + their public key (or vice versa) derive a shared secret via Elliptic Curve Diffie-Hellman on secp256k1.
2. **AES-256-CBC** — The shared secret is used as the key for AES-256-CBC encryption. A random IV is generated per message.
3. **Payload format** — The ciphertext is base64-encoded and transmitted as `<base64>?iv=<base64iv>`.

Both parties derive the same shared secret, so either can encrypt/decrypt messages to/from the other.

## License

Public domain / unlicense.
