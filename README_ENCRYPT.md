# DDC SDK “Built-In Encryption” RFP

**Goal**  
Today the DDC SDK encrypts user data only if the *caller* plugs in a cipher (e.g. `NaclCipher`). We want **encryption out-of-the-box** so every developer gets strong, audited protection without extra work.  
  
Within the first 2 days we expect you to deliver the first built-in cipher: **AES-256** (CBC mode + PKCS7 padding), mirroring the existing `NaclCipher` API.

**Scope of Work**  
  1. **`AES256Cipher` implementation** – pure TypeScript, zero external deps except Node `crypto`.  
  2. **Plug-in registration** – export the class from `@cere-ddc-sdk/core/src/cipher/` alongside `NaclCipher`.  
  3. **Unit tests** – full round-trip coverage (happy path + bad key/iv, tampered ciphertext, etc.).
  4. **UI integration** - integration in DDC Playground & CLI .
  5. **Docs & Typedoc** – short “how to switch ciphers” snippet in `packages/core/README.md`.

**Deliverables**  
  * New source under `packages/core/src/cipher/AES256Cipher.ts`  
  * ≥ 95 % line coverage for the cipher folder  
  * Example usage in `examples/encryption/aes256.ts`  
  * CHANGELOG entry + semver bump (e.g. `v0.5.0-aes`)  
  * PR passes CI (lint ➜ build ➜ tests ➜ type-check)

**Success criteria**  
  * `encrypt()` + `decrypt()` return identical plaintext for all NIST test vectors.  
  * No existing SDK tests regress; bundle-size increase ≤ 5 KB gzipped.  
  * No new `eslint-plugin-security` / `no-weak-crypto` warnings.  
  * Work completes in **1 dev-day** (timeline confirmed by Ulad).

---

## Quick-Start for External Contributors

### 1 · Clone & bootstrap

```bash
# 1. Clone the repo
git clone https://github.com/Cerebellum-Network/cere-ddc-sdk-js.git
cd cere-ddc-sdk-js

# 2. Use Node.js v18.17.1 (recommended); then install deps
pnpm i         # workspace-wide
````

### 2 · Run the SDK Playground 

```bash
pnpm run build          # builds all workspace packages
pnpm run playground     # starts Vite dev server

# Open http://localhost:5174/
# → Upload a test file and immediately download it back.
```
<details> <summary>Step-by-step playground flow (click to expand)</summary>
Keep the default seed phrase & click Continue

Choose Testnet & click Continue

Click Skip when asked for “Identity”

Accept the default cluster (e.g. 0x825…) and existing bucket

Skip encryption options (for now)

Pick any file to upload → click Continue

Click the blue link to download the file back

</details>


---

With AES-256 shipped as a first-class cipher, every dApp that stores data through the **DDC SDK** enjoys strong, peer-reviewed encryption by default — no extra code, no extra risk. Future milestones can layer on more algorithms (ChaCha20-Poly1305, ECIES) using the same plug-in pattern.

