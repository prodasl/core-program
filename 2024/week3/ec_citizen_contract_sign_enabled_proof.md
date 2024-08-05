# Project proposal for a ZKP circuit that validates if a Ecuadorian citizen is able to sign contract with the electronic cedula data
## Explanation of Validation Constraints
### Identity Validation (IDVerifier):
Validates the numero_identificacion using a circuit like the one provided earlier to ensure it meets the check digit algorithm.
### Age Verification (AgeVerifier):
Checks the fecha_nacimiento to ensure the individual is at least 18 years old.
### Nationality Verification (NationalityVerifier):
Confirms the nacionalidad field is "Ecuatoriana".
### Document Validity (DocumentVerifier):
Ensures the fecha_expiracion is a future date to confirm the cédula is still valid.
### Biometric Data Matching:
Verifies biometric data such as huella_digital_derecha and huella_digital_izquierda to ensure the individual is who they claim to be.

## Use Case Diagram
```mermaid
---
title: Use Case Diagram for ZKP-Based Cédula Validation
---
classDiagram
  class User {
    +submitPersonalInfo()
    +submitBiometricData()
    +receiveZKP()
    +submitZKPForContractSigning()
  }

  class IdentityValidator {
    +validateID()
    +checkNationality()
    +checkAge()
    +checkDocumentValidity()
  }

  class ZKPGenerator {
    +generateZKP()
  }

  class ContractValidator {
    +verifyZKP()
  }

  User --|> IdentityValidator : uses
  User --|> ZKPGenerator : uses
  User --|> ContractValidator : uses
```
## Sequence Diagram

```mermaid
sequenceDiagram
    participant User
    participant System
    participant Validator

    User ->> System: Submit Personal Info & Biometric Data
    System ->> System: Validate ID
    alt Valid ID
        System ->> System: Check Nationality
        System ->> System: Check Age
        System ->> System: Check Document Validity
        System ->> System: Generate ZKP
        System ->> User: Return ZKP
        User ->> Validator: Submit ZKP for Contract Signing
        Validator ->> System: Verify ZKP
        System ->> Validator: ZKP Verification Result
    else Invalid ID
        System ->> User: Return Error
    end
```
## Component Diagram
```mermaid
---
title: Component Diagram for ZKP-Based Cédula Validation
---
graph TD
    User -->|Submits Info| IdentityValidator
    IdentityValidator -->|Validates ID| IDVerifier
    IdentityValidator -->|Checks Age| AgeVerifier
    IdentityValidator -->|Checks Nationality| NationalityVerifier
    IdentityValidator -->|Checks Document Validity| DocumentVerifier
    IdentityValidator -->|Generates ZKP| ZKPGenerator
    User -->|Receives ZKP| ZKPGenerator
    User -->|Submits ZKP| ContractValidator
    ContractValidator -->|Verifies ZKP| ZKPVerifier

    subgraph System
        IdentityValidator
        IDVerifier
        AgeVerifier
        NationalityVerifier
        DocumentVerifier
        ZKPGenerator
        ContractValidator
        ZKPVerifier
    end
```
## Activity Diagram
```mermaid
---
title: Activity Diagram for ZKP-Based Cédula Validation
---
flowchart TD
    A[Start] --> B[Submit Personal Info & Biometric Data]
    B --> C[Validate ID]
    C -->|Valid| D[Check Nationality]
    D --> E[Check Age]
    E --> F[Check Document Validity]
    F -->|Valid| G[Generate ZKP]
    G --> H[Return ZKP to User]
    H --> I[Submit ZKP for Contract Signing]
    I --> J[Verify ZKP]
    J -->|Valid| K[Allow Contract Signing]
    J -->|Invalid| L[Reject Contract Signing]
    C -->|Invalid| M[Return Error]
    M --> Z[End]
    K --> Z
    L --> Z

```
