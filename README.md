# Developer Console “Top-Up” RFP

- **Goal**
  We need a service that can top-up user accounts by charging a stored credit card and triggering the on-chain transaction through an external ramp service. The **Top-Up** micro-service listen to a low-balance event (or a user request) and automatically fund the account, so builders never hit a hard stop.

![image](https://github.com/user-attachments/assets/123965b4-ba52-42e3-b110-07889f49fffc)

- **Scope of Work**
    1. **I/O**
       - Input: amount, wallet_id, Credit Card authorisation token
       - Data stored: Vault-tokenised credit-card auth (PCI-DSS SAQ-A)
       - Processing: Charge card → receive auth-ID → call external ramp service for on-chain top-up
       - Output → “JSON {tx_hash, timestamp} + webhook callback”
    2. **Trigger & Listener**
       - Event source: low-balance webhook
       - Listener: top-up endpoint
    3. **Ramp integration**
       - trigger an external ramp service that in turn delivers the $CERE tokens into the user account (can be a placeholder contract/wallet)
    5. **Credit Card Authorisation**
       - manage credit authorisation by storing a token securily in a database, enabling automated top-ups by pre-authorising the credit card
      
  **Estimated development time**: 1 day

- **Deliverables**  
  * Source code  under `cluster-apps/apps/developer-console/topup-service/`  
  * Unit & e2e tests (≥ 90 % line coverage)
  * E2E flow video recording 
  * README quick-start (below) and change-log  

- **Success criteria**  
  * A fresh account with ≤ threshold balance is automatically replenished within **5 seconds** (can be on testnet).  
  * The full test suite passes in CI; container image publishes to GitHub Packages; Helm install works out-of-the-box.

---

## Quick Start Guide 🚀

**1. Setting Up Your Environment 🛠️**

- **Clone the Repository**
  ```bash
  git clone https://github.com/Cerebellum-Network/cluster-apps.git
  ```
- **Payment Provider Test Accounts**
  - Sign up for a Stripe test account and obtain test API keys.
- **Configure the Project**
  - Update configuration files (e.g., `.env`) with your test API keys.

**2. Running the Base Implementation ⚙️**

- **Install Dependencies**
  ```bash
  npm install
  # or
  yarn install
  ```
- **Start the Application**
  ```bash
  npm start
  # or
  yarn start
  ```
- **Test the Base Functionality**
  - Access the Developer Console at `http://localhost:3000` and attempt a test top-up.

