# Private token airdrop ZK tool project proposal.
This system is designed for private use by a project, foundation, company (known as sender) to send airdrops to a selected group of beneficiaries ensuring the privacy of the beneficiaries about how much and the current balance of their airdroped tokens. The system will be restricted to recieve funds and set the assignments by one sender only (the sender is set in the initial setup of system).

## Summary
This set of diagrams represents a ZK airdrop system with the following features:

### Verifier Smart Contract:

Receives encrypted lists of assignments.
Checks if the total incoming assignments amount are available.
Locks the corresponding amount of approved assignments.
Registers the approved assignments.
Approves or denies encrypted transfer requests based on the "proof of available balance".
Sends approved transactions to the custody contract.

### Beneficiary App:

Generates ZKP known as "proof of available balance".
Sends encrypted transfer request transactions to the verifier.
Shows the result of requested transactions (approved or denied).

### Custody Smart Contract:

Receives funds from the sender.
Executes the encrypted approved transfers received from the verifier.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#ffcc00', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#ffcc00'}}}%%
---
title: Use Case Diagram for ZK Airdrop System
---
classDiagram
  class VerifierSmartContract {
    +receiveEncryptedList()
    +approveOrDenyAssignment()
    +lockAmount()
    +registerAssignment()
    +approveOrDenyTransferRequest()
    +sendApprovedTransactionToCustody()
  }

  class BeneficiaryApp {
    +generateZKP()
    +sendEncryptedTransferRequest()
    +showTransactionResult()
  }

  class CustodySmartContract {
    +receiveFunds()
    +executeApprovedTransfers()
  }

  Sender --|> CustodySmartContract : deposits funds
  Sender --|> VerifierSmartContract : sends encrypted list
  Beneficiary --|> BeneficiaryApp : uses
  BeneficiaryApp --|> VerifierSmartContract : sends encrypted transfer request
  VerifierSmartContract --|> CustodySmartContract : sends approved transaction
```

```mermaid
sequenceDiagram
    participant Sender
    participant CustodySmartContract
    participant VerifierSmartContract
    participant BeneficiaryApp
    participant Beneficiary
    participant ThirdPartyWallet

    Sender ->> CustodySmartContract: Deposit Funds
    CustodySmartContract ->> CustodySmartContract: Update Balance

    Sender ->> VerifierSmartContract: Send Encrypted List
    VerifierSmartContract ->> VerifierSmartContract: Check Total Amount
    alt Amount Available
        VerifierSmartContract ->> VerifierSmartContract: Lock Amount
        VerifierSmartContract ->> VerifierSmartContract: Register Assignments
    else Amount Not Available
        VerifierSmartContract ->> Sender: Reject Transaction
    end

    Beneficiary ->> BeneficiaryApp: Request Transfer
    BeneficiaryApp ->> BeneficiaryApp: Generate ZKP
    BeneficiaryApp ->> VerifierSmartContract: Send Encrypted Transfer Request
    VerifierSmartContract ->> VerifierSmartContract: Verify ZKP
    alt ZKP Valid
        VerifierSmartContract ->> VerifierSmartContract: Approve Transfer
        VerifierSmartContract ->> CustodySmartContract: Send Approved Transaction
        CustodySmartContract ->> ThirdPartyWallet: Execute Transfer
        CustodySmartContract ->> BeneficiaryApp: Notify Approval
    else ZKP Invalid
        VerifierSmartContract ->> BeneficiaryApp: Notify Denial
    end
```

```mermaid
---
title: Component Diagram for ZK Airdrop System
---
graph TD
    Sender -->|Deposits Funds| CustodySmartContract
    Sender -->|Sends Encrypted List| VerifierSmartContract
    VerifierSmartContract -->|Sends Approved Transaction| CustodySmartContract
    Beneficiary -->|Uses| BeneficiaryApp
    BeneficiaryApp -->|Sends Encrypted Transfer Request| VerifierSmartContract
    CustodySmartContract -->|Executes Transfers| ThirdPartyWallet

    subgraph VerifierSmartContract
        direction TB
        VerifyAmount[Check Total Amount]
        LockAmount[Lock Amount]
        RegisterAssignments[Register Assignments]
        VerifyZKP[Verify ZKP]
        ApproveTransfer[Approve Transfer]
        DenyTransfer[Deny Transfer]
    end

    subgraph BeneficiaryApp
        GenerateZKP[Generate ZKP]
        SendRequest[Send Encrypted Transfer Request]
        ShowResult[Show Transaction Result]
    end

    subgraph CustodySmartContract
        ReceiveFunds[Receive Funds]
        UpdateBalance[Update Balance]
        ExecuteTransfers[Execute Approved Transfers]
    end
```
```mermaid
---
title: Activity Diagram for ZK Airdrop System
---
flowchart TD
    A[Start] --> B[Sender Deposits Funds]
    B --> C[Update Custody Balance]
    C --> D[Sender Sends Encrypted List]
    D --> E[Verifier Checks Total Amount]
    E -->|Available| F[Lock Amount]
    F --> G[Register Assignments]
    E -->|Not Available| H[Reject Transaction]
    G --> I[Beneficiary Requests Transfer]
    I --> J[Generate ZKP]
    J --> K[Send Encrypted Transfer Request]
    K --> L[Verifier Verifies ZKP]
    L -->|Valid| M[Approve Transfer]
    M --> N[Send Approved Transaction to Custody]
    N --> O[Execute Transfer to Third Party Wallet]
    L -->|Invalid| P[Deny Transfer]
    M --> Q[Notify Approval]
    P --> R[Notify Denial]
    O --> S[Notify Approval]
    S --> Z[End]
    Q --> Z
    R --> Z
```
