# PortalDot Hackathon 2026 — PortalMatch

### PortalMatch

## Project Overview
- **Problem Statement:** Modern Web2 chat platforms force users to surrender sensitive PII (phone numbers, emails) just to talk to strangers, and because account creation is free, they are heavily plagued by spam bots and sybil attacks. Meanwhile, emerging Web3 social dApps often fail due to the "cold-start hell," where empty matchmaking queues kill user retention.
- **Solution:** PortalMatch is a privacy-first, 1-on-1 anonymous matchmaking dApp. It automatically generates ephemeral local burner wallets and enforces a strict on-chain financial barrier. Users must execute a real Substrate transaction (paying 0.05 POT as a matchmaking gas fee) to enter the queue. This flips the economics of spam, making bot networks financially unsustainable while keeping real users 100% anonymous.
- **Blockchain Relevance:** Built natively on the **Portaldot Appchain**. The project heavily utilizes Substrate's local node architecture, executing verifiable `balances.transferKeepAlive` transactions and reading live on-chain states via `system.account` to govern the social matchmaking flow.

### Technical Architecture
- **Overall architecture diagram:**
```text
[ Telegram WebApp / Browser ] 
       │
       ├─(1) Auto-Generates Burner Wallet (sr25519)
       ▼
[ Matchmaking Request ] ───(2) User clicks "Search"
       ├─(3) Signs & Broadcasts Transaction
       ▼
[ ⛓️ PORTALDOT APPCHAIN (ON-CHAIN) ]
       │   └─ Executes: balances.transferKeepAlive
       │   └─ Deducts: 0.05 POT (Gas/Match Fee)
       ▼
[ ⚡ FIREBASE FIRESTORE (OFF-CHAIN) ]
       │   └─ Listens for On-Chain Success
       │   └─ Pairs users & provisions secure room
       ▼
[ 💬 1-on-1 Secure Chat Panel ]

```

* **Core tech stack:**
* **Blockchain platform:** Portaldot Appchain (Local Node / Substrate Framework)
* **Smart contract language:** N/A (Utilizes Native Substrate Pallets)
* **Frontend framework:** HTML5, Vanilla JS, TailwindCSS
* **Other components:** `@polkadot/api`, `@polkadot/util-crypto`, Firebase Firestore (Ephemeral signaling router), Telegram WebApp API.



### Smart Contracts *(skip if not applicable)*

*This project does not deploy custom WASM/ink! contracts. It directly leverages the native Substrate `Balances` pallet built into the Portaldot Appchain runtime for gas and treasury fee settlements.*

### Installation & Setup

#### Requirements

* Ubuntu WSL2 or Native Linux environment
* Portaldot Testnet Node Binaries
* Visual Studio Code with "Live Server" extension

#### Steps

1. **Launch the Portaldot Appchain Node (Alice):**
Open your first terminal and start the Alice validator node:
```bash
cd ~/portaldot/portaldot-testnet-ubuntu
./portaldot_dev --dev --alice --name portal_alice --base-path /tmp/alice --rpc-cors all --unsafe-ws-external --unsafe-rpc-external

```


2. **Launch the Portaldot Appchain Node (Bob):**
Open a second terminal and start the Bob sync node.
*(Note: Replace `<ALICE_PEER_ID>` with the actual Peer ID string printed in Alice's terminal logs).*
```bash
./portaldot_dev --dev --bob --name portal_bob --base-path /tmp/bob --port 30334 --rpc-port 9945 --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/<ALICE_PEER_ID>

```


3. **Clone the repository:**
```bash
git clone https://github.com/philia-dev/PortalMatch.git
cd PortalMatch

```


4. **Install dependencies:** No `npm install` required for the frontend. CDN links are embedded natively in the HTML file.
5. **Compile & deploy:** N/A (Frontend logic runs entirely client-side).
6. **Launch frontend:** Open the cloned folder in VS Code, right-click `index.html`, and select **"Open with Live Server"**. The app will launch locally at `http://127.0.0.1:5500/index.html`.

### Demo

* **Live demo link:** https://drive.google.com/file/d/1B627gBaf60CqwuZVUS-Xak_gQu0IS2W6/view
* **Test accounts / test data:** To test the flow, generate two separate burner wallets by opening the app in a normal tab and an Incognito tab. Fund these auto-generated wallets with test POT using the [Polkadot.js Apps Local RPC](https://www.google.com/search?q=https://polkadot.js.org/apps/%3Frpc%3Dws://127.0.0.1:9944%23/accounts) transferring from the default **ALICE** developer account.

### Roadmap

* **Completed features:**
* Automated local Substrate wallet generation (`sr25519`).
* Integration with `@polkadot/api` for live balance fetching and transaction signing.
* On-chain sybil-resistance execution (0.05 POT deduction).
* P2P matchmaking and secure off-chain chat routing.


* **Next phase plans:**
* **Gas Cashback (Zero-Fee Matching):** Implementing "Happy Hours" where users who match and actively chat receive a 100% refund on their matchmaking gas fee to combat cold-start hell.
* **"Jackpot Match" (Gacha Element):** 5-10% chance during Happy Hour for matched users to win an automated 1 POT airdrop to bootstrap viral liquidity.
* **Telegram Referral Integration:** Seamless invite system utilizing Telegram's native sharing components.



### Team

* **Team name:** Philia
* **Members & roles:** Philia (Solo Web3 Developer, Node Runner, modular blockchain enthusiast)
* **Contact info:** @philia_adenaua (discord)

### License

MIT License

```

```
