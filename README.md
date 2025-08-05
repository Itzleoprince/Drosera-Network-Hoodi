# Drosera-Network-Hoodi
This guide outlines the steps to participate in the Drosera Hoodi testnet, using your PHONE or VPS. 

# System Setup Recommendations
* At least 4 GB of RAM
* 2 CPU Cores
* Minimum 20 GB of available storage space
* You can get a `VPS` here https://contabo.com/en/vps/ and select your choice <img width="1901" height="890" alt="image" src="https://github.com/user-attachments/assets/089b3814-c0dd-4692-94dd-bdbfcd0b3955" />
* Create an account and get your `Hoodi RPC` here https://www.ankr.com/rpc/projects Switch to testnet and get your RPC.
* Get your `Hoodi ETH faucet` here https://stakely.io/faucet/ethereum-hoodi-testnet-eth  <img width="1776" height="907" alt="image" src="https://github.com/user-attachments/assets/16d5ab36-56f2-47b2-8038-ef5cdeccb01b" />

# Install Termius and Login your `VPS` Details [EITHER PHONE OR SYSTEM]
<img width="960" height="508" alt="image" src="https://github.com/user-attachments/assets/e1ca5c53-5c8c-466a-8986-e7c34f630b2c" />

# Update and upgrade system packages
```
sudo apt-get update && sudo apt-get upgrade -y
```
#  Install required dependencies
```
sudo apt install -y curl ufw iptables build-essential git wget lz4 jq make gcc nano \
automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev \
tar clang bsdmainutils ncdu unzip
```
# Install Docker 
```
sudo apt update -y && sudo apt upgrade -y && \
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove -y $pkg; done && \
sudo apt-get update && \
sudo apt-get install -y ca-certificates curl gnupg && \
sudo install -m 0755 -d /etc/apt/keyrings && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
sudo chmod a+r /etc/apt/keyrings/docker.gpg && \
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && \
sudo apt update -y && \
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && \
sudo docker run hello-world
```

<h1 align="center">DROSERA TRAP SETUP</h1>

# Step 1. System Preparation
**Drosera CLI**

Install the Drosera CLI and start it:
```
curl -L https://app.drosera.io/install | bash
```
```
source ~/.bashrc
```
```
droseraup
```
**Foundry CLI**

Install and update Foundry:
```
curl -L https://foundry.paradigm.xyz | bash
```
```
source ~/.bashrc
```
```
foundryup
```
**Bun (JS runtime):**
```
curl -fsSL https://bun.sh/install | bash
```
```
source ~/.bashrc
```

# Step 2: Deploy Your Trap
* Create and enter a new project folder
```
mkdir my-drosera-trap && cd my-drosera-trap
```

