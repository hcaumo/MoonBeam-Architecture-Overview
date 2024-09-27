Drexfy Platform Overview
Architecture Overview

Drexfy is a blockchain-based platform that simplifies the process of investing in securitized SME credits while ensuring compliance and security through robust KYC/AML procedures. The platform leverages various AWS services and blockchain technology to provide a seamless and secure user experience.
Registration Process

The registration process ensures that all users undergo thorough KYC and AML checks to maintain compliance and security.
Registration Workflow:
![register_process](https://github.com/user-attachments/assets/72eab113-3e25-4ed4-b2f1-9c042c3ea4ac)

    User Registration:
        Users begin the registration process via the Bubble.io frontend.

    KYC/AML Check:
        User details are sent to a third-party service for KYC and AML checks.

    Wallet Generation:
        Upon successful KYC/AML verification, the Wallet Generator Lambda function is triggered via API Gateway.
        A new wallet is generated, and the private key is encrypted using the Encrypt PrivateKey Lambda function and AWS KMS.
        The encrypted private key is securely stored in AWS DynamoDB.

    NFT Classification:

    After the Wallet Generator Lambda creates a new wallet, the NFT Classification Lambda is activated.
    NFT Issuance:
    The Lambda function issues an NFT to the user's wallet based on their classification, which could be one of the following categories:
        US Accredited Investor
        US Non-Accredited Investor
        European Accredited Investor
        European Non-Accredited Investor
        Sanctioned

    Smart Contract Interaction:
    The classification details and NFT issuance are recorded in the InvestorClassification smart contract within the Moonbeam Network for regulatory compliance and validation.

This process ensures all users are thoroughly vetted before participating in the platform, maintaining compliance and security.
Buy Investment Process

Drexfy offers a streamlined experience for investors to purchase securitized SME credits, leveraging blockchain technology and AWS services for secure and efficient transactions.
Buy Investment Workflow:
![buy_investment_process](https://github.com/user-attachments/assets/841f80fd-f301-4085-8607-971b382e36ac)

    User Initiation:
        Users initiate the investment purchase through the Bubble.io frontend and access the payment gateway checkout.
        The user's wallet address is retrieved from the Bubble.io database.

    Payment Gateway Interaction:
        The payment gateway processes the payment and sends a webhook to the AWS API Gateway, triggering the Deposit Lambda function.

    Deposit Handling:
        The Deposit Lambda function decrypts the user's private key using the Decrypt PrivateKey Lambda function.
        It triggers the Transfer Stablecoin Lambda function to manage stablecoin transfer.

    Stablecoin Transfer:
        Stablecoins are transferred from the user's wallet to the securitization company's wallet.
        The Transfer Stablecoin function then triggers the Transfer Financial Instrument Lambda function.

    Financial Instrument Transfer:
        Financial instruments are transferred from the securitization company's wallet to the user's wallet, completing the investment purchase.

Repayment Process

The repayment process handles monthly repayment transactions, updating financial instruments and managing smart contracts on the blockchain.
Repayment Workflow:
![stablecoin_repayment](https://github.com/user-attachments/assets/e9c165e3-9b77-4a47-bd7c-12539a7863d1)

    Admin Triggers Repayment:
        Admin initiates the repayment process through the API Gateway, retrieving wallet information from the Bubble.io database.

    Transfer Stablecoin Lambda:
        Transfers stablecoins to the user after decrypting the private key using the Decrypt PrivateKey Lambda function.
        Executes the stablecoin transaction on the Moonbeam RPC, updating the Stablecoin Smart Contract.

    Transfer Financial Instrument Lambda:
        Transfers the financial instrument back to the securitization entity.
        Executes the transfer via the Moonbeam RPC, updating the Financial Instrument Smart Contract.

    Burn Stablecoin Lambda:
        Burns the transferred stablecoin, removing it from circulation.
        The burn operation is carried out on the Moonbeam RPC, reflecting the change in the Stablecoin Smart Contract.

    Burn Financial Instrument Lambda:
        Burns the financial instrument, finalizing the repayment process.
        The burn operation is executed on the Moonbeam RPC, updating the Financial Instrument Smart Contract.

    Paymaster Lambda:
        Triggers the Particle Network Paymaster to sponsor gas fees for transactions, ensuring the platform covers blockchain interaction costs for users.

Compliance Process

Drexfy ensures compliance with AML regulations, taking immediate action if a user fails AML checks.
WebHook Notification Workflow:
![webhook_notification_process](https://github.com/user-attachments/assets/89cfa1e3-3d54-4e9b-9d1d-a9a02622bae2)

    AML Rejection Notification:
        The third-party service monitors users for AML compliance.
        If a user is flagged, the service sends a WebHook notification to the AML Checker Lambda function.

    User Blocking:
        The AML Checker Lambda function notifies the Bubble.io frontend.
        The user's account is flagged and blocked from making purchases or withdrawals until the issue is resolved.
        Change Investor Classification NFT metadata and On-Chain classification.

Contact & Support

For more information, please contact our team at info@drexfy.com
