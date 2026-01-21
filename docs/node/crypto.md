# `crypto` Module

> Cryptography utilities: hashes, HMAC, ciphers, signatures, keys, random bytes

---

## Import (ESM)

```js
import {
  // Hashing
  createHash,
  createHmac,
  getHashes,

  // Ciphers
  createCipheriv,
  createDecipheriv,
  getCiphers,

  // Sign / Verify
  createSign,
  createVerify,

  // Key generation & handling
  generateKeyPair,
  generateKeyPairSync,
  createPublicKey,
  createPrivateKey,
  createSecretKey,

  // Randomness
  randomBytes,
  randomUUID,
  randomFill,
  randomFillSync,

  // KDF / Passwords
  pbkdf2,
  pbkdf2Sync,
  scrypt,
  scryptSync,

  // Constants
  constants
} from 'node:crypto';
```

<details>
<summary>Examples</summary>

```js
import { createHash, randomBytes } from 'node:crypto';
```

</details>

---

## Hashing

```js
// Create hash instance
createHash(algorithm, options?)        // (string, object?) → Hash
// Create HMAC instance
createHmac(algorithm, key, options?)   // (string, string|Buffer|KeyObject, object?) → Hmac
// List supported hash algorithms
getHashes()                             // () → string[]
```

<details>
<summary>Examples</summary>

```js
createHash('sha256', { outputLength: 32 });     // algorithm, options
createHmac('sha256', 'secret', {});             // algorithm, key, options
getHashes();                                    // no args
```

</details>

---

## Hash / Hmac Methods

```js
// Add data to hash
hash.update(data, inputEnc?)           // (string|Buffer, string?) → this
// Finalize hash
hash.digest(outputEnc?)                // (string?) → Buffer|string
```

<details>
<summary>Examples</summary>

```js
createHash('sha256')
  .update('data', 'utf8')               // data, encoding
  .digest('hex');                       // output encoding
```

</details>

---

## Ciphers (Encryption)

```js
// Create cipher
createCipheriv(alg, key, iv, opts?)    // (string, Buffer|KeyObject, Buffer, object?) → Cipher
// Create decipher
createDecipheriv(alg, key, iv, opts?)  // (string, Buffer|KeyObject, Buffer, object?) → Decipher
// List supported ciphers
getCiphers()                           // () → string[]
```

<details>
<summary>Examples</summary>

```js
const key = randomBytes(32);            // key
const iv = randomBytes(16);             // iv
createCipheriv('aes-256-cbc', key, iv, {});
createDecipheriv('aes-256-cbc', key, iv, {});
getCiphers();
```

</details>

---

## Cipher / Decipher Methods

```js
// Encrypt/decrypt data
cipher.update(data, inEnc?, outEnc?)   // (string|Buffer, string?, string?) → Buffer|string
// Finalize cipher
cipher.final(outEnc?)                  // (string?) → Buffer|string
// Enable/disable padding
cipher.setAutoPadding(auto)            // (boolean) → this
```

<details>
<summary>Examples</summary>

```js
cipher.update('text', 'utf8', 'hex');   // data, inputEnc, outputEnc
cipher.final('hex');                    // outputEnc
cipher.setAutoPadding(true);            // boolean
```

</details>

---

## Signing & Verification

```js
// Create signer
createSign(algorithm)                  // (string) → Sign
// Create verifier
createVerify(algorithm)               // (string) → Verify
```

<details>
<summary>Examples</summary>

```js
createSign('RSA-SHA256');
createVerify('RSA-SHA256');
```

</details>

---

## Sign / Verify Methods

```js
// Add data to sign
sign.update(data, inputEnc?)           // (string|Buffer, string?) → this
// Generate signature
sign.sign(key, outEnc?)                // (KeyObject|string|object, string?) → Buffer|string
// Verify signature
verify.verify(key, sig, sigEnc?)       // (KeyObject|string|object, Buffer|string, string?) → boolean
```

<details>
<summary>Examples</summary>

```js
sign.update('msg', 'utf8');
sign.sign(privateKey, 'hex');           // key, outputEnc
verify.verify(publicKey, signature, 'hex');
```

</details>

---

## Key Generation & Handling

```js
// Generate key pair (async)
generateKeyPair(type, opts, cb)        // (string, object, Function) → void
// Generate key pair (sync)
generateKeyPairSync(type, opts)        // (string, object) → object
// Create public key object
createPublicKey(key)                   // (string|Buffer|object) → KeyObject
// Create private key object
createPrivateKey(key)                  // (string|Buffer|object) → KeyObject
// Create secret key object
createSecretKey(key)                   // (Buffer) → KeyObject
```

<details>
<summary>Examples</summary>

```js
generateKeyPair('rsa', { modulusLength: 2048 }, () => {});
generateKeyPairSync('rsa', { modulusLength: 2048 });
createPublicKey(pubPem);
createPrivateKey(privPem);
createSecretKey(randomBytes(32));
```

</details>

---

## Randomness

```js
// Generate cryptographically strong bytes
randomBytes(size, cb?)                // (number, Function?) → Buffer
// Generate RFC 4122 UUID
randomUUID(opts?)                     // (object?) → string
// Fill buffer with random data
randomFill(buf, offset?, size?, cb)   // (Buffer, number?, number?, Function) → void
// Sync version
randomFillSync(buf, offset?, size?)   // (Buffer, number?, number?) → Buffer
```

<details>
<summary>Examples</summary>

```js
randomBytes(16);                       // size
randomUUID({ disableEntropyCache: false });
randomFill(Buffer.alloc(8), 0, 8, () => {});
randomFillSync(Buffer.alloc(8), 0, 8);
```

</details>

---

## Password / Key Derivation

```js
// PBKDF2 (async)
pbkdf2(pwd, salt, iter, len, digest, cb) // (string|Buffer, string|Buffer, number, number, string, Function) → void
// PBKDF2 (sync)
pbkdf2Sync(pwd, salt, iter, len, digest)  // (...) → Buffer
// Scrypt (async)
scrypt(pwd, salt, len, opts?, cb)        // (string|Buffer, string|Buffer, number, object?, Function) → void
// Scrypt (sync)
scryptSync(pwd, salt, len, opts?)        // (...) → Buffer
```

<details>
<summary>Examples</summary>

```js
pbkdf2('pass', 'salt', 1000, 32, 'sha256', () => {});
pbkdf2Sync('pass', 'salt', 1000, 32, 'sha256');
scrypt('pass', 'salt', 32, { N: 16384 }, () => {});
scryptSync('pass', 'salt', 32, { N: 16384 });
```

</details>

---

## Crypto Constants

```js
// RSA padding modes
constants.RSA_PKCS1_PADDING
constants.RSA_PKCS1_PSS_PADDING

// Diffie-Hellman
constants.DH_CHECK_P_NOT_SAFE_PRIME

// Timing-safe compare result
constants.CRYPTO_BUFFER_SIZE
```

<details>
<summary>Examples</summary>

```js
constants.RSA_PKCS1_PADDING;            // padding mode
constants.CRYPTO_BUFFER_SIZE;           // internal buffer size
```

</details>

---

## Notes

* Prefer `scrypt` for password hashing
* Use `randomBytes` for secrets
* Never reuse IVs with block ciphers
