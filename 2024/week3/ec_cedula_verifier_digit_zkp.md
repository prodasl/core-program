# Ecuadorian cedula verfier digit ZKP project proposal
## Explanation of Validation Constraints
### Proof Generation (ProofGenerator):
The user submits the first 9 digits of their ID.
The system calculates the 10th digit based on the verifier algorithm.
The system generates a zero-knowledge proof that the 10th digit is correct without revealing the actual ID number.
### Proof Verification (ProofVerifier):
The verifier receives the zero-knowledge proof from the user.
The system verifies the proof to ensure the 10th digit is correct according to the verifier algorithm without knowing the actual ID number.
## Use case diagram
```mermaid
---
title: Use Case Diagram for 10th Digit Verifier using ZKP
---
classDiagram
  class User {
    +submitIDDigits()
    +receiveProof()
    +submitProofForVerification()
  }

  class System {
    +generateProof()
    +verifyProof()
  }

  User --|> System : uses

```
## Sequence Diagram
```mermaid
sequenceDiagram
    participant User
    participant System
    participant Verifier

    User ->> System: Submit First 9 Digits of ID
    System ->> System: Calculate 10th Digit
    System ->> System: Generate ZKP Proof
    System ->> User: Return ZKP Proof
    User ->> Verifier: Submit ZKP Proof
    Verifier ->> System: Verify ZKP Proof
    System ->> Verifier: Proof Verification Result

```
### Component Diagram
```mermaid
---
title: Component Diagram for 10th Digit Verifier using ZKP
---
graph TD
    User -->|Submits First 9 Digits of ID| ProofGenerator
    ProofGenerator -->|Generates Proof| ZKPCircuit
    User -->|Receives Proof| ProofGenerator
    User -->|Submits Proof| ProofVerifier
    ProofVerifier -->|Verifies Proof| ZKPCircuit

    subgraph System
        ProofGenerator
        ProofVerifier
        ZKPCircuit
    end
```
### Activity diagram
```mermaid
---
title: Activity Diagram for 10th Digit Verifier using ZKP
---
flowchart TD
    A[Start] --> B[Submit First 9 Digits of ID]
    B --> C[Calculate 10th Digit]
    C --> D[Generate ZKP Proof]
    D --> E[Return ZKP Proof to User]
    E --> F[Submit ZKP Proof for Verification]
    F --> G[Verify ZKP Proof]
    G -->|Valid| H[Proof Valid]
    G -->|Invalid| I[Proof Invalid]
    H --> Z[End]
    I --> Z[End]
```
