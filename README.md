# `StarCoin`测试转主网挖矿教程

> 记录工作相关的东西， 杂七杂八的。

### 1、修改账户相关信息


```
# 关掉服务
systemctl stop starcoin_miner
systemctl stop starcoin
# 升级挖矿相关代码
cd ~/starcoin
rm * -rf
wget https://github.com/starcoinorg/starcoin/releases/download/v1.0.0/starcoin-ubuntu-latest.zip
unzip starcoin-ubuntu-latest.zip

tee /etc/systemd/system/starcoin.service > /dev/null <<EOF 
[Unit]
Description=starcoin Full Node
After=network-online.target

[Service]
User=root
ExecStart=/root/starcoin/starcoin-artifacts/starcoin -n main
Restart=always
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF

# 删除主网的原始数据
rm -rf ~/.starcoin/main/account_vaults
# 复制测试账户到主网
cp -rf ~/.starcoin/barnard/account_vaults ~/.starcoin/main/
# 启动服务
systemctl start starcoin
systemctl start starcoin_miner
```




















```
# 测试网

[Unit]
Description=starcoin Full Node
After=network-online.target

[Service]
User=root
ExecStart=/root/starcoin/starcoin-artifacts/starcoin -n barnard --push-server-url http://miner-metrics-pushgw.starcoin.org:9191/ --push-interval 5 --miner-thread 2
Restart=always
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```
