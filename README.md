# 💬 PortalMatch - Privacy-First Anonymous Matchmaking dApp on Portaldot Appchain

PortalMatch is an innovative, privacy-centric, 1-on-1 anonymous matchmaking and secure chatting decentralized application (dApp) engineered natively for the **Portaldot Appchain Network**. By synthesizing automated in-memory Substrate burner wallets, Telegram WebApp identity abstraction, and rigid on-chain financial routing, PortalMatch establishes a sybil-resistant communication network where cryptographic identity security coexists with robust token economics.

This repository contains the complete frontend implementation optimized for the **Portaldot Dev-Cycle Lab / Hackathon Season 1 Demo Day**.

---

## 🛑 The Problem: Privacy Invasions and the "Cold-Start Hell"
Modern web communications and social networks suffer from two compounding vulnerabilities:
1. **Identity & Spam Vulnerabilities (Web2):** Centralized casual chat platforms systematically force users to surrender highly sensitive PII (phone numbers, OAuth profiles, emails) to access peer connections. Concurrently, because account creation is economically free, these networks are perpetually flooded with sybil frameworks, malicious automated bots, and targeted phishing vector spams.
2. **The Cold-Start Trap (Web3 Social):** Emerging decentralized social applications frequently succumb to "cold-start hell"—an architectural state where low initial user density yields empty matchmaking queues, causing immediate user churn and completely destroying early-stage network effects.

---

## 💡 The Solution & Core Technical Innovations
PortalMatch introduces a dual-layered architectural solution to solve both decentralized identity privacy and network bootstrapping challenges:

### 1. Cryptographic Sybil Resistance via On-Chain Settlement
PortalMatch flips the economics of spam. Upon launching the application, the system dynamically derives a unique Substrate-compliant pair (`sr25519`) within the local browser storage using `@polkadot/util-crypto` to act as an ephemeral burner wallet. 
To enter the matchmaking pool, users cannot simply press a button; they **must execute a live on-chain Substrate extrinsic** (`balances.transferKeepAlive`) sending exactly **0.05 POT** to the system Treasury. By enforcing a tangible micro-financial friction point per match query, automated bot generation becomes economically unsustainable, creating an ironclad barrier against spam while ensuring 100% cryptographic anonymity.

### 2. Post-Hackathon Roadmap: Gamified Growth Loops
To mathematically mitigate the cold-start problem and drive user concentration during early deployment phases, PortalMatch introduces native economic tokenomic loops planned for the next iteration:
* **Gas Cashback (Zero-Fee Matching):** During high-priority system windows ("Happy Hours"), the anti-spam on-chain transaction barrier remains highly active. However, if matched users cross a verified organic engagement index (e.g., actively conversing for more than 2 minutes), an automated backend routine processes a **100% Gas Cashback refund** of the 0.05 POT fee directly from the Treasury back to the burner wallet.
* **"Jackpot Match" Incentivization:** To trigger intense viral user onboarding and FOMO, unrefunded gas fees collected during regular hours are accumulated into a public lottery pool. During Happy Hour events, every single successful match event triggers a randomized verifiable cryptographic seed check, offering a **5% to 10% chance to win a 1 POT Jackpot**, directly airdropped to both participants.
* **Network Bootstrapping via Seed Users:** Integrating directly with Telegram's WebApp sharing API allows immediate virality. During initial launch phases, official incentivized standby liquidity nodes (Seed Users funded via ecosystem grants) remain active in the queue to guarantee organic users achieve sub-10-second matchmaking resolution.

---

## 🎯 Technical Checklist: 🟢 GREEN (Demo Day Ready)
PortalMatch fully complies with the rigid criteria established for the highest production-ready tier:
* **✅ Native Portaldot Deployment:** Seamlessly communicates over a P2P local architecture powered by operational Alice & Bob validator profiles.
* **✅ Deep State Connection:** Establishes a direct WebSocket connection (`WsProvider`) targeting the RPC portal at `ws://127.0.0.1:9944` using `@polkadot/api`.
* **✅ On-Chain Financial State Integration:** Leverages active `system.account` subscription hooks to dynamically read, format, and display live POT token balances directly within the client UI.
* **✅ True Cryptographic Extrinsics:** Every matchmaking search signs and broadcasts a verifiable block mutation on-chain, moving real assets to the designated system Treasury.
* **✅ Structured Off-Chain Hybrid Routing:** Utilizes Firebase Firestore strictly as a high-speed, ephemeral off-chain message routing and signaling matrix, preventing block state bloat while keeping core financial and identity layers strictly on-chain.

---

