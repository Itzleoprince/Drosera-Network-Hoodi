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
* Set your Git identity (replace with your actual GitHub email and username):
```
git config --global user.email "you@example.com"
```
```
git config --global user.name "your_username"
```

# Push your `Discord Username` on-chain and earn CADET role

* Create new file 
```
nano src/Trap.sol
```
* Copy and paste this contract code inside:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ITrap} from "drosera-contracts/interfaces/ITrap.sol";

interface IMockResponse {
    function isActive() external view returns (bool);
}

contract Trap is ITrap {
    address public constant RESPONSE_CONTRACT = 0x25E2CeF36020A736CF8a4D2cAdD2EBE3940F4608;
    string constant discordName = "YOUR-DISCORD-USERNAME"; // add your discord name here

    function collect() external view returns (bytes memory) {
        bool active = IMockResponse(RESPONSE_CONTRACT).isActive();
        return abi.encode(active, discordName);
    }

    function shouldRespond(bytes[] calldata data) external pure returns (bool, bytes memory) {
        // take the latest block data from collect
        (bool active, string memory name) = abi.decode(data[0], (bool, string));
        // will not run if the contract is not active or the discord name is not set
        if (!active || bytes(name).length == 0) {
            return (false, bytes(""));
        }

        return (true, abi.encode(name));
    }
}
```
* Change `YOUR-DISCORD-USERNAME` to match your actual Discord Username
* Save File

* Initialize the Trap template
```
forge init -t drosera-network/trap-foundry-template
```
* Install JS dependencies and compile:
```
bun install
forge build
```

<img width="1028" height="307" alt="image" src="https://github.com/user-attachments/assets/0c904520-c591-4169-bb9f-94230257b32b" />

**Edit Drosera.toml file**
```
nano drosera.toml
```
* Update the variables with the following values:
* `path` = `"out/Trap.sol/Trap.json"`
* `response_contract` = `"0x25E2CeF36020A736CF8a4D2cAdD2EBE3940F4608"`
* `response_function` = `"respondWithDiscordName(string)"`

<img width="809" height="403" alt="image" src="https://github.com/user-attachments/assets/fd1c2295-736d-4daa-b038-b4120b72323f" />


**DEPLOY TRAP**
```
DROSERA_PRIVATE_KEY=PV_KEY drosera apply
```
* Replace `PV_KEY` with your `EVM` Private Key
* Once it asks, just type `ofc` and hit Enter

# For Existing users
* If you’ve deployed a Trap before, you might run into an error as an existing user. To fix it, just open your `drosera.toml` file and add your Trap address manually.

**Search for your trap by searching for your wallet address here app.drosera.io**

<img width="1870" height="542" alt="image" src="https://github.com/user-attachments/assets/adbfcdaf-7e75-41c1-b897-b43134681bc2" />


**Click on `Trap config` Highlighted in yellow** 

<img width="941" height="441" alt="image" src="https://github.com/user-attachments/assets/c01dc53e-a4d7-4cea-8688-9ff464a6123d" />


**Here is your trap address**

<img width="938" height="124" alt="image" src="https://github.com/user-attachments/assets/ef73e294-a0fd-42f3-8970-dda5067386c6" />


**Run this command while still in `my-drosera-trap` directory:**
```
nano drosera.toml
```
**Add your Trap address in this format:**
```
whitelist = ["YOUR WALLET ADDRESS"]
address = "YOUR TRAP ADDRESS"
```

<img width="824" height="440" alt="image" src="https://github.com/user-attachments/assets/9f6374bb-025f-444b-ad8e-ece4e6819010" />



**Apply changes with this command:**
```
DROSERA_PRIVATE_KEY=PV_KEY drosera apply
```
* Replace `PV_KEY` with your private key

# Step 3: Check your deployed Trap on Drosera Dashboard  

**Connect your Drosera EVM wallet at app.drosera.io**

Then, click **"Traps Owned"** to view your deployed Traps, or simply search for your Wallet address. 

<img width="1907" height="888" alt="image" src="https://github.com/user-attachments/assets/0a48a0a7-5080-470f-9d71-147f1451a73f" />

# Step 4: Send Bloom Boost
Deposit `Hoodi ETH` by clicking on **Send Bloom Boost** highlighted in yellow 

<img width="839" height="361" alt="image" src="https://github.com/user-attachments/assets/0abd86b9-2ef7-4a2f-a520-24a3c87aa254" />

# Step 5: Verify configuration and check for errors
```
drosera dryrun
```


<h1 align="center">SETUP YOUR OPERATOR</h1>

## Operator CLI Setup
```
cd ~
```
* Download
```
curl -LO https://github.com/drosera-network/releases/releases/download/v1.20.0/drosera-operator-v1.20.0-x86_64-unknown-linux-gnu.tar.gz
```
* Install
```
tar -xvf drosera-operator-v1.20.0-x86_64-unknown-linux-gnu.tar.gz
```

* Move drosera-operator
```
sudo cp drosera-operator /usr/bin
```
* Verify it's working
```
drosera-operator
```

## Install the required Docker image
```
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

## Register Operator 
```
drosera-operator register --eth-rpc-url https://ethereum-hoodi-rpc.publicnode.com --eth-private-key PV_KEY
```
* Change `PV_KEY` with your private key.
* You might get an error if you've registered an Operator before

## Open ports
```
sudo ufw allow ssh
```
```
sudo ufw allow 22
```
```
sudo ufw enable
```
```
sudo ufw allow 31313/tcp
```
```
sudo ufw allow 31314/tcp
```

### Configure Docker

```
git clone https://github.com/Itzleoprince/Drosera-Network-Hoodi
```
```
cd Drosera-Network
```
```
cp .env.examples .env
```
Edit `.env` file
```
nano .env
```
* Replace `your_evm_private_key` with your actual private key and `your_vps_public_ip` with your server's IP

Edit `docker-compose.yaml` file:
```
nano docker-compose.yaml
```
* Replace `YOUR PERSONAL HOODI RPC` with your personal `Hoodi RPC`
* Save

###  Run Your Operator
```
docker compose up -d
```
### Check Logs for error
```
docker logs -f drosera-node
```

### Last Step for New users
* Opt-in Trap
Go to your Drosera dashboard and click on opt-in highlighted in yellow below to connect your Operator

<img width="863" height="159" alt="image" src="https://github.com/user-attachments/assets/7a3a8908-61c3-4646-8948-47b6c7033adc" />

* Check for Green Blocks

<img width="822" height="149" alt="image" src="https://github.com/user-attachments/assets/5d0d9afb-d326-4480-9520-e39e7777e2fb" />










