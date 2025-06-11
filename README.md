# Developer Console “Top-Up” RFP

- **Goal**
  We need a service that can top-up user accounts by charging a stored credit card and triggering the on-chain transaction through an external ramp service. The **Top-Up** micro-service listen to a low-balance event (or a user request) and automatically fund the account, so builders never hit a hard stop.

## 🌲Data Model / Endpoints

Flow for topping up a customer's DDC account, integrating payment and blockchain services, all managed within a cluster.

---

  **Estimated development time core functionality**: 1 day

---

## User Stories

- **User Successfully Tops Up DDC Wallet via Credit/Debit Card**
  - User logs in, enters amount and card details, payment is processed, DDC balance increases, confirmation displayed, transaction recorded.

- **Payment Security and Compliance**
  - Card fields rendered via secure, PCI DSS-compliant IFRAME; raw card data never stored.

- **Handling Failed or Declined Payments**
  - Clear error messages for failed payments, no funds deducted, failed transactions logged.

- **Payment Method Management**
  - Users can add, remove, or update payment methods securely; changes are immediate and confirmed.

---

# 🥡 Contribution Guide

To integrate the Top-Up component into the Developer Console UI, follow these structured steps for both the frontend and backend implementations:

### **Frontend: Developer Console UI Integration**

- **Branch Creation**
    
    Begin by creating a new branch from the development branch to isolate your Top-Up component changes.
    
- **Repository Setup**
    
    Clone the main project repository to your local environment:
    
    ```jsx
    git clone https://github.com/Cerebellum-Network/cluster-apps.git
    ```
    
- **Payment Provider Configuration**
    - Register for a Stripe test account and generate test API keys as per [Stripe's documentation](https://docs.stripe.com/keys).
    - Ensure these keys are stored securely and used exclusively in test mode.
    - Clone the Developer Console UI repository and update the configuration files (e.g., `.env`) with your Stripe test keys to enable payment processing in a safe environment.
- **Dependency Installation**
    
    Navigate to your project directory and install all required dependencies:
    
    ```jsx
    npm install
    ```
    
    or
    
    ```jsx
    yarn install
    ```
    
- **Local Development**
    
    Start the application locally:
    
    ```jsx
    npm start
    ```
    
    or
    
    ```jsx
    yarn start
    ```
    
    Access the Developer Console at `http://localhost:3000` and verify the Top-Up functionality using test payment methods.
    

### **Backend: Customer Payment Service (CPS) Implementation**

- **Repository and Deployment**
    
    Clone follwing repository: 
    
    ```jsx
    git clone https://github.com/Cerebellum-Network/customer-payment-service
    ```
    
    Ensure the backend is containerized by providing a Docker image, enabling local testing and seamless deployment on Kubernetes clusters.
    
- **Key Backend Responsibilities**
    - Securely initiate and manage payment flows (SetupIntent and PaymentIntent) with the payment provider (e.g., Stripe).
    - Never store raw card data; instead, use payment method tokens or references provided by the payment gateway.
    - Handle webhook events for asynchronous payment status updates and reconciliation.
    - Support both one-time and recurring (auto top-up) payments using saved payment methods.

**Testing and Validation**

- Use the Developer Console UI to simulate top-up transactions with test cards.
- Ensure all payment flows are executed securely and that sensitive card data is never exposed to the frontend or stored on your servers.
- Validate that the backend correctly processes payments, updates DDC wallet balances, and manages payment method tokens for future transactions.

**Deployment**

- Provide clear documentation and Docker images to facilitate both local and production deployments.
- Ensure environment variables and configuration steps are well-documented for smooth integration and scaling.
