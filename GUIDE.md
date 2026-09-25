# Full Guide: Running a Nillion L1 Testnet Node

This tutorial walks through setting up a node on Nillion's new L1
testnet on Sepolia. This isn't an update to an existing Blacklight L2
mainnet node — it's a separate testnet, part of **Dusk**, the first
phase of Nillion's Covenants roadmap.

This guide assumes you already know how to rent a server and set up a
VPS (renting/provisioning is not covered here). Written primarily for
a VPS running Ubuntu Linux.

> 📸 Screenshots for every step are available in the
> [`/assets`](./assets) folder.

## Contents

1. [Create a wallet](#1-create-a-wallet)
2. [Claim testnet tokens](#2-claim-testnet-tokens)
3. [Set up the VPS and node dashboard](#3-set-up-the-vps-and-node-dashboard)
4. [Run the node](#4-run-the-node)
5. [Register and stake](#5-register-and-stake)
6. [Fund the node](#6-fund-the-node)
7. [Back up your keys](#7-back-up-your-keys)
8. [Disclaimer](#disclaimer)

---

## 1. Create a wallet

Create a new EVM wallet to use for registering your node — any EVM
wallet like Rabby or MetaMask works, but ideally a separate one from
your main wallets.

## 2. Claim testnet tokens

Claim your test 20 NIL from the Nillion faucet, then grab some Sepolia
ETH from a faucet of your choice:

- Google Cloud for Web3 (requires a Google account)
- Conduit (requires GitHub)
- Alchemy

## 3. Set up the VPS and node dashboard

Once your VPS is rented, set up, and updated, go to the node setup
page and connect your wallet — make sure the network is set to
**Sepolia**. Click **Set up node**.

Confirm you have enough test tokens: minimum **0.06 ETH** and
**10 NIL**. Once both checkmarks turn green, continue.

Choose your operating system and install Docker. If Docker is already
installed, skip ahead; otherwise copy the install command shown for
your OS.

## 4. Run the node

Pull the Docker image and start the node container, then run the
official node command from the dashboard. On first run, the terminal
will show `REGISTRATION REQUIRED` — the node generates its keys and
displays an **OPERATOR ADDRESS** and **MPK**. Enter both values into
the setup dashboard.

The official command (from the dashboard) uses `-it --rm`, which
shuts the node down when you disconnect from the server. Two
alternatives keep it running:

### Option A — background mode (recommended)

See [`docker-compose.yml`](./docker-compose.yml) for the full setup,
or run directly:

```bash
docker run -d --init --name blacklight-l1-node \
  --restart unless-stopped \
  -v blacklight-l1-node-state:/data/state \
  -e RPC_URL=https://l1-rpc-proxy.testnet.nillion.network \
  -e CONFIG_ADDRESS=0xebB338689fB32317DDFD8282F8a42dcA6271cB2d \
  -e STATE_FILE=/data/state/node-state.json \
  -e FEED_MODE=real \
  ghcr.io/nillionnetwork/blacklight-l1-node/node:0.1.0
```

**What the flags do:**

| Flag | Purpose |
|---|---|
| `-d` | runs the container in detached (background) mode |
| `--init` | ensures clean signal handling / process reaping |
| `--restart unless-stopped` | auto-restarts after a server reboot or crash |
| `-v blacklight-l1-node-state:/data/state` | isolated volume that persists the node's keys and state |
| `-e RPC_URL=...` | testnet RPC endpoint |
| `-e CONFIG_ADDRESS=...` | on-chain config contract address |
| `-e STATE_FILE=...` | path to the node's local state file |
| `-e FEED_MODE=real` | run against live testnet data feed |

View the logs to retrieve your registration keys:

```bash
docker logs --tail 20 blacklight-l1-node
```

Copy the **OPERATOR ADDRESS** (starts with `0x...`) and **MPK** from
the `REGISTRATION REQUIRED` block in the log output.

### Option B — tmux session

Unlike the flags above, `tmux` doesn't change the dashboard's command
at all — you just run it as-is inside a session that survives an SSH
disconnect.

```bash
# install tmux
sudo apt update && sudo apt install tmux -y

# start a session
tmux new -s nillion

# run the standard command from the dashboard inside the session,
# then detach with Ctrl+B, D — the node keeps running in the background

# reattach later to check logs
tmux a -t nillion
```

After entering the operator address and MPK on the dashboard, click
**Continue** to proceed to registration.

## 5. Register and stake

In the **STAKE (NIL)** field, enter the amount you want to stake
(minimum 10 NIL). Click **Approve & Register** and confirm the wallet
transaction.

## 6. Fund the node

The node needs test ETH to post reports and rotate keys regularly.
Click **Send** to transfer the recommended **0.05 Sepolia ETH**
directly to the generated node address, then sign the transaction.

If successful, you'll land on a final step with a link to your node's
dashboard: `https://blacklight-l1.testnet.nillion.com/nodes/`.

Your node is now live.

## 7. Back up your keys

Copy the node's key and state file directly out of the container:

```bash
docker cp blacklight-l1-node:/data/state/node-state.json ./node-state-backup.json
cat ./node-state-backup.json
```

Store this backup somewhere safe.

---

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
