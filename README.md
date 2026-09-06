# Blockchain Voting System

Semester project for a blockchain course. This is a decentralized election app (dApp) where votes are recorded on a local Ethereum blockchain instead of a central database.

Each Ethereum account can vote once. Candidate names, vote counts, and who has already voted are stored in a Solidity smart contract, so results cannot be changed after the fact.

## Features

- Two candidates are created when the contract is deployed
- Voters pick a candidate and cast a vote from MetaMask
- Each address can vote only once
- Invalid candidate IDs are rejected
- The page updates when a new vote is recorded
- Truffle tests cover initialization, voting, double-voting, and invalid votes

## Tech stack

| Layer | Tool |
| --- | --- |
| Smart contract | Solidity `^0.5.16` |
| Framework | Truffle |
| Local blockchain | Ganache (port `7545`) |
| Wallet | MetaMask |
| Frontend | HTML, Bootstrap, jQuery, Web3.js, Truffle Contract |
| Dev server | lite-server |

## Project structure

```
Voting-System/
├── contracts/          # Solidity contracts
│   ├── Election.sol    # Voting logic
│   └── Migrations.sol
├── migrations/         # Deployment scripts
├── src/                # Web UI
│   ├── index.html
│   └── js/app.js
├── test/               # Truffle tests
│   └── election.js
├── truffle-config.js   # Network config (Ganache on 127.0.0.1:7545)
└── bs-config.json      # lite-server serves src/ and compiled contracts
```

## How the contract works

`Election.sol` stores:

- a `Candidate` struct (`id`, `name`, `voteCount`)
- a `voters` mapping so an address cannot vote twice
- a `candidates` mapping and `candidatesCount`

The constructor adds **Candidate 1** and **Candidate 2**. `vote(uint _candidateId)` checks that the sender has not voted and that the candidate exists, then increments that candidate’s count and emits `votedEvent`.

## Prerequisites

- [Node.js](https://nodejs.org/)
- [Truffle](https://trufflesuite.com/docs/truffle/) (`npm install -g truffle`)
- [Ganache](https://trufflesuite.com/ganache/) desktop app or CLI
- [MetaMask](https://metamask.io/) browser extension

## Setup and run

1. Install dependencies:

```bash
npm install
```

2. Start Ganache and keep it running on **HTTP://127.0.0.1:7545**.

3. Compile and deploy the contract to Ganache:

```bash
truffle migrate --reset
```

4. In MetaMask, add a Custom RPC network for Ganache (`http://127.0.0.1:7545`) and import one of Ganache’s accounts using its private key.

5. Start the frontend:

```bash
npm run dev
```

The app opens in the browser. Choose a candidate and click **Vote**. After you vote, the form hides for that account and the results table updates.

## Tests

```bash
truffle test
```

The tests check that:

- the contract starts with two candidates and zero votes
- a valid vote is recorded and the voter is marked as voted
- voting for an invalid candidate reverts
- the same account cannot vote twice

## Course notes

This project shows the usual dApp flow used in introductory blockchain courses:

1. Write business rules in a smart contract
2. Deploy to a personal Ethereum network
3. Talk to the contract from a web page with Web3
4. Use an account wallet so each voter has a unique address

It is meant for local development and demonstration, not a live public election.
