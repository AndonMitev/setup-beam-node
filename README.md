# Beam Node Setup

## 1. Connect to Contabo

```bash
ssh root@162.244.24.79
```

## 2. Create User (Optional)

```bash
adduser avalanche
usermod -aG sudo avalanche
su - avalanche
```

## 3. Install AvalancheGo and Beam Subnet-EVM

### 3.1 Create Base Directory with Proper Permissions

```bash
sudo mkdir -p /home/avalanche/.avalanchego
sudo chown -R avalanche:avalanche /home/avalanche/.avalanchego
```

### 3.2 Download and Setup AvalancheGo v1.12.0

> Note: This guide is compatible with AvalancheGo version 1.12.0. [Reference Documentation](https://docs.onbeam.com/chain/nodes/introduction)

```bash
cd /home/avalanche
sudo wget https://github.com/ava-labs/avalanchego/releases/download/v1.12.0/avalanchego-linux-amd64-v1.12.0.tar.gz
sudo tar -xvf avalanchego-linux-amd64-v1.12.0.tar.gz
sudo cp -r avalanchego-v1.12.0/* /home/avalanche/.avalanchego/
```

### 3.3 Create Plugins Directory and Install Beam Subnet-EVM

```bash
sudo mkdir -p /home/avalanche/.avalanchego/plugins
cd /home/avalanche
sudo mkdir subnetevm
cd subnetevm
sudo wget https://github.com/ava-labs/subnet-evm/releases/download/v0.6.12/subnet-evm_0.6.12_linux_amd64.tar.gz
sudo tar -xvzf subnet-evm_0.6.12_linux_amd64.tar.gz
sudo cp subnet-evm /home/avalanche/.avalanchego/plugins/kLPs8zGsTVZ28DhP1VefPCFbCgS7o5bDNez8JUxPVw9E6Ubbz
```

### 3.4 Setup Configuration Directories

```bash
sudo mkdir -p /home/avalanche/.avalanchego/configs
sudo mkdir -p /home/avalanche/.avalanchego/configs/chains/2tmrrBo1Lgt1mzzvPSFt73kkQKFas5d1AP88tv9cicwoFp8BSn
```

### 3.5 Create Node Configuration

```bash
sudo nano /home/avalanche/.avalanchego/configs/node.json
```

Add the following JSON:

```json
{
  "track-subnets": "eYwmVU67LmSfZb1RwqCMhBYkFyG8ftxn6jAwqzFmxC9STBWLC"
}
```

### 3.6 Download `upgrade.json`

```bash
cd /home/avalanche/.avalanchego/configs/chains/2tmrrBo1Lgt1mzzvPSFt73kkQKFas5d1AP88tv9cicwoFp8BSn
sudo wget https://raw.githubusercontent.com/Merit-Circle/beam-subnet/main/subnets/beam-mainnet/upgrade.json
```

### 3.7 Create Systemd Service File

```bash
sudo nano /etc/systemd/system/avalanchego.service
```

Add the following configuration:

```ini
[Unit]
Description=AvalancheGo systemd service
After=network.target

[Service]
Type=simple
User=avalanche
ExecStart=/home/avalanche/.avalanchego/avalanchego --config-file=/home/avalanche/.avalanchego/configs/node.json
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
```

### 3.8 Set Correct Permissions

```bash
sudo chown -R avalanche:avalanche /home/avalanche/.avalanchego
```

### 3.9 Start the Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable avalanchego
sudo systemctl start avalanchego
```

### 3.10 Check Service Status

```bash
sudo systemctl status avalanchego
```

### 3.11 Check Sync Status

```bash
sudo journalctl -u avalanchego -f

```
