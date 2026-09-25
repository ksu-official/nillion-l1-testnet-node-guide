# nillion-l1-testnet-node-guide
Official website Nillion: https://www.nillion.com


Step-by-step guide to setting up and running a node on Nillion's new
**L1 testnet** (Sepolia) — part of **Dusk**, the first phase of
Nillion's Covenants roadmap centered on Encrypted Markets.

> This is a **separate L1 testnet**, not an update to Blacklight L2
> mainnet nodes.

> Unofficial community guide, not affiliated with the Nillion team.

## What is Nillion?

Nillion is a decentralized network for privacy-preserving computation
— a "blind computer" that can process encrypted data (via MPC and
related cryptographic techniques) without ever revealing it, to the
node operators or anyone else.

## Contents

- 📖 [Full guide](GUIDE.md)
- 🐳 [docker-compose setup](docker-compose.yml)
- ⚙️ [.env example](.env.example)

## Repository structure

```
nillion-l1-testnet-node-guide/
├── README.md             — this file
├── GUIDE.md              — full step-by-step guide
├── docker-compose.yml    — ready-to-use node config (background mode)
├── .env.example          — environment variable template
├── LICENSE               — CC BY 4.0
└── assets/               — setup screenshots

```

## Quick facts

|                 |                                                      |
| --------------- | ---------------------------------------------------- |
| Network         | Nillion L1 Testnet (Sepolia)                         |
| Part of         | Dusk — Covenants roadmap phase 1                     |
| Requirements    | Ubuntu VPS, Docker, EVM wallet (Rabby/MetaMask)      |
| Minimum stake   | 10 NIL (testnet)                                     |
| Minimum funding | 0.06 ETH + 10 NIL to register, 0.05 ETH to fund node |

## Author

Ksu 👠  — Nillion Operator Guild Champion (Community Helper, Educational Contributor & Guild Champion, Aug 2026). Also operates a Blacklight verifier node on L2 mainnet.

📎 X- https://x.com/Sabrina94729
📎 Medium - https://medium.com/@just_ksu
📎 Discord - just_ksu

## License

[CC BY 4.0](LICENSE) — free to use and adapt, with attribution.

## Disclaimer

⚠️ **Testnet only — DYOR / NFA.** This material is for educational
purposes only. Everything you do on the testnet is your own
responsibility, including running a node, interacting with contracts,
claiming tokens from the faucet, and taking part in activity programs.
Testnets can change at any moment — parameters, rules, mechanics, and
conditions may shift without warning. Any rewards received on a
testnet do not guarantee future rewards or conversion to mainnet.
**Use a separate test wallet.** Do not enter private keys from
addresses that hold real assets. Evaluate all risks yourself. The
author is not responsible for any losses, mistakes, technical issues,
misunderstandings, or unmet expectations.
