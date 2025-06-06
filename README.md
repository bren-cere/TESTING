# Developer Console “Top-Up” RFP

- **Goal**  
  Developer-Console accounts occasionally run out of credits ($CERE tokens) needed to enjoy an uninterrupted service by the cluster. The **Top-Up** micro-service will detect a low-balance event (or a user request) and automatically fund the account, so builders never hit a hard stop.

- **Scope of Work**  
  1. **Event Listener** – listens to Decentralized Data Cluster (DDC) for `BalanceLow` events and to an HTTP endpoint for manual top-up requests.  
  2. **Ramp integration** – trigger an external ramp service that in turn delivers the $CERE tokens into the user account
  3. **Credit Card Authorisation** – manage credit authorisation by storing a token securily in a database, enable automated top-ups by credit card 

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

