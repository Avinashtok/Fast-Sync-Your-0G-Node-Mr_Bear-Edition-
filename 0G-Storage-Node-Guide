# 🚀 Fast Sync Your 0G Node - Mr\_Bear Edition

A simple guide to help you run, stop, and fast-sync your 0G Storage Node using snapshots. Ideal for those looking to avoid syncing from scratch.

🔧 **Includes:**

* 📈 Snapshot-based sync from block `3514016`
* 🛠️ Systemd commands for start/stop
* 📁 Log monitoring & troubleshooting
* 📬 Telegram Support: [@Mrr\_Bear](https://t.me/Mrr_Bear)

---

## 📘 Introduction

### 💽 Storage Nodes:

Storage Nodes manage and serve data, unlike Validator Nodes that focus on blockchain security. They ensure persistence and availability for long-term data such as training datasets or large AI models. Running one helps decentralize and distribute 0G data effectively.

---

## 👨‍💻 0G Storage Node Guide by MrBear

---

## 🖥️ System Requirements

![System Requirements](https://raw.githubusercontent.com/Avinashtok/README/main/Screenshot%202025-07-08%20103325.png)

---

## 📦 Prerequisites

Before setting up your storage node:

* 📜 Understand that 0G Storage Nodes interact with on-chain smart contracts for blob root confirmation and PoRA (Proof of Random Access) mining.
* 📄 Refer to the official documentation for deployed contract addresses: [0G Testnet Overview](https://docs.0g.ai/developer-hub/testnet/testnet-overview)
* 🗄️ Storage Note: Nodes may consume up to 130 GB per day. For uninterrupted 24-hour operation, it's advised to have at least 1 TB of disk space.
* ⚙️ For best results and fast transactions, always prefer official RPC endpoints listed in 0G documentation.

---

## ⚙️ Pre-Requirements

* 🌐 Add 0G-Galileo-Testnet chain: [testnet-information](https://docs.0g.ai/run-a-node/testnet-information)
* 💧 Take faucet: [faucet.0g.ai](https://faucet.0g.ai/)

---

## 📦 Install Dependencies

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install curl iptables build-essential git wget lz4 jq make cmake gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip screen ufw -y
```

### 🦀 Install rustup

```bash
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env
```

✅ Check version:

```bash
rustc --version
```

### 🟢 Install Go

```bash
wget https://go.dev/dl/go1.24.3.linux-amd64.tar.gz && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf go1.24.3.linux-amd64.tar.gz && \
rm go1.24.3.linux-amd64.tar.gz && \
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc && \
source ~/.bashrc
```

✅ Check version:

```bash
go version
```

---

## 🧾 Clone the Repository

```bash
git clone https://github.com/0glabs/0g-storage-node.git
cd 0g-storage-node && git checkout v1.0.0 && git submodule update --init
```

---

## ⚙️ Build in Release Mode

```bash
cargo build --release
```

🧰 Double-check Release Mode:

```bash
cargo build --release
```

---

## 🧰 Set Configurations

```bash
rm -rf $HOME/0g-storage-node/run/config.toml
curl -o $HOME/0g-storage-node/run/config.toml https://raw.githubusercontent.com/Mayankgg01/0G-Storage-Node-Guide/main/config.toml
```

🔑 Add your wallet private key (without 0x prefix):

```bash
nano $HOME/0g-storage-node/run/config.toml
```

![Edit Config](https://github.com/user-attachments/assets/a513812f-177e-4a74-83a9-1548c98f4556)

---

## 🌐 Configure Custom RPC

Choose RPC from: [AstroStake RPC Status](https://www.astrostake.xyz/0g-status) and edit `config.toml`

![Custom RPC](https://github.com/user-attachments/assets/44b682a5-45ce-4fc8-8c3a-7f2355f3b9ac)

---

## 🔧 Create Systemd Service

```bash
sudo tee /etc/systemd/system/zgs.service > /dev/null <<EOF
[Unit]
Description=ZGS Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/0g-storage-node/run
ExecStart=$HOME/0g-storage-node/target/release/zgs_node --config $HOME/0g-storage-node/run/config.toml
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable zgs
sudo systemctl start zgs
```

---

## 📥 Download Snapshot For Faster Sync

📈 Sync starts from block `3514016`

### 🛠️ Snapshot Instructions

1. 🔴 Stop Node:

```bash
sudo systemctl stop zgs
```

2. 🧹 Delete old database:

```bash
rm -rf $HOME/0g-storage-node/run/db/flow_db
```

3. ⬇️ Download Snapshot:

```bash
wget -O flow_db.tar.gz https://github.com/Avinashtok/0g-fast-sync/releases/download/backup-3507655/flow_db-3507655.tar.gz
```

4. 📦 Extract:

```bash
tar -xzvf $HOME/0g-storage-node/run/db/flow_db.tar.gz -C $HOME/0g-storage-node/run/db/
```

5. 🔄 Restart Node:

```bash
sudo systemctl restart zgs
```

---

## 📊 Monitor Your Node

### ✅ Check Node Status:

```bash
sudo systemctl status zgs
```

![Sync Monitor](https://github.com/Avinashtok/README/raw/main/Screenshot%202025-07-08%20083738.png)

### 🔍 View Logs:

```bash
tail -f ~/0g-storage-node/run/log/zgs.log.$(TZ=UTC date +%Y-%m-%d)
```

### 🧪 Watch Sync Progress:

```bash
while true; do \
response=$(curl -s -X POST http://localhost:5678 -H "Content-Type: application/json" \
-d '{"jsonrpc":"2.0","method":"zgs_getStatus","params":[],"id":1}'); \
logSyncHeight=$(echo $response | jq '.result.logSyncHeight'); \
connectedPeers=$(echo $response | jq '.result.connectedPeers'); \
echo -e "logSyncHeight: \033[32m$logSyncHeight\033[0m, connectedPeers: \033[34m$connectedPeers\033[0m"; \
sleep 5; \
done
```

![Snapshot Screenshot](https://github.com/Avinashtok/README/raw/main/Screenshot%202025-07-08%20072343.png)

---

## 🧹 How to Clear Data & Restart Your Node 🛠️

*If your VPS storage is running low or cluttered, follow these to clean up:

```bash
sudo systemctl stop zgs
```
```bash
sudo systemctl disable zgs
```
```bash
sudo rm /etc/systemd/system/zgs.service
```
```bash
rm -rf $HOME/0g-storage-node
```

---

## 🔔 Additional Notes

🔐 **Security**: Guard your `miner_key` like treasure 🏰🔑. If exposed, your rewards and control can be stolen! 💸

🌐 **Network**: Keep internet stable and ports open so your node can communicate 🔌🌍

👀 **Monitoring**: Track logs and resource use. A smooth-running node = optimal sync 🖥️✨

🔄 **Stay Updated**: Always check for latest software updates and follow [official documentation](https://docs.0g.ai/).

🌟 **Contribution**: Running a node supports decentralization 💪 and rewards you along the way 🚀💰
