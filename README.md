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
* **Smart contract language:** N/A (Utilizes Native Substrate Pallets instead of WASM/ink! for maximum efficiency)
* **Frontend framework:** HTML5, Vanilla JS, TailwindCSS
* **Other components:** `@polkadot/api`, `@polkadot/util-crypto`, Firebase Firestore (Ephemeral signaling router), Telegram WebApp API.



### Smart Contracts

*This project does not deploy custom WASM/ink! contracts. It directly leverages the native Substrate `Balances` pallet built into the Portaldot Appchain runtime for gas and treasury fee settlements to execute the sybil-resistance logic.*

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


3. **Clone the Repository:**
Open your terminal and run the following commands to clone the correctly named project repository:
```bash
git clone https://github.com/philia-dev/portaldot-hackathon-2026-PortalMatch-Philia.git
cd portaldot-hackathon-2026-PortalMatch-Philia

```


4. **Set Up the Project Frontend in VS Code:**
* Type `code .` in your terminal or open Visual Studio Code manually and select **File > Open Folder...** to open the cloned directory.
* Open the Extensions view in VS Code (`Ctrl + Shift + X`).
* Search for **Live Server** (authored by *Ritwick Dey*) and click **Install**.


5. **Serve the Application:**
* Open the `index.html` file in your VS Code editor.
* Right-click anywhere inside the code editor and select **"Open with Live Server"**, or click the blue **"Go Live"** button on the bottom right of the VS Code status bar.
* If prompted by your Operating System's firewall, allow access for both Private and Public networks.
* Your default browser will automatically open the application at `http://127.0.0.1:5500/index.html`. *(Note: Do not open the HTML file directly via the `file://` protocol, as this will restrict local storage permissions and break the Web3 wallet generation).*



### Demo

* **Live demo link:** https://drive.google.com/file/d/1B627gBaf60CqwuZVUS-Xak_gQu0IS2W6/view
* **Test accounts / test data (Simulating 2 Users):**
To test the end-to-end matchmaking flow and verify the on-chain fee deductions locally, follow this exact sequence to simulate two independent users:

**A. Generate Separate Client Identifiers (Burner Wallets)**
1. **User 1:** Keep your main browser tab open at `http://127.0.0.1:5500/index.html`. The app will instantly generate a local Substrate wallet address (`sr25519`) and display a balance of `0.0000 POT`.
2. **User 2:** Open a new **Incognito / Private Browsing** window and navigate to the same local URL. Using Incognito ensures the browser's local storage is isolated, forcing the app to generate a second, unique burner wallet address.


**B. Distribute Testnet Liquidity (Existential Deposit & Gas)**
1. Open the [Polkadot.js Apps Explorer](https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9944#/accounts) connected to your local Portaldot node.
2. Locate the default **ALICE** development account.
3. Click **Send** next to ALICE's account.
4. Copy **User 1's** generated wallet address from your browser UI, paste it as the "send to address" in Polkadot.js, set the amount to **1000 UNIT**, and click **Make Transfer > Sign and Submit**.
5. Repeat the exact same process to send **1000 UNIT** to **User 2's** generated address.


**C. Execute On-Chain Matchmaking & Off-Chain Chat**
1. **Refresh the UI:** Go back to both User 1 and User 2 browser windows and refresh the pages (`F5`). Both should now display the funded POT balances.
2. **Initiate User 1:** In the User 1 window, set your age and gender preferences, then click **Search Now (Gas: POT)**. The app will locally sign the transaction, deduct the 0.05 POT fee, broadcast it to the Appchain, and show the "Finding Match..." loading screen.
3. **Initiate User 2:** Quickly switch to the User 2 (Incognito) window, set the preferences, and click **Search Now (Gas: POT)**.
4. **The Match:** Once the Portaldot network finalizes User 2's block transaction, the Firebase signaling server will pair them together.
5. **Chat Interface:** The UI will dynamically transition into the secure 1-on-1 chat room simultaneously across both browser windows. Send a text message or a Giphy sticker to verify the off-chain routing.



### Roadmap

**Completed features:**
* Automated local Substrate wallet generation (`sr25519`).
* Integration with `@polkadot/api` for live balance fetching and transaction signing.
* On-chain sybil-resistance execution (0.05 POT fee deduction).
* P2P matchmaking and secure off-chain chat routing via Firebase.


**Next phase plans:**
* **Gas Cashback (Zero-Fee Matching):** Implementing "Happy Hours" where users who match and actively chat receive a 100% refund on their matchmaking gas fee to combat cold-start hell.
* **"Jackpot Match" (Gacha Element):** 5-10% chance during Happy Hour for matched users to win an automated 1 POT airdrop to bootstrap viral liquidity.
* **Telegram Referral Integration:** Seamless invite system utilizing Telegram's native WebApp sharing components.



### Team

* **Team name:** Philia
* **Members & roles:** Philia (Solo Web3 Developer, Node Runner, modular blockchain enthusiast)
* **Contact info:** @philia_adenaua (discord)

### License

MIT License

```

```
