# DDC SDK “Built-In Encryption” RFP

**Goal**  
Today the DDC SDK encrypts user data only if the *caller* plugs in a cipher (e.g. `NaclCipher`). We want **encryption out-of-the-box** so every developer gets strong, audited protection without extra work.  
  
Within the first 2 days we expect you to deliver the first built-in cipher: **AES-256** (CBC mode + PKCS7 padding), mirroring the existing `NaclCipher` API.

**Scope of Work**  
  1. **`AES256Cipher` implementation** – pure TypeScript, zero external deps except Node `crypto`.  
  2. **Plug-in registration** – export the class from `@cere-ddc-sdk/core/src/cipher/` alongside `NaclCipher`.  
  3. **Unit tests** – full round-trip coverage (happy path + bad key/iv, tampered ciphertext, etc.).
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

## Quick Start Guide 🚀

### 1. Setup environment

1. Clone GitHub repository: [`cere-ddc-sdk-js`](#)
2. Download and install Node.js **v18.17.1** [Node.js – Download Node.js](https://nodejs.org/)

### 2. Run DDC SDK Playground

1. Install dependencies:
    ```bash
    npm i
    ```
2. Build packages and playground application
    ```bash
    npm run build
    ```
3. Run playground application
    ```bash
    npm run playground
    ```
4. Open [http://localhost:5174/](http://localhost:5174/)
5. Upload and download file to ensure it’s working
   1. Keep default Seed phrase as is and click `Continue` button
   2. Select `Testnet` and click `Continue` button
   3. Click `Skip`
   4. Keep default Cluster (`0x825...`) and use existing bucket `573062`
   5. Click `Skip`
   6. Select file on your local file system you would like to upload and click `Continue`
   7. Click a blue link to download a file

### 3. Run DDC CLI

1. Install `ddc-cli` package
    ```jsx
    npm install -g @cere-ddc-sdk/cli
    ```
2. Install `npx`

    ```jsx
    npm install -g npx
    ```

3. Put any file you would like to upload to the same directory from where you execute commands
4. Upload file

    ```jsx
    npx @cere-ddc-sdk/cli upload ./hello-world.txt network=testnet --signer="hybrid label reunion only dawn maze asset draft cousin height flock nation" --bucketId=573062
    ```

5. More details and examples you can find here: https://github.com/Cerebellum-Network/cere-ddc-sdk-js/blob/main/packages/cli/README.md
---

## Resources 📚
- DDC SDK (Monorepo): [GitHub Link](https://github.com/Cerebellum-Network/cere-ddc-sdk-js)
- CLI Package: [GitHub Link](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/packages/cli)
- Playground App: [GitHub Link](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/playground)

---

With AES-256 shipped as a first-class cipher, every dApp that stores data through the **DDC SDK** enjoys strong, peer-reviewed encryption by default — no extra code, no extra risk. Future milestones can layer on more algorithms (ChaCha20-Poly1305, ECIES) using the same plug-in pattern.

