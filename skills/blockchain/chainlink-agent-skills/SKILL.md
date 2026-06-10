---
name: chainlink-agent-skills
description: "Use when working with Chainlink oracle networks, CCIP cross-chain messaging, or smart contract data feeds. Official Chainlink agent skills on the agentskills.io spec."
version: 1.0.0
author: Chainlink (smartcontractkit)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [chainlink, oracle, ccip, smart-contracts, web3, defi, blockchain, cross-chain, data-feeds]
    homepage: https://github.com/smartcontractkit/chainlink-agent-skills
    related_skills: [hermes-blockchain-oracle]
---

# Chainlink Agent Skills

## Overview

Official Chainlink agent skills on the [agentskills.io](https://agentskills.io) spec, maintained by Chainlink / Smart Contract Kit. Covers oracle network data feeds, CCIP (Cross-Chain Interoperability Protocol) cross-chain messaging, and smart contract interaction — the foundational Web3 infrastructure layer used by most major DeFi protocols.

## When to Use

- Fetching real-time price feeds (ETH/USD, BTC/USD, 500+ pairs) from Chainlink Data Feeds
- Initiating or monitoring cross-chain token transfers and messages via CCIP
- Querying Chainlink Functions for off-chain computation results
- Verifying Chainlink VRF (Verifiable Random Function) outputs
- Building or debugging smart contracts that consume Chainlink oracles
- Don't use for: direct wallet transactions (use a wallet skill), on-chain analytics (use hermes-blockchain-oracle)

## Installation

```bash
hermes skills install smartcontractkit/chainlink-agent-skills
```

Or via direct URL:

```bash
hermes skills install https://raw.githubusercontent.com/smartcontractkit/chainlink-agent-skills/main/SKILL.md
```

## Skill Areas

### Data Feeds

Real-time and historical price data from Chainlink's decentralized oracle network:

```
"What is the current ETH/USD price from Chainlink?"
"Get the BTC/USD price feed on Arbitrum"
"Show me the last 10 rounds for the LINK/ETH feed on Mainnet"
```

Supported networks: Ethereum Mainnet, Arbitrum, Optimism, Polygon, Avalanche, BNB Chain, Base, and more.

### CCIP (Cross-Chain Interoperability Protocol)

```
"Send 100 USDC from Ethereum to Arbitrum via CCIP"
"Check the status of CCIP message 0xabc..."
"What are the supported CCIP lanes from Ethereum Mainnet?"
"Estimate the CCIP fee for a cross-chain transfer"
```

### Chainlink Functions

Off-chain computation with on-chain verification:

```
"Call a Chainlink Function to fetch weather data and store on-chain"
"Check the result of Functions request ID 0x123..."
```

### VRF (Verifiable Random Function)

```
"Request a verifiable random number for my NFT mint contract"
"Check the status of VRF request 0xdef..."
```

## Configuration

Set your RPC endpoints for the networks you want to interact with:

```bash
export ETH_MAINNET_RPC=https://mainnet.infura.io/v3/YOUR_KEY
export ARBITRUM_RPC=https://arb1.arbitrum.io/rpc
export POLYGON_RPC=https://polygon-rpc.com
```

For CCIP and Functions, you'll also need a funded wallet:

```bash
export PRIVATE_KEY=0x...   # Never commit this — use .env
```

## Common Pitfalls

1. **RPC rate limits.** Free RPC endpoints (Infura, Alchemy free tier) have rate limits. Use a paid tier or self-hosted node for production workloads.
2. **CCIP lane not supported.** Not all chain pairs have active CCIP lanes. Check [docs.chain.link/ccip/supported-networks](https://docs.chain.link/ccip/supported-networks) for the current lane matrix.
3. **Stale price feed.** Chainlink feeds have a heartbeat (typically 1h or 24h). If `updatedAt` is older than the heartbeat, the feed may be paused — handle this case in production.
4. **Gas estimation.** CCIP transfers require gas on both the source and destination chain. Ensure wallet has native tokens on both chains.
5. **Private key in env.** Never hardcode `PRIVATE_KEY` in code. Use `.env` with `.gitignore` or a secrets manager.

## Verification Checklist

- [ ] Skill installed: `hermes skills list | grep chainlink`
- [ ] RPC endpoint set and accessible: test with a simple `eth_blockNumber` call
- [ ] Data feed query returns current price: *"Get ETH/USD price from Chainlink on Mainnet"*
- [ ] CCIP lane available for intended chain pair (if using cross-chain)
- [ ] Wallet funded with native tokens on source chain (if using CCIP/Functions)
