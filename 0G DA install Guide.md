# 0G-DA-Node Install Guide

## System Requirements
| Category | Requirements |
| ------------ | ------------ |
| CPU | 8+ cores |
| RAM | 16+ GB |
| Storage | 1 TB NVME SSD |
| Bandwidth | 100 MBps for Download / Upload |

### System Update
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

```bash
git clone https://github.com/0glabs/0g-da-node.git
```
```bash
sudo apt install libssl-dev
```
```bash
sudo apt install pkg-config
```
```bash
cd && cd 0g-da-node
```
```bash
cargo build --release
```
```bash
cd dev_support
```
```bash
./download_params.sh
```
```bash
sudo cp -R /root/0g-da-node/dev_support/params /root/0g-da-node/target/release
```
```bash
cd
cd 0g-da-node
```
```bash
cargo run --bin key-gen
```
```bash
sudo tee /etc/systemd/system/da.service > /dev/null <<EOF
[Unit]
Description=DA Node
After=network.target

[Service]
User=root
WorkingDirectory=$HOME/0g-da-node/target/release
ExecStart=$HOME/0g-da-node/target/release/server --config $HOME/0g-da-node/config.toml
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
```bash
sudo systemctl daemon-reload && \
sudo systemctl enable da && \
sudo systemctl start da
```
```bash
sudo journalctl -u da -f -o cat
```
