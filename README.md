
<h1 align="center">
  <br>
  <a href="http://www.amitmerchant.com/electron-markdownify"><img src="logo_xchain.png" alt="Markdownify" width="200"></a>
  <br>
  xchainpy
  <br>
</h1>

<h4 align="center">A python library with a common interface built for simple and fast interaction with <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_bitcoin">Bitcoin</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_ethereum">Ethereum</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_bitcoincash">Bitcoin Cash</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_litecoin">Litecoin</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_binance">Binance Chain</a> and <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_thorchain">Thorchain</a>.</h4>

<p align="center">
  <a href="#description">Description</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#usage">Usage</a> •
  <a href="#Packages">Packages</a> •
  <a href="https://github.com/xchainjs/xchainpy-lib/issues">Report Issue</a> •
  <a href="https://github.com/xchainjs/xchainpy-lib/issues">Request Feature</a> •
  <a href="https://t.me/xchainpy">Telegram</a>
</p>

## Description

xchainpy relies upon a <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#readme">commmon interface</a> which enables the following functions for all supported chains:

* Initialization with a <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#set-phrase">BIP39 phrase</a> (can be used cross chain)
* Get <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#get-balance">balance</a> and <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#get-transactions">transaction history</a> for a given account (default is index 0)
* <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#transfer">Transfer</a> funds
* Built-in <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#get-fees">fee recommendations</a>

xchainpy also enables chain specific interactions such as writing/reading smart contracts on ethereum, are available within the chain specific packages. See <#usage">usage examples</a>


## Getting Started

Install individual packages using <a href="https://pypi.org/search/?q=xchainpy">PyPi</a> e.g.:
```bash
# pip install ethereum module
$ pip install xchainpy-ethereum
```

Clone from command line:

```bash
# Clone this repository
$ git clone https://github.com/xchainjs/xchainpy-lib
```

## Usage

## Packages

<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
xchainpy is a python version of <a href="https://github.com/xchainjs/xchainjs-lib">XChainJS</a> and this readme is very much inspired by the content there. 