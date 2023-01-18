# How to setup blockscout with elysium

```
sudo apt -y update && sudo apt -y upgrade
```

## Add erlang repos
```
# go to your home dir
cd ~
# download deb
wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb
# download key
wget https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc
# install repo
sudo dpkg -i erlang-solutions_2.0_all.deb
# install key
sudo apt-key add erlang_solutions.asc
# remove deb
rm erlang-solutions_2.0_all.deb
# remove key
rm erlang_solutions.asc
```

## Add NodeJS repo
```
sudo curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```

## Env variables for elysium testnet
```
ETHEREUM_JSONRPC_VARIANT="parity"
ETHEREUM_JSONRPC_HTTP_URL="https://elysium-testnet-rpc.vulcanforged.com/"
ETHEREUM_JSONRPC_TRACE_URL="https://elysium-testnet-rpc.vulcanforged.com/"
DATABASE_URL='postgresql://postgres:blockscout@localhost:5432/blockscout?ssl=false'
SECRET_KEY_BASE="TnfDvU8A6glp1dFdbjAtqgYxy2oFTyw6ITF1g0b/6IoX68rfVuJZCUYwpLuGV9pB"
CHAIN_ID=1338
HEART_COMMAND="systemctl restart explorer"
NETWORK="Elysium"
SUBNETWORK="Testnet" 
LOGO="/images/Elysium-Scan-Compact-small.svg"
LOGO_FOOTER="/images/Elysium-Scan-Compact-small.svg"
COIN="lava"
COIN_NAME="LAVA"
INDEXER_DISABLE_BLOCK_REWARD_FETCHER="true"
INDEXER_DISABLE_PENDING_TRANSACTIONS_FETCHER="true"
INDEXER_DISABLE_INTERNAL_TRANSACTIONS_FETCHER="true"
BLOCKSCOUT_PROTOCOL="http"
PORT=4000
BLOCKSCOUT_VERSION=5.0.0
LINK_TO_OTHER_EXPLORERS=false
HIDE_BLOCK_MINER=true
DISABLE_EXCHANGE_RATES=true
WEBAPP_URL=https://elysium-explorer-blockscout.vulcanforged.com/
JSON_RPC="https://elysium-testnet-rpc.vulcanforged.com/"
SUPPORTED_CHAINS='[ { "title": "Elysium Mainnet", "url": "https://elysium-blockscout.vulcanforged.com/" }, { "title": "Elysium Testnet", "url": "https://elysium-explorer-blockscout.vulcanforged.com/", "test_net?": true }]'
LINK_TO_OTHER_EXPLORERS=false
```
