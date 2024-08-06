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
#### Multiple Deposits and Assignments:
The sender can do multiple deposits and assignments based on the liquidity balance.

## Sequence Diagram
```mermaid
sequenceDiagram
    participant Sender
    participant Verifier
    participant Custody
    participant Beneficiary
    participant dApp
    participant Circuit

    Sender ->> Custody: Deposit Tokens
    Custody ->> Custody: Update Liquidity Pool

    Sender ->> Custody: Encrypted CSV with Assignments (address, amount, expiration date)
    Custody ->> Custody: Register assigned balances to Beneficiaries

    Beneficiary ->> dApp: Inputs wallet address for proof generation
    dApp ->> Prover: Send input data
    Prover ->> Prover: Generate proof
    Prover ->> dApp: Send the proof
    dApp ->> Beneficiary: Provide the proof

    Beneficiary ->> dApp: Request balance query
    dApp ->> Custody: Request balance sending the proof
    Custody ->> Verifier: Request verification of the proof
    Verifier ->> Verifier: Checks validity of proof
    Verifier ->> Custody: Confirm verification result
    Custody ->> dApp: Send balance

    Beneficiary ->> dApp: Request a transfer to other wallet
    dApp ->> Custody: Request transfer sending the proof
    Custody ->> Verifier: Request verification of the proof
    Verifier ->> Verifier: Checks validity of proof
    Verifier ->> Custody: Confirm verification result
    Custody ->> Send balance
    Custody ->> OtherWallet: Transfer Tokens 
```
