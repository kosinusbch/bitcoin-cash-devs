# Setting up a BitDB Fountainhead node on a brand new Ubuntu server

Reading all this text is fully optional, you can copy paste the commands below in the exact order they're shown and it should work fine, but keep in mind the **text in bold is bold for a reason**.

## Overview

- [1 Prerequisite](#overview)
  - [1a Installing NodeJS, NPM & PM2](#step-1a---installing-nodejs-npm--pm2)
  - [1b Installing MongoDB](#step-1b---installing-mongodb)
  - [1c Installing & setting up Bitcoin ABC](#step-1c---installing-and-setting-up-your-bitcoin-abc-node)
- [2 Install and sync Bitd](#step-2---install-and-sync-bitd)
- [3 Setting up Bitserve](#step-3---setting-up-bitserve-a-front-end-api-for-bitdb)
- [4 Setting up Sockserve](#step-4---setting-up-sockserve-a-streaming-output-of-new-transactions-and-blocks)
- [5 Setting up a nginx reverse proxy](#step-5---setting-up-nginx-to-work-as-a-reverse-proxy-for-your-bitserve-and-bitsocket)
- [6 Conclusion](#conclusion)

## Step 1a - Installing NodeJS, NPM & PM2

The standard nodejs version that Ubuntu installs by default is usually pretty outdated. As of writing this the LTS release of NodeJS is 10.16.0. You can verify this yourself at [nodejs.org](https://nodejs.org) and change the first number accordingly. Always use LTS when working with BitDB to avoid unforeseen issues.

**Install dependencies and set your desired NodeJS version**

```
sudo apt-get install curl software-properties-common
```
```
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
```

**Install NodeJS and NPM**

```
sudo apt-get install nodejs
```

**Verify installation**

```
nodejs -v
>v10.16.0

npm -v
>6.9.0
```

**Install PM2 globally**

```
npm install pm2 -g
```

## Step 1b - Installing MongoDB

**Install MongoDB**

```
sudo apt install -y mongodb
```

**Verify installation**

```
sudo systemctl status mongodb
```

^ If this does not say `Active: active (running)`, run `sudo systemctl start mongodb`

## Step 1c - Installing and setting up your Bitcoin ABC node

For the purpose of this guide we will be using Bitcoin ABC, although any other node software compatible with the Bitcoin ABC RPC endpoint will work perfectly fine.

**First install the required dependencies to be able to run add-apt-repository**

```
apt-get install -y software-properties-common
```

**Then add the [Bitcoin-ABC](https://launchpad.net/~bitcoin-abc/+archive/ubuntu/ppa) launchpad repository**

```
sudo add-apt-repository ppa:bitcoin-abc/ppa
```

```
sudo apt-get update
```

**Install your Bitcoin ABC full node**

```
sudo apt-get install bitcoind bitcoin-qt
```

__**Before doing anything else**__, make sure you set up your Bitcoin configuration file to be compatible with BitDB. If this file is not created by default, you can continue, if it does exist, make sure the new configuration you add does not conflict with the existing one, and replace where needed.

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

# BitDB makes heavy use of JSON-RPC so it's set to a higher number
rpcworkqueue=512
```

**Now we're ready to start syncing our Bitcoin ABC node.**

```
bitcoind --daemon
```

To watch how far it's gotten, you can run `bitcoin-cli getblockchaininfo`

**Wait for bitcoind to finish before moving on to step 2.**

## Step 2 - Install and sync Bitd

Bitd is the software that fetches blockchain data from your full node and imports it into MongoDB

**Clone the bitd repository:**
```
git clone https://github.com/fountainhead-cash/bitd.git && cd bitd
```

**Install dependencies:**
```
npm install
```

**Configure bitd: If you changed your RPC password/username earlier, change it here as well.**
```
cp .env.example .env
```
```
vim .env
```

**Start bitd:**
```
pm2 start index.js --name="bitd" --node-args="--max_old_space_size=4096"
```

**Note:** `--max_old_space_size=` (in MB) can be lowered, but if you have the hardware for it, we recommend you keep it here.

**Check Progress**
```
pm2 logs bitd
```

If you had any issues here, please report to [our telegram group](https://t.me/fountainhead-cash) and we can help you out with anything that may have gone wrong


## Step 3 - Setting up Bitserve: a front-end API for BitDB

**Clone the Bitserve repository:**
```
git clone https://github.com/fountainhead-cash/bitserve.git && cd bitserve
```

**Install dependencies:**
```
npm install
```

**Configure Bitserve:**

```
cp .env.example .env
```
```
vim .env
```

**Start Bitserve**
```
pm2 start index.js --name="bitserve"
```

If you had any issues here, please report to [our telegram group](https://t.me/fountainhead-cash) and we can help you out with anything that may have gone wrong



## Step 4 - Setting up Sockserve: a streaming output of new transactions and blocks

**Clone the Sockserve repository:**
```
git clone https://github.com/fountainhead-cash/sockserve.git && cd sockserve
```

**Install dependencies:**
```
npm install
```

**Configure Sockserve:**
```
cp .env.example .env
```
```
vim .env
```

**Start Sockserve**
```
pm2 start index.js --name="sockserve"
```


If you had any issues here, please report to [our telegram group](https://t.me/fountainhead-cash) and we can help you out with anything that may have gone wrong

## Step 5 - Setting up Nginx to work as a reverse proxy for your bitserve and bitsocket

**Installing nginx**
```
sudo apt-get install nginx
```

**Setting up your server block**
```
vim /etc/nginx/sites-available/bitdb
```

**Paste the following in and**

```
server {
	listen 80;
	listen [::]:80;
	server_name __MY_DOMAIN_HERE__;
	index index.html;
        
	location ~ ^/(s|channel)(.*?) {
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $http_host;
		proxy_set_header X-NginX-Proxy true;
		proxy_pass http://127.0.0.1:3001;
		proxy_redirect off;
	}
	location / {
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $http_host;
		proxy_set_header X-NginX-Proxy true;
		proxy_pass http://127.0.0.1:3000;
		proxy_redirect off;
	}
}
```

**Create a symbolic link to sites-enabled**

```
sudo ln -s /etc/nginx/sites-available/bitdb /etc/nginx/sites-enabled/
```

**Test configuration**

```
sudo nginx -t
```

**If it displays no errors, go ahead and restart your server**

```
sudo service nginx restart
```

## Conclusion

Your server should now be available at `http://__MY_DOMAIN_HERE__`

Sockserve can be found at `http://__MY_DOMAIN_HERE__/channel`

If you opted not to use nginx, you can access bitserve at `127.0.0.1:3000` and sockserve at `127.0.0.1:3001`