## 🛠️ Tech Stack & Dependencies
* **Core Blockchain Layer:** Portaldot Network (Substrate-Based Dev Framework)
* **Web3/RPC Interface:** `@polkadot/api` v14+, `@polkadot/keyring`, `@polkadot/util-crypto`
* **Real-time Signal Router:** Firebase v11.6.1 (Firestore & Anonymous Auth Core)
* **Platform Wrapper:** Telegram WebApp API (`telegram-web-app.js`)
* **Presentation Layer:** Vanilla ECMAScript JS, TailwindCSS Engine, FontAwesome UI Elements
* **Media API Provider:** Giphy Verified Stickers SDK Architecture

---

## 🚀 Local Deployment & Setup Guide (Visual Studio Code)

Follow this step-by-step configuration to instantiate the Portaldot network environment and bind the PortalMatch application layout using Visual Studio Code.

### Step 1: Initialize the Local Portaldot Infrastructure
Ensure your local environment (Ubuntu, WSL2, or native Linux) has the verified Portaldot testnet node binaries compiled. You must spin up a local 2-node authority chain with fully exposed external RPC channels to allow the browser runtime to communicate with the Substrate runtime.

**Terminal 1 (Alice Validator Node):**
```bash
cd ~/portaldot/portaldot-testnet-ubuntu
./portaldot_dev --dev --alice --name portal_alice --base-path /tmp/alice --rpc-cors all --unsafe-ws-external --unsafe-rpc-external

```

*Observe the startup logs closely and copy the unique Peer ID string printed for the Alice node instance (e.g., `/p2p/12D3K...`).*

**Terminal 2 (Bob Sync Node):**

```bash
./portaldot_dev --dev --bob --name portal_bob \
--base-path /tmp/bob \
--port 30334 \
--rpc-port 9945 \
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/<PASTE_ALICE_PEER_ID_HERE>

```

### Step 2: Configure the Project Directory in VS Code

1. Launch **Visual Studio Code**.
2. Navigate to the top menu and select **File > Open Folder...**.
3. Choose the directory where your production-ready `index.html` file resides.
4. Open the internal extensions view panel (`Ctrl + Shift + X`).
5. Search for the extension **Live Server** (authored by *Ritwick Dey*) and click **Install**.

### Step 3: Launch the Frontend Web Server

1. Return to the Explorer tab and open the `index.html` file within the editor.
2. **Right-click** inside the open code panel and click **"Open with Live Server"**, or select the blue **"Go Live"** button located on the bottom right of the VS Code status bar.
3. If an Operating System network firewall notification prompts, explicitly check both **Private** and **Public** options, then select **Allow Access**.
4. The application will instantly launch on your default system browser at: `http://127.0.0.1:5500/index.html`

---

## 🎬 End-to-End Multi-User Demonstration

To thoroughly demonstrate the multi-client network matching mechanics and verified POT fee deductions, perform the following testing sequence:

### 1. Generate Separate Client Identifiers

* **User 1 Instance:** Retain the standard active tab launched by the Live Server extension (`http://127.0.0.1:5500/index.html`). The UI will automatically generate a new cryptographic wallet address showing `0.0000 POT`.
* **User 2 Instance:** Open an entirely separate **Incognito / Private Browsing Window** and access the identical URL. This forces the runtime to generate a secondary completely independent burner address profile.

### 2. Distribute Testnet Token Liquidity

To allow the burner accounts to satisfy the Substrate network's Existential Deposit (ED) and pay the 0.05 POT matchmaking entry gas, you must fund them from the developer reserve:

1. Open the local Substrate explorer link: [Polkadot.js Apps UI Portal](https://www.google.com/search?q=https://polkadot.js.org/apps/%3Frpc%3Dws://127.0.0.1:9944%23/accounts)
2. Locate the master development account (**ALICE**), which contains default system funds, and click **Send**.
3. Copy the public address generated by **User 1**, paste it into the recipient account input, set the transfer allocation value to **1000 UNIT**, and execute **Make Transfer > Sign and Submit**.
4. Repeat the exact token transfer steps to send another **1000 UNIT** to the public address generated in the **User 2** (Incognito) tab.

### 3. Execute Matchmaking and Off-Chain Signaling

1. Return to the browser instances and refresh both windows (`F5`). The UI balance fields will immediately update to reflect the received POT balances.
2. On the **User 1** layout, input demographic preferences (Age/Gender) and select **Search Now (Gas: POT)**. The backend will trigger a signing prompt via the local seed state, broadcast the block mutation, and transition smoothly into the pulsing *Finding Match...* tracking layout.
3. Shift immediately to the **User 2** window, align preferences, and select **Search Now (Gas: POT)**.
4. **Success! 🎉** Within exactly one block finalized time, both transactions clear the Substrate ledger, POT fee deductions reflect instantly across both wallet balances, and the application switches to the secure 1-on-1 chatting panel.

**DEMO VIDEO:**
```

 https://drive.google.com/file/d/1koNE6scC7BUOpGurtVI6ekojs884TPnh/view

```
