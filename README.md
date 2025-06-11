# Developer Console “Top-Up” RFP
Build a Top-Up micro-service that automatically keeps Cere developer accounts funded with a stored credit card, preventing service interruptions from low balances. Your solution will be used daily across the ecosystem, making life easier for developers worldwide 🤝

Components:
- Frontend: Developer Console UI Integration
- Backend: Customer Payment Service (CPS) Implementation

Budget & timing:
-  Budget: $1.000 USDC 
-  Estimated development time core functionality: 2 days

**🚨 Reminder:** before diving into your proposal, make sure to carefully review the [**Contribution Guidelines**](..%2F..%2FREADME.md). ✅

## Data Model / Endpoints

![DDC Topup Data Model](https://raw.githubusercontent.com/bren-cere/TESTING/main/assets/ddc_topup_datamodel.png)

## User Stories

- **User Successfully Tops Up DDC Wallet via Credit/Debit Card**
  - User logs in, enters amount and card details, payment is processed, DDC balance increases, confirmation displayed, transaction recorded.

- **Payment Security and Compliance**
  - Card fields rendered via secure, PCI DSS-compliant IFRAME; raw card data never stored.

- **Handling Failed or Declined Payments**
  - Clear error messages for failed payments, no funds deducted, failed transactions logged.

- **Payment Method Management**
  - Users can add, remove, or update payment methods securely; changes are immediate and confirmed.

## Testing & Deployment

**Testing and Validation**

- Use the Developer Console UI to simulate top-up transactions with test cards.
- Ensure all payment flows are executed securely and that sensitive card data is never exposed to the frontend or stored on your servers.
- Validate that the backend correctly processes payments, updates DDC wallet balances, and manages payment method tokens for future transactions.

**Deployment**

- Provide clear documentation and Docker images to facilitate both local and production deployments.
- Ensure environment variables and configuration steps are well-documented for smooth integration and scaling.
  
## Quickstart
To integrate the Top-Up component into the Developer Console UI, follow these structured steps for both the frontend and backend implementations: [Developer Console Top-Up Documentation](https://github.com/Cerebellum-Network/cluster-apps/blob/dev/apps/developer-console/developer_console_topup.md#-contribution-guide)
