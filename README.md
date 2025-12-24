# EncryptedMathDuel

Encrypted Math Duel is a confidential on-chain game where two players submit encrypted guesses to a hidden target number. 
The contract computes which player is closer using homomorphic operations, without ever revealing any of the numbers. 
Only an encrypted winner code (tie / player 1 / player 2) is exposed and later publicly decryptable. 
This showcases how game logic and outcomes can remain private while still being verifiable on-chain.

---

## Contract
- **Contract name:** `EncryptedMathDuel`
- **Network:** Sepolia
- **Contract address:** `0xc0e38DD2fa617b8E189EB88BD80ACebE0588143C` 
- **Relayer SDK:** `@zama-fhe/relayer-sdk` (v0.3.x required)

---

## Features


Encrypted correct answer and player guesses using FHEVM.

Fully homomorphic distance comparison to determine the closest guess.

Winner encoded as an encrypted value (0, 1, 2) with public decryption via relayer.

Simple frontend flow: create duel, submit guesses, compute winner, make public & decrypt.

---

Modern glassmorphic UI built with pure HTML + CSS.  
Powered by Zama Relayer SDK v0.3.0-5 and Ethers.js v6.15.


Zero knowledge of inputs — full privacy preserved

Modern dual-column glassmorphic UI built with pure HTML + CSS

Powered by Zama Relayer SDK v0.3.0 and Ethers.js v6

🛠 Quick Start
Prerequisites

Node.js ≥ 20

npm / yarn / pnpm

MetaMask or any injected Ethereum-compatible wallet

## Installation (development)
1. Clone repo  
```bash
git clone <repo-url>
cd health-metric-zone
Install dependencies (example)

npm install
# or
yarn install

Install Zama Relayer SDK on frontend

npm install @zama-fhe/relayer-sdk @fhevm/solidity ethers

Build & deploy (Hardhat)

npx hardhat clean
npx hardhat compile
npx hardhat deploy --network sepolia
Make sure your hardhat.config.js includes the Zama config and the Solidity version ^0.8.27.


Make sure `hardhat.config.js` has the fhEVM config and Solidity `^0.8.27`.

---

## Frontend (Quickstart)

## 🧮 Encrypted Math Duel – Frontend (Quickstart)

1. Set `CONFIG.CONTRACT_ADDRESS` in `frontend/index.html` to the deployed `EncryptedMathDuel` address.  
2. Serve the frontend (`npx serve frontend` or any static server).

**Important:**  
- Each `duelKey` (e.g. `"duel-1"`) is mapped to a `duelId = keccak256(duelKey)`.  
- A duel **can be created only once** per `duelId`.  
- Re‑calling `submitCorrect` with the same `duelKey` will revert with `duel already exists`.  
- To start a new duel, use a different `duelKey` (e.g. `"duel-2"`, `"game-1"`, etc.).

**Host (correct answer):**  
- Encrypt correct answer via Relayer using `add16(correctAnswer)` and call:  
  `submitCorrect(bytes32 duelId, bytes32 encCorrect, bytes attestation)`  
- `duelId` is derived on the frontend as `keccak256(utf8(duelKey))`.

**Players (guesses):**  
- Each player encrypts their guess via `add16(guess)` and calls:  
  `submitGuess1(bytes32 duelId, bytes32 encGuess, bytes attestation)` or  
  `submitGuess2(bytes32 duelId, bytes32 encGuess, bytes attestation)`.

**Reveal winner:**  
- Encrypt zero once via `add16(0)` and call:  
  `computeWinner(bytes32 duelId, bytes32 encZero, bytes attestation)`  
- Read `winnerHandle(bytes32 duelId)` to get the ciphertext handle.  
- Call `makeWinnerPublic(bytes32 duelId)`.  
- Decrypt the winner on the frontend:


const out = await relayer.publicDecrypt([handle]);
const v = out.clearValues[handle] ?? out.clearValues[handle.toLowerCase()];
const winnerCode = Number(v); // 0=tie, 1=player1, 2=player2;

---


# Security & Privacy
The contract never stores plain health data.
FHE.allow and FHE.allowThis are used so only authorized parties (owner + contract) can decrypt.
Users must protect their wallets and local attestation proofs — if lost, privacy is still preserved (attestations are on inputs).

# Common Commands:

Compile: npx hardhat compile
Deploy: npx hardhat deploy --network sepolia
Serve frontend: npx serve frontend or any static server

Troubleshooting
If publicDecrypt returns undefined: ensure you passed a clean bytes32 handle and that the contract used FHE.makePubliclyDecryptable(...).
If Relayer worker fails in browser: ensure server sends Cross-Origin-Opener-Policy: same-origin and Cross-Origin-Embedder-Policy: require-corp headers.

## 📁 Project Structure
tinderdao-private-match/
├── contracts/
│   └── EncryptedMathDuel.sol                # Main FHE-enabled matchmaking contract
├── deploy/                                  # Deployment scripts
├── frontend/                                # Web UI (FHE Relayer integration)
│   └── index.html
├── hardhat.config.js                        # Hardhat + FHEVM config
└── package.json                             # Dependencies and npm scripts

📜 Available Scripts
Command	Description
npm run compile	Compile all smart contracts
npm run test	Run unit tests
npm run clean	Clean build artifacts
npm run start	Launch frontend locally
npx hardhat deploy --network sepolia	Deploy to FHEVM Sepolia testnet
npx hardhat verify	Verify contract on Etherscan
🔗 Frontend Integration

The frontend (pure HTML + vanilla JS) uses:

@zama-fhe/relayer-sdk v0.3.0

ethers.js v6.13

Web3 wallet (MetaMask) connection

Workflow:

Connect wallet

Encrypt & Submit a preference query (desired criteria)

Compute match handle via computeMatchHandle()

Make public the result using makeMatchPublic()

Publicly decrypt → get final result (MATCH ✅ / NO MATCH ❌)

🧩 FHEVM Highlights

Encrypted types: euint8, euint16

Homomorphic operations: FHE.eq, FHE.and, FHE.or, FHE.gt, FHE.lt

Secure access control using FHE.allow & FHE.allowThis

Public decryption enabled with FHE.makePubliclyDecryptable

Frontend encryption/decryption handled via Relayer SDK proofs

📚 Documentation

Zama FHEVM Overview

Relayer SDK Guide

Solidity Library: FHE.sol

Ethers.js v6 Documentation

🆘 Support

🐛 GitHub Issues: Report bugs or feature requests

💬 Zama Discord: discord.gg/zama-ai
 — community help

📄 License

BSD-3-Clause-Clear License
See the LICENSE
 file for full details.