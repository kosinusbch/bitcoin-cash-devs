## Bitcoin Cash Developers

Somewhat curated list of all the best Bitcoin Cash development resources, including hosting, domain registration, libraries, learning resources and API's. Everything an aspiring to proffessional BCH developer would need to know to start the next big illegal blockchain piracy site or perhaps a cute on-chain animal collectibles platform.

* [Learning Resources](#learning-resources)
* [Useful API's](#useful-apis)
* [Block Explorers](#block-explorers)
* [Domain Registration](#domain-registration)
* [Hosting Platforms](#hosting-platforms)

## Learning Resources

### [Setting up a BitDB Fountainhead node on a brand new Ubuntu server](installing-bitdb-on-ubuntu.md)

> A complete walkthrough of setting up BitDB fountainhead and all it's dependencies.

### [Installing and setting up BCHN for developers](installing-and-configuring-bchn-for-devs.md)

> This guide will walk you through setting up BCHN on Ubuntu and configuring it for developer use-cases.

### [Installing NodeJS, NPM & PM2](installing-nodejs-npm-and-pm2.md)

> This guide will walk you through installing NodeJS, NPM and PM2 on a Ubuntu server.

## Useful API's

### [BitDB Fountainhead](https://bitdb.fountainhead.cash/explorer)

>Probably the only API you'll ever need to develop on Bitcoin Cash. Indexes every part of a Bitcoin Cash transaction down to individual pushdata and makes it searchable through a simple web-based API. Supports a very powerful query language so you can manipulate both the queries and the result with the full power of [mongodb and jq](https://docs.fountainhead.cash/docs/query_v3).

### [Bitsocket Fountainhead](https://bitsocket.fountainhead.cash/channel)

>Supports the BitDB query language, but listens for new transactions and sends them out using SSE (Server-Sent Events) which is supported in all major browsers. Perfect for use-cases such as wallet software, payment screens, blockchain explorers and visualizers. Transactions are sent from the server within miliseconds of them hitting the network and are considered visually instant by the user.

### [SLPDB & SLPSocket](https://slpdb.fountainhead.cash)

>Works similarly to BitDB Fountainhead, but specializes in SLP transactions. It has also switched to human-readable column names and has a number of additional features including a database of UTXO and much more. The perfect API for integrating SLP support into your app, wallet, exchange or any other use-cases involving Simple Ledger Protocol tokens.
>
>[SLPDB](https://slpdb.fountainhead.cash) - [SLPSocket](https://slpsocket.fountainhead.cash/channel)

### [BCHD gRPC](https://bchd.cash/)

>BCHD is a full node integration that is built for developers first. It integrates a very high performance RPC endpoint made with [gRPC](https://www.grpc.io/), a technology built by a team at Google in golang with the goal of great performance, great scalability. As it's a new technology, it's currently not well supported, especially in front-end enviorments, but great if you need speed and power.

### [Bitcoin.com Rest](https://rest.bitcoin.com)

>A rest endpoint ran by bitcoin.com that can be used to get information on addresses, blocks, the network, mining, transactions, simple ledger tokens and even cashaccounts.

### [Blockchair API](https://github.com/Blockchair/Blockchair.Support/blob/master/API_DOCUMENTATION_EN.md)

>Blockchair is a very powerful block explorer on it's own, and as is their API. Comes with a number of limitations however, including the need to apply for an API key and a 30rpm rate-limit for non-commercial use cases. For commercial use-cases such as a site with ads, you need to purchase a "Premium" key,

### [BTC.com API](https://bch.btc.com/api-doc#API)

>BTC.com provides a good general purpose API. Not as feature-full as some of the others, but still a solid API overall

## Block Explorers

### [Bitcoin.com Explorer](https://explorer.bitcoin.com)

>Supports Bitcoin Cash, Bitcoin and to varying degrees SLP tokens. Definetively one of the most well-funded explorers out there, which is really shown in the quality design and UX that the user is prompted with. Easy to get around, easy to find your way back and supports all of the features that you expect out of a block explorer.

### [SimpleLedger.info](https://simpleledger.info)

>By far one of the most powerful explorers out there for both SLP tokens and normal Bitcoin Cash transactions. It allows you to search by transaction id, address, token names, tickers and even cashaccounts. Once you get to the page you're looking for it will display detailed information, graphs and statistics.
>
> It relies soley on SLPDB and BitDB for it's data while everything else is done in the browser, so downtime is pretty much non-existant when it comes to this one.

### [Blockchair](https://blockchair.com/bitcoin-cash)

>Supports Bitcoin Cash and a varying degree of smaller coins and ERC-20 tokens. User interface is a bit cluttered, but that is made up for by the features it provides such as the ability to sort by pretty much anything, hide and show specific columns and even the ability to notify you once a transaction has received a new confirmation.

### [Blockdozer](https://www.blockdozer.com/)

>A block explorer powered by the [Bitprim](https://bitprim.com/) full node. Supports everything you'd imagine it would and has a very minimalistic and un-cluttered user interface.

### [BTC.COM Block Explorer](https://bch.btc.com)

>Bitcoin Cash, Bitcoin and Ethereum block explorers. Features all the things you'd imagine it does in addition to network statistics such as hashrate, coin distribution, block-sizes, transaction fees, difficulty and a few useful metrics.

### [ViaExplorer](https://explorer.viabtc.com/)

>Mainly focused on the Chinese market as you can tell by their font, features all the standard features you'd expect from a block explorer in addition to 16 different metrics such as transactions per day, pool distribution, miner profitability, tx's per block, active addresses list and much more.

### [Explorer.cash](https://explore.cash)

> Fast and efficient blockchain explorer, powered by the BCHD fullnode.

### [Bitcoinunlimited Explorer](https://explorer.bitcoinunlimited.info)

> Simple and open-source blockchain explorer powered by Bitcoin Unlimited.

## Domain Registration

### [Njalla](https://njal.la)

>A service founded by Peter Sunde, PirateBay co-founder. Ignores all but the most serious takedown notices and keeps your domains safe and information private. Unlike other registrars njalla claims "ownership" of the domain, so any lawsuits or takedown notices will be directed to 1337 LLC in a lawyers worst nightmare, Saint Kitts and Nevis.
>
>It's not the cheapest service out there, but makes up for it by being the internet equivilant of a no-extradition country with absolute privacy. Credits your balance after 1 confirmation.

### [Namecheap](https://namecheap.com)

>Good for cheap domains. If you expect to be a bit of an inconvenient customer or receive more than a few takedown notices a month you can expect to get your domains suspended pretty fast though. Credits your balance at 0 confirmations.

### [Epik](https://www.epik.com/)

>Known for allowing a lot of edgy free-speech platforms such as 8chan, patriots.win and gab.com when no one else wanted to. They recently had a pretty bad breach, but as long as you enter fake whois info and pay using cryptocurrency you should be entirely unaffected even if the same thing happens in the future.
