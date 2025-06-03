# DDC SDK “Built-In Encryption” RFP — 60-Second Overview

**Goal**  
  Today the DDC SDK encrypts user data only if the *caller* plugs in a cipher (e.g. `NaclCipher`). We want **encryption out-of-the-box** so every developer gets strong, audited protection without extra work.  
  *Milestone 1* delivers the first built-in cipher: **AES-256** (CBC mode + PKCS7 padding), mirroring the existing `NaclCipher` API.

**Scope of Work (Milestone 1)**  
  1. **`AES256Cipher` implementation** – pure TypeScript, zero external deps except Node `crypto`.  
  2. **Plug-in registration** – export the class from `@cere-ddc-sdk/core/src/cipher/` alongside `NaclCipher`.  
  3. **Unit tests** – full round-trip coverage (happy path + bad key/iv, tampered ciphertext, etc.).  
  4. **Docs & Typedoc** – short “how to switch ciphers” snippet in `packages/core/README.md`.

**Deliverables**  
  * New source under `packages/core/src/cipher/AES256Cipher.ts`  
  * ≥ 95 % line coverage for the cipher folder  
  * Example usage in `examples/encryption/aes256.ts`  
  * CHANGELOG entry + semver bump (e.g. `v0.5.0-aes`)  
  * PR passes CI (lint ➜ build ➜ tests ➜ type-check)

**Success criteria**  
  * `encrypt()` + `decrypt()` produce identical plaintext for all test vectors.  
  * All existing SDK tests remain green; bundle size increase ≤ 5 KB gzipped.  
  * No new *crypto-lint* warnings (ESLint rule: `no-weak-crypto`).

---

## Quick-Start for External Contributors

> **Goal:** clone the repo, run the new AES-256 tests, and see them pass in < 10 minutes.

### 1 · Clone & bootstrap

```bash
git clone https://github.com/Cerebellum-Network/cere-ddc-sdk-js.git
cd cere-ddc-sdk-js
pnpm i          # workspace install
````

### 2 · Run the full test suite

```bash
pnpm test                # Jest + ts-node
```

### 3 · Play with the cipher locally

```bash
node examples/encryption/aes256.js <<'EOF'
Hello, encrypted world!
EOF
```

Expected output:

```
Ciphertext (base64): ...
Decrypted again: Hello, encrypted world!
```

### 4 · Lint & type-check

```bash
pnpm lint     # eslint --max-warnings=0
pnpm build    # tsc --noEmit
```

### 5 · Submit your PR

1. Fork the repo, push a feature branch `feat/aes256-cipher`.
2. Open a PR against **`rfp/built-in-encryption`** with the checklist template ticked.
3. Ensure the GitHub Actions workflow shows all ✅ before requesting review.

---

With AES-256 shipped as a first-class cipher, every dApp that stores data through the **DDC SDK** enjoys strong, peer-reviewed encryption by default — no extra code, no extra risk. Future milestones can layer on more algorithms (ChaCha20-Poly1305, ECIES) using the same plug-in pattern.

