💬 PortalMatch - Web3 Anonymous Matchmaking dApp on Portaldot

PortalMatch is a privacy-first, 1-on-1 anonymous matchmaking and chatting dApp built on top of the **Portaldot Appchain network**. It features auto-generated local burner wallets, Telegram WebApp identity integration, and strictly requires real on-chain Substrate transactions to pay gas fees and join the matchmaking queue.

This project has been optimized and prepared for the **Portaldot Dev-Cycle Lab / Hackathon Demo Day**.

---

## 🎯 Demo Day - GREEN Criteria Checklist
This project fully satisfies the **🟢 GREEN (Low Risk, Demo Day Ready)** technical requirements:
* **✅ Local Portaldot Node Runs:** Seamlessly operates on a 2-node local P2P network configuration (Alice & Bob).
* **✅ Project Connected to Local Node:** Frontend establishes a direct WebSocket connection to `ws://127.0.0.1:9944` using `@polkadot/api`.
* **✅ Real On-Chain Transaction Works:** Clicking search triggers a real `balances.transferKeepAlive` transaction, sending a 0.05 POT matchmaking fee to the system Treasury.
* **✅ POT Gas / Fee is Shown:** Dynamically reads and updates the account state via `system.account` to render the user's real-time POT balance directly on the UI.
* **✅ MVP Core Flow Runs End-to-End:** Complete functional flow from automated wallet creation -> on-chain gas settlement -> live multi-window matchmaking queue -> instant peer-to-peer chatting.
* **✅ Mocked Parts Do Not Affect Core Flow:** Firebase Firestore is utilized purely as an ephemeral off-chain message routing mechanism, leaving the financial, identity, and consensus layers entirely on-chain.

---

## 🛠️ Tech Stack & Dependencies
* **Blockchain Infrastructure:** Portaldot (Substrate-based Local Appchain Network)
* **Frontend Design:** HTML5, Vanilla JavaScript, TailwindCSS, FontAwesome Icons
* **Web3 Libraries:** `@polkadot/api`, `@polkadot/keyring`, `@polkadot/util-crypto`
* **Real-Time Data Routing:** Firebase Firestore (for signaling chat rooms)
* **Target Platform:** Telegram WebApp API Integration
* **Media Integrations:** Giphy Stickers API Integration

---

## 🚀 How to Run Locally Using Visual Studio Code

### 1. Launch the Portaldot Appchain Nodes
Ensure your local Ubuntu WSL2 or native Linux environment has the Portaldot testnet package running. To allow the frontend browser context to interact with the Substrate RPC websocket, run the nodes with external access flags enabled.

**Terminal 1 (Alice Node):**

```

```bash
cd ~/portaldot/portaldot-testnet-ubuntu
./portaldot_dev --dev --alice --name portal_alice --base-path /tmp/alice --rpc-cors all --unsafe-ws-external --unsafe-rpc-external

```

*Make sure to copy the unique Peer ID printed in Alice's terminal log.*

**Terminal 2 (Bob Node):**

```bash
./portaldot_dev --dev --bob --name portal_bob \
--base-path /tmp/bob \
--port 30334 \
--rpc-port 9945 \
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/<PASTE_ALICE_PEER_ID_HERE>

```

### 2. Set Up the Project Frontend in VS Code

1. Open **Visual Studio Code**.
2. Click **File > Open Folder...** and choose the directory containing your `index.html`.
3. Open the **Extensions** marketplace view on the left bar (`Ctrl + Shift + X`).
4. Search for **Live Server** (developed by *Ritwick Dey*) and click **Install**.

### 3. Serve the Application

1. Click back to your file explorer view and open your `index.html` file.
2. **Right-click** anywhere inside the code editor screen and select **"Open with Live Server"**, OR simply click the blue **"Go Live"** button located on the bottom status bar of VS Code.
3. If a Windows Defender Firewall alert pops up, ensure you check both **Private** and **Public** networks, then click **Allow Access**.
4. Your default web browser will automatically load the application at: `http://127.0.0.1:5500/index.html`

---
