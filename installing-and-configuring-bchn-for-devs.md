# Installing and setting up your Bitcoin Cash Node

**First install the required dependencies to be able to run add-apt-repository**

```
apt-get install -y software-properties-common
```

**Then add the Bitcoin Cash Node repository**

```
sudo add-apt-repository ppa:bitcoin-cash-node/ppa
```

```
sudo apt-get update
```

**Install your Bitcoin Cash Node full node**

```
sudo apt-get install bitcoind bitcoin-qt
```

__**Before doing anything else**__, make sure you set up your Bitcoin configuration file to be compatible with most development use-cases. If this file is not created by default, you can continue, if it does exist, make sure the new configuration you add does not conflict with the existing one, and replace where needed.

**Create your bitcoin.conf file**

```
vim $HOME/.bitcoin/bitcoin.conf
```

In it, paste and modify the following to your liking. If you're not experienced with this kind of thing, leave as-is.

```
dbcache=4000

# Must set txindex=1 so Bitcoin keeps the full index
txindex=1

# [rpc]
# Accept command line and JSON-RPC commands.
server=1

# Choose a strong password here
rpcuser=root
rpcpassword=bitcoin

# If you want to allow remote JSON-RPC access
rpcallowip=0.0.0.0/0

# [wallet]
disablewallet=1

# [ZeroMQ]
# ZeroMQ messages power the realtime BitDB crawler
# so it's important to set the endpoint
zmqpubrawtx=tcp://127.0.0.1:28332
zmqpubhashblock=tcp://127.0.0.1:28332

# Some use-cases makes heavy use of JSON-RPC so it's set to a higher number
rpcworkqueue=512
```

**Now we're ready to start syncing our Bitcoin Cash Node.**

```
bitcoind --daemon
```

To watch how far it's gotten, you can run `bitcoin-cli getblockchaininfo`
