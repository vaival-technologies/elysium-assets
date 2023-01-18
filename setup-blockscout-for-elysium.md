# How to setup blockscout with elysium

```
sudo apt -y update && sudo apt -y upgrade
```

# setup postgres using docker-compose
```
version: "3"
services:
  postgres_db:
    image: postgres:14
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_DB: blockscout
      POSTGRES_PASSWORD: blockscout
    volumes:
      - ./postgres_data:/var/lib/postgresql/data
    ports:
     - "127.0.0.1:5432:5432"
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
sudo apt -y install nodejs
```

## Install Rust and Cargo
```
sudo curl https://sh.rustup.rs -sSf | sh -s -- -y
sudo apt -y install cargo
```

## Install required version of Erlang
```
sudo apt -y install esl-erlang=1:24.*
```

## Install required version of Elixir
```
cd ~
mkdir /usr/local/elixir
wget https://github.com/elixir-lang/elixir/releases/download/v1.13.4/Precompiled.zip
sudo unzip -d /usr/local/elixir/ Precompiled.zip
rm Precompiled.zip
sudo ln -s /usr/local/elixir/bin/elixir /usr/local/bin/elixir
sudo ln -s /usr/local/elixir/bin/mix /usr/local/bin/mix
sudo ln -s /usr/local/elixir/bin/iex /usr/local/bin/iex
sudo ln -s /usr/local/elixir/bin/elixirc /usr/local/bin/elixirc
```

## run elixir -v
```
Erlang/OTP 24 [erts-12.3.1] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [jit]
Elixir 1.13.4 (compiled with Erlang/OTP 22)
```

## other dependecies
```
sudo apt -y install automake libtool inotify-tools gcc libgmp-dev make g++ git
```

## set environment variables
```
export SECRET_KEY_BASE=TnfDvU8A6glp1dFdbjAtqgYxy2oFTyw6ITF1g0b/6IoX68rfVuJZCUYwpLuGV9pB
export DATABASE_URL=postgresql://postgres:blockscout@localhost:5432/blockscout?ssl=false
export ETHEREUM_JSONRPC_VARIANT=parity
export ETHEREUM_JSONRPC_HTTP_URL=https://elysium-testnet-rpc.vulcanforged.com/
export SUBNETWORK=Elysium Testnet
export ETHEREUM_JSONRPC_WS_URL=wss://elysium-testnet-socket.vulcanforged.com/
export COIN_NAME=LAVA
export COIN=LAVA
```

## clone and compile BlockScout
```
git clone https://github.com/vaival-technologies/blockscout.git
cd blockscout
mix deps.get
mix local.rebar --force
mix phx.gen.secret
export MIX_ENV=prod
export SECRET_KEY_BASE="912X3UlQ9p9yFEBD0JU+g27v43HLAYl38nQzJGvnQsir2pMlcGYtSeRY0sSdLkV/"
mix local.hex --force
mix do deps.get, local.rebar --force, deps.compile, compile
mix do ecto.create, ecto.migrate
cd apps/block_scout_web/assets
sudo npm install
sudo node_modules/webpack/bin/webpack.js --mode production
cd ~/blockscout
sudo mix phx.digest
cd apps/block_scout_web
mix phx.gen.cert blockscout blockscout.local
```
## create and run BlockScout service
```
sudo touch /etc/systemd/system/explorer.service
sudo vi /etc/systemd/system/explorer.service
```
The contents of the explorer.service file should look like this:
```
[Unit]
Description=BlockScout Server
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=root
StandardOutput=syslog
StandardError=syslog
WorkingDirectory=/usr/local/blockscout
ExecStart=/usr/local/bin/mix phx.server
EnvironmentFile=/usr/local/blockscout/env_vars.env

[Install]
WantedBy=multi-user.target
```

Enable service
```
sudo systemctl daemon-reload
sudo systemctl enable explorer.service
sudo mv ~/blockscout /usr/local
sudo systemctl start explorer.service
sudo systemctl status explorer.service
sudo journalctl -u explorer.service -f
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
