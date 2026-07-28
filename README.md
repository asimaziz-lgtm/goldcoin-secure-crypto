# GoldCoin Secure Crypto

Smart contract, formal-verification, security-audit, and companion-app
artifacts for the **Gold-Backed Secure Cryptocurrency (GBCC)** architecture —
an ERC-721-based system linking physical gold coins to blockchain tokens via
NFC/QR-hash binding, an escrow transfer protocol, and zk-SNARK-based
redemption. Companion materials for a manuscript under review at the
*International Journal of Information and Computer Security* (IJICS),
Inderscience.

## Contents

- **`contracts/GoldCoinToken.sol`** — the remediated core contract
  (Listing 1 in the manuscript). Reflects the fix for a reentrancy weakness
  identified by static analysis: coin metadata is now written before the
  external-call-bearing `_safeMint`, and a `ReentrancyGuard` modifier is
  applied to state-changing entry points.
- **`contracts/GoldCoinToken_PreRemediation.sol`** — the original,
  pre-remediation version of the contract, kept for reference against the
  Section 4.4 pre-remediation static/symbolic analysis run described in the
  manuscript.
- **`hardhat.config.js`** — Hardhat project config, targeting Polygon Amoy
  (chain ID 80002) plus Etherscan/Polygonscan verification settings.
- **`scripts/deployAmoy.js`** — deployment script for Polygon Amoy testnet.
  Writes deployment metadata to `deployment-amoy.json`.
- **`scripts/gasMeasurement.js`** — measures gas consumption for the
  contract's core functions against a deployed instance on Amoy.
- **`formal-verification/OwnershipShuffling.spthy`** — the Tamarin Prover
  formal model of the ownership-shuffling protocol (Algorithm 2 in the
  manuscript), plus `OwnershipShuffling_ProofTranscript.txt`, the recorded
  proof transcript.
- **`audit/`** — raw Slither and Mythril output from re-verifying the
  remediated contract, plus the exact commands and tool versions used. See
  `audit/README.md`. (The Section 4.4 pre-remediation reports discussed in
  the manuscript are not included here yet.)
- **`metadata/TokenMetadataStandard.json`** — the token metadata schema
  used for coin records.
- **`mobile/`** — companion mobile-app source (Kotlin): `BlockchainIntegration.kt`
  handles on-chain transfers and biometric-gated transaction confirmation;
  `VerificationProcess.kt` implements the NFC/QR cross-verification flow
  used to authenticate a physical coin against its on-chain token.

## Getting started

```bash
npm install
cp .env.example .env   # fill in AMOY_RPC_URL / PRIVATE_KEY / POLYGONSCAN_API_KEY
npx hardhat compile
npm run deploy:amoy
CONTRACT_ADDRESS=<deployed address> npm run gas:amoy
```

## License

MIT — see `LICENSE`.
