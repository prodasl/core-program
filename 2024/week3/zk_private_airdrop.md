# Private token airdrop ZK tool project proposal.
This system is designed for private use by a project, foundation, company (known as sender) to send airdrops to a selected group of beneficiaries ensuring the privacy of the beneficiaries about how much and the current balance of their airdroped tokens. The system will be restricted to recieve funds and set the assignments by one sender only (the sender is set in the initial setup of system).

## Summary of Features
#### Verifier Smart Contract (Verifier):
Receives encrypted lists of assignments.
Checks if the total incoming assignments amount is available.
Locks the corresponding amount if approved.
Registers the approved incoming assignments.
Approves or denies transfer requests based on "proof of available balance."
Sends approved transactions to the custody contract.

#### Beneficiary App (Prover):
Generates a ZKP for "proof of available balance."
Sends encrypted transfer request transactions with the proof.
Shows the result of requested transactions.

#### Custody Smart Contract (Custody):
Receives funds from the sender.
Executes encrypted approved transfers received from the verifier.

#### Sender dApp:
Gets the list of assignments.
Sends the encrypted list to the verifier.


## System design
```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#ffcc00', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#ffcc00'}}}%%
---
title: Use Case Diagram for ZK Airdrop System
---
classDiagram
  class Sender {
    +getListOfAssignments()
    +sendEncryptedListToVerifier()
  }

  class Beneficiary {
    +generateProofOfAvailableBalance()
    +sendTransferRequest()
    +showTransactionResult()
  }

  class Verifier {
    +receiveEncryptedList()
    +approveOrDenyTransferRequests()
    +sendApprovedTransactionsToCustody()
  }

  class Custody {
    +receiveFunds()
    +executeApprovedTransfers()
  }

  Sender --|> Verifier : sends encrypted list
  Beneficiary --|> Verifier : sends transfer request
  Verifier --|> Custody : sends approved transactions
  Sender --|> Custody : sends funds
  Beneficiary --> Verifier : generateProofOfAvailableBalance
```

```mermaid
sequenceDiagram
    participant Sender
    participant Verifier
    participant Custody
    participant Beneficiary

    Sender ->> Custody: Send Funds
    Custody ->> Custody: Update Balance
    Custody ->> Custody: Execute Approved Transfers

    Sender ->> Verifier: Send Encrypted List
    Verifier ->> Verifier: Check Assignments
    alt Assignments Valid
        Verifier ->> Verifier: Lock Amounts
        Verifier ->> Verifier: Register Assignments
    else Assignments Invalid
        Verifier ->> Sender: Reject Transaction
    end

    Beneficiary ->> Verifier: Send Transfer Request with Proof
    Verifier ->> Verifier: Verify Proof and Check Balance
    alt Proof Valid and Balance Available
        Verifier ->> Custody: Send Approved Transaction
        Verifier ->> Beneficiary: Approve Transaction
    else Proof Invalid or Insufficient Balance
        Verifier ->> Beneficiary: Deny Transaction
    end

```

```mermaid
---
title: Component Diagram for ZK Airdrop System
---
graph TD
    Sender -->|Get List of Assignments| AssignmentsModule
    AssignmentsModule -->|Encrypt List| EncryptionModule
    Sender -->|Send Encrypted List| Verifier

    Beneficiary -->|Generate Proof| ZKProofModule
    Beneficiary -->|Send Transfer Request| Verifier

    Verifier -->|Receive Encrypted List| DecryptionModule
    Verifier -->|Check Assignments| AssignmentsCheckModule
    Verifier -->|Lock Amounts| LockingModule
    Verifier -->|Register Assignments| RegistryModule
    Verifier -->|Verify Proof| ProofVerificationModule
    Verifier -->|Send Approved Transactions| Custody

    Custody -->|Receive Funds| FundModule
    Custody -->|Execute Transfers| TransferExecutionModule

    subgraph Verifier
        DecryptionModule
        AssignmentsCheckModule
        LockingModule
        RegistryModule
        ProofVerificationModule
    end

    subgraph Custody
        FundModule
        TransferExecutionModule
    end

    subgraph Sender
        AssignmentsModule
        EncryptionModule
    end

    subgraph Beneficiary
        ZKProofModule
    end
```
```mermaid
---
title: Activity Diagram for ZK Airdrop System
---
flowchart TD
    A[Start] --> B[Sender Gets List of Assignments]
    B --> C[Encrypt List]
    C --> D[Send Encrypted List to Verifier]
    D --> E[Verifier Receives Encrypted List]
    E --> F[Check Total Assignment Amount]
    F -->|Valid| G[Lock Amounts]
    G --> H[Register Assignments]
    F -->|Invalid| I[Reject Transaction]
    H --> J[End]

    subgraph Beneficiary
        K[Generate Proof of Available Balance] --> L[Send Transfer Request with Proof]
        L --> M[Verifier Receives Transfer Request]
        M --> N[Verify Proof]
        N --> O[Check Balance]
        O -->|Valid and Available| P[Approve Transaction]
        O -->|Invalid or Insufficient| Q[Deny Transaction]
        P --> R[Send Approved Transaction to Custody]
        Q --> S[End]
        R --> T[Execute Transfer]
        T --> S
    end
```
