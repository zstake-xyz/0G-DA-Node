# 0G-DA-Node Install Guide

git clone https://github.com/0glabs/0g-da-node.git

sudo apt install libssl-dev

sudo apt install pkg-config

cd && cd 0g-da-node

cargo build --release

cd dev_support

./download_params.sh

sudo cp -R /root/0g-da-node/dev_support/params /root/0g-da-node/target/release

cd
cd 0g-da-node

cargo run --bin key-gen

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

sudo systemctl daemon-reload && \
sudo systemctl enable da && \
sudo systemctl start da

sudo journalctl -u da -f -o cat
