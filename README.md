# Blockchain

## Activity

[台中前端社群 7/28 徹底瞭解區塊鏈 從私有鏈出發 @夢森林](https://taichung-frontend.kktix.cc/events/180728-blockchain)

## Blog

[BlockChain - 私有鏈建立 ( Geth & Node.js )](https://dotblogs.com.tw/explooosion/2018/07/29/172031)

# Geth

## Installation

1. Download [Geth](https://geth.ethereum.org/downloads/)
2. Install Geth for Windows

Check versions:

```bash
geth -h
```

## Generate

```bash
geth --datadir data init genesis.json
```

## Start

```bash
geth --datadir data --networkid 123456 --rpc --rpccorsdomain "*" --nodiscover --rpcapi="db,eth,net,web3,personal" console
```

### Create Account

```bash
personal.newAccount()
```

```bash
personal.newAccount("123456")
```

### View Info

```
admin.nodeInfo
```

### Worker miner

```bash
# cpu 
miner.start(1)
```

```bash
# failed in Windows
miner.stop()
```

### Look up default miner account

```bash
eth.coinbase
```

### Change default miner

```bash
miner.setEtherbase(eth.accounts[1])
```

Or set by address:

```bash
miner.setEtherbase("0x5de1753525553737be6b17cec3451d548dbdfdb4")
```

### Look up Account address

```bash
eth.accounts[0]
```

### Look up Account money

```bash
eth.getBalance(eth.accounts[0])
```

Or:

```bash
web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
```

### Unlock Account (before using money)

```bash
personal.unlockAccount(eth.accounts[0])
```

### Transfer money

```bash
eth.sendTransaction({from:eth.accounts[0],to:"0x1222ca94a9064039ac2e892d2ee7ecf955019c0b",value:web3.toWei(3,"ether")})
```

Or:

```bash
eth.sendTransaction({from:eth.accounts[0],to:eth.accounts[1],value:web3.toWei(3,"ether")})
```

Transaction status

```bash
txpool.status
```

### Look up block info

```bash
eth.blockNumber
```

```bash
eth.getTransaction("0x0c59f431068937cbe9e230483bc79f59bd7146edc8ff5ec37fea6710adcab825")
```

```bash
eth.getBlock(1)
```

# eth-netstats (Dashboard View)

Ethereum Network Stats [https://ethstats.net/](https://github.com/cubedro/eth-netstats).

## Installation

### Clone project

```bash
git clone https://github.com/cubedro/eth-netstats.git
```

```bash
cd eth-netstats
```

```bash
npm install
```

### Install grunt-tool

```bash
npm install -g grunt-cli
```

### Deploy by grunt

```bash
grunt
```

### Websocket setting

Create file `ws_secret.json`:

```json
{
    "WS_SECRET": "update"
}
```

### Run server

```bash
npm start
```

# eth-net-intelligence-api (Record Transfer)

Ethereum Network Intelligence API [http://tinyurl.com/ofndjbo](https://github.com/cubedro/eth-net-intelligence-api).

## Installation

### Clone project

```bash
git clone https://github.com/cubedro/eth-net-intelligence-api.git
```

```bash
cd eth-net-intelligence-api
```

```bash
npm install
```

### Install process manager

[PM2](https://github.com/Unitech/pm2) is a Production Runtime and Process Manager for Node.js applications with a built-in Load Balancer.

```bash
npm install -g pm2
```

### Modify app.json

```json
[
{
"name" : "node-app",
"script" : "app.js",
"log_date_format" : "YYYY-MM-DD HH:mm Z",
"merge_logs" : false,
"watch" : false,
"max_restarts" : 10,
"exec_interpreter" : "node",
"exec_mode" : "fork_mode",
"env":
{
"NODE_ENV" : "production",
"RPC_HOST" : "localhost",
"RPC_PORT" : "8545",
"LISTENING_PORT" : "30303",
"INSTANCE_NAME" : "marginchain",
"CONTACT_DETAILS" : "",
"WS_SERVER" : "http://localhost:3000",
"WS_SECRET" : "update",
"VERBOSITY" : 3
}
```

### Modify lib/node.js

PR from [Ensure results is always an array #242](https://github.com/cubedro/eth-net-intelligence-api/pull/242/commits/d4e2fd3999566a77019373c7556a4b282310e1fa)

```js
// var interv = {};
var interv = [];
```

```js
// results = false;
results = [];
```

### Start server

Run in current terminal.

```bash
npm start
```

Run in background with pm2.

```
pm2 start app.json
```

## Manage PM2

### Look up pm2 status

```bash
pm2 list
```

### Logs from pm2

```bash
pm2 log
```

### Restart pm2

By name:

```bash
pm2 restart node-app
```

By id:

```bash
pm2 restart 0
```

### Stop pm2

By name:

```
pm2 stop node-app
```

By id:

```bash
pm2 stop 0
```

### Kill in pm2 list

```bash
pm2 kill
```

# Truffle

The most popular Ethereum development framework [http://truffleframework.com](https://github.com/trufflesuite/truffle).

## Installation

```bash
npm install -g truffle
```

## Create project

```bash
mkdir hello
```

```bash
cd hello
```

```bash
truffle init
```

## Contracts

### Create

Create file `contracts/HelloWorld.sol`:

```sol
pragma solidity ^0.4.23;

contract HelloWorld {

  function sayHello() public returns (string) {
      return ("Hello World");
  }
}
```

### Compile

```bash
truffle compile
```

Or:

```bash
cd contracts
```

```bash
truffle compile
```

### Migrations

Create file `migrations/2_deploy_contracts.js`:

```js
var HelloWorld = artifacts.require("HelloWorld");

module.exports = function (deployer) {
    deployer.deploy(HelloWorld);
}
```

### Setting Network

Modify file `truffle.js`:

```js
module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // to customize your Truffle configuration!

  networks: {
    development: {
      host: "localhost",
      port: "8545",
      network_id: "*", // Match any network id
    },
  },
};
```

### Unlock Account

Before depoy contracts, we need to unlock account

```bash
personal.unlockAccount(eth.accounts[0], "123456", 1500)
```

Or:

```bash
personal.unlockAccount(eth.accounts[0])
```

### Migrate

PS: **You should keep in mind before running script.**

```bash
miner.start(1)
```

### Deploy

```bash
truffle migrate
```

Or:

```bash
cd migrations
```

```bash
truffle migrate
```
