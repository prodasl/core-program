# Private token airdrop ZK tool project proposal.

## Summary of System Features
#### Depositing Tokens:
The sender can deposit a certain number of tokens to the custody smart contract’s liquidity pool.
#### Assigning Tokens:
The sender can use a CSV list with assigned values (address, amount, expiration date) to assign an available balance on the custody smart contract to the beneficiaries detailed in the CSV file.
#### Generating ZKP:
A beneficiary can get a ZKP that proves they are the owner of a specific wallet address.
#### Checking Balance:
Beneficiaries can consult their balance using their ZKP.
#### Transferring Tokens:
A beneficiary can use their ZKP to transfer tokens to other wallets from their available balance on the custody smart contract without revealing their own wallet address.
#### Withdrawing Unclaimed Tokens:
The unclaimed tokens until the expiration date specified for each assignment can be withdrawn by the custody smart contract only.
#### Multiple Deposits and Assignments:
The sender can do multiple deposits and assignments based on the liquidity balance.

## Use Case Diagram
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#ffcc00', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#ffcc00'}}}%%
---
title: Use Case Diagram for Privacy-Preserving Airdrop System
---
classDiagram
  class Sender {
    +depositTokens()
    +assignTokens()
    +multipleDeposits()
  }

  class Beneficiary {
    +generateZKP()
    +transferTokens()
    +consultBalance()
  }

  class CustodySmartContract {
    +receiveDeposits()
    +assignBalances()
    +verifyZKP()
    +transferTokens()
    +withdrawUnclaimedTokens()
  }

  Sender --|> CustodySmartContract : uses
  Beneficiary --|> CustodySmartContract : uses
```
## Sequence Diagram
```mermaid
sequenceDiagram
    participant Sender
    participant CustodySmartContract
    participant Beneficiary

    Sender ->> CustodySmartContract: Deposit Tokens
    CustodySmartContract ->> CustodySmartContract: Update Liquidity Pool

    Beneficiary ->> CustodySmartContract: Generate ZKP for Wallet Address
    CustodySmartContract ->> Beneficiary: Provide ZKP

    Sender ->> CustodySmartContract: Encrypted CSV with Assignments (address, amount, expiration date)
    CustodySmartContract ->> CustodySmartContract: Assign Balances to Beneficiaries

    Beneficiary ->> CustodySmartContract: Transfer Tokens using ZKP
    CustodySmartContract ->> CustodySmartContract: Verify ZKP
    CustodySmartContract ->> OtherWallet: Transfer Tokens

    Beneficiary ->> CustodySmartContract: Consult Balance using ZKP
    CustodySmartContract ->> CustodySmartContract: Verify ZKP
    CustodySmartContract ->> Beneficiary: Return Balance

    CustodySmartContract ->> CustodySmartContract: Withdraw Unclaimed Tokens (after expiration date)
```

## Component Diagram
```mermaid
---
title: Component Diagram for Privacy-Preserving Airdrop System
---
graph TD
    Sender -->|Deposits Tokens| CustodySmartContract
    CustodySmartContract -->|Updates Liquidity Pool| LiquidityPool
    Beneficiary -->|Generates ZKP| ZKPCircuit
    ZKPCircuit --> CustodySmartContract
    Sender -->|Sends Encrypted CSV| CSVProcessor
    CSVProcessor --> CustodySmartContract
    Beneficiary -->|Transfers Tokens using ZKP| ZKPTransfer
    ZKPTransfer --> CustodySmartContract
    Beneficiary -->|Consults Balance using ZKP| ZKPBalanceCheck
    ZKPBalanceCheck --> CustodySmartContract
    CustodySmartContract -->|Withdraws Unclaimed Tokens| LiquidityPool

    subgraph CustodySmartContract
        LiquidityPool
        ZKPCircuit
        CSVProcessor
        ZKPTransfer
        ZKPBalanceCheck
    end
```
## Activity Diagram
```mermaid
---
title: Activity Diagram for Privacy-Preserving Airdrop System
---
flowchart TD
    A[Start] --> B[Sender Deposits Tokens]
    B --> C[Update Liquidity Pool]
    C --> D[Beneficiary Generates ZKP]
    D --> E[Provide ZKP to Beneficiary]
    E --> F[Sender Sends Encrypted CSV]
    F --> G[Assign Balances to Beneficiaries]
    G --> H[Beneficiary Transfers Tokens using ZKP]
    H --> I[Verify ZKP]
    I --> J[Transfer Tokens to Other Wallet]
    J --> K[Beneficiary Consults Balance using ZKP]
    K --> L[Verify ZKP]
    L --> M[Return Balance]
    M --> N[Check for Expiration Date]
    N -->|Expired| O[Withdraw Unclaimed Tokens]
    O --> P[Update Liquidity Pool]
    N -->|Not Expired| Q[Wait for Expiration Date]
    P --> Z[End]
    Q --> N
```
