## 创建钱包并部署 NEAR CLI
**创建钱包：**

![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p1.png)

**设置 CLI：**

    sudo apt update && sudo apt upgrade -y

**安装开发者工具、Node.js 和 npm**

    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash 
    sudo apt install build-essential nodejs
    PATH="$PATH"

**检查****Node.js和npm版本：**

    node -v
    npm -v

**安装** **NEAR-CLI**

    sudo npm install -g near-cli
    export NEAR_ENV=shardnet
    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
    echo 'export NEAR_ENV=shardnet' >> ~/.bash_profile
    source $HOME/.bash_profile


## 部署Nearcore

**下载** **Nearcore**

    git clone https://github.com/near/nearcore
    cd nearcore
    git fetch
    git checkout f7f0cb22e85e9c781a9c71df7dcb17f507ff6fde

**编译**

    cargo build -p neard --release --features shardnet
    mkdir -p ~/.near/shardnet

**初始化**

    ./target/release/neard --home ~/.near/shardnet init --chain-id shardnet --download-genesis

**获取配置文件**

    rm ~/.near/shardnet/config.json
    wget -O ~/.near/shardnet/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json

**启动节点**

    cd ~/nearcore
    ./target/release/neard --home ~/.near/shardnet run

## 激活节点
**登录**

    near login
![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p2.png)

**生成秘钥文件**

    near generate-key buildlinks.factory.shardnet.near
    cp ~/.near-credentials/shardnet/buildlinks.factory.shardnet.near.json ~/.near/validator_key.json

编辑“account_id” => buildlinks.factory.shardnet.near

    sed -i "s#private_key#secret_key#g" ~/.near/validator_key.json

## 挂载质押池
**部署质押池**

    export NEAR_ENV=shardnet
    near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "buildlinks", "owner_id": "buildlinks.shardnet.near", "stake_public_key": "ed25519:Bc43xBTKDc7UtRy1U3k5xTNvhY9MRVEBXWTfhio6xzLk", "reward_fee_fraction":{"numerator": 5, "denominator": 100},"code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="buildlinks.shardnet.near" --amount=30 --gas=300000000000000
![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p3.png)

**质押**

    near call buildlinks.factory.shardnet.near deposit_and_stake --amount 3000 --accountId buildlinks.shardnet.near --gas=300000000000000

![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p4.png)

**取消质押**

    near call buildlinks.factory.shardnet.near unstake '{"amount": "10"}' --accountId buildlinks.shardnet.near --gas=300000000000000
![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p5.png)

**发布新提案并更新质押余额**

    near call buildlinks.factory.shardnet.near ping '{}' --accountId buildlinks.shardnet.near --gas=300000000000000
![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p6.png)

## 检查节点状态
**查看日志**

    journalctl -n 100 -f -u neard | ccze –A

**检查状态**

    sudo apt install curl jq
    curl -s http://127.0.0.1:3030/status | jq .version

![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p7.png)

**检查委托⼈和质押情况**

    near view buildlinks.factory.shardnet.near get_accounts '{"from_index": 0,"limit": 10}' --accountId buildlinks.shardnet.near

![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p8.png)

**节点监控页面**

[http://34.102.40.151:3030/status](http://34.102.40.151:3030/status)

![](https://github.com/Ricknow/Near-Stake-War-Challenges/blob/main/challenge5/imgs/p9.png)

