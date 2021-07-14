
<h1 align="center">
  <br>
  <a href="http://www.amitmerchant.com/electron-markdownify"><img src="logo_xchain.png" alt="Markdownify" width="200"></a>
  <br>
  XChainPy
  <br>
</h1>

<h4 align="center">A python library with a common interface built for simple and fast interaction with <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_bitcoin">Bitcoin</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_ethereum">Ethereum</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_bitcoincash">Bitcoin Cash</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_litecoin">Litecoin</a>, <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_binance">Binance Chain</a> and <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_thorchain">Thorchain</a>.</h4>

<p align="center">
  <a href="#description">Description</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#packages-and-usage">Packages and Usage</a> •
  <a href="https://github.com/xchainjs/xchainpy-lib/issues">Report Issue</a> •
  <a href="https://github.com/xchainjs/xchainpy-lib/issues">Request Feature</a> •
  <a href="https://t.me/xchainpy">Telegram</a>
</p>

## Description

XChainPy relies upon a <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#readme">commmon interface</a> which enables the following functions for all supported chains:
* Initialization with a <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#set-phrase">BIP39 phrase</a> (can be used cross-chain)
* Get <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#get-balance">balance</a> and <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#get-transactions">transaction history</a> for a given account (default is index 0)
* <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#transfer">Transfer</a> funds
* Built-in <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client#get-fees">fee recommendations</a>

XChainPy also enables <a href="#chain-specific-packages">chain specific interactions</a> such as writing/reading smart contracts on Ethereum.

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

## Packages and Usage

What follows is a short overview of packages and an illustration of basic usage through some simple test cases.

### Cross-chain packages
#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_crypto">XChainPy-Crypto</a>

Create a cross-chain BIP39 keystore from a passphrase. A encrypt and decrypt passphrase test case:

```python
import pytest
from xchainpy_crypto import crypto

class TestCrypto:

    phrase = 'flush viable fury sword mention dignity ethics secret nasty gallery teach fever'
    
    @pytest.mark.asyncio
    async def test_import_keystore(self):
        password = 'thorchain'
        keystore = await crypto.encrypt_to_keystore(self.phrase , password)
        decrypted = await crypto.decrypt_from_keystore(keystore , password)
        assert decrypted == self.phrase
```

See additional <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_crypto/tests/test_crypto.py">XChainPy-Crypto test cases</a>.

#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_client">XChainPy-Client</a>

A generalized interface for crypto wallet clients, it is used by the chain specific implementations. Clients are initialized with a mnemonic passphrase.
Below is a test case for initializing a client with a passphrase and checking balance of default account, test case uses <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_binance/tests/test_binance_clients.py">Binance Chain client</a>.

```python
import pytest
from xchainpy_binance.client import Client

class TestBinanceClient:

    phrase = 'rural bright ball negative already grass good grant nation screen model pizza'

    @pytest.mark.asyncio
    async def test_has_balances(self):
        self.client = Client(self.phrase, network='mainnet')
        assert await self.client.get_balance()
```

See chain specific packages for additional implementation examples.

### Chain specific packages
#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_bitcoin">XChainPy-Bitcoin</a>
Bitcoin module for XChainPy clients. Test case for a Bitcoin transfer with a memo and using a "fast" fee rate:
```python
import pytest
from xchainpy_bitcoin.client import Client

class TestBitcoinClient:

    phrase = 'caution pear excite vicious exotic slow elite marble attend science strategy rude'
    testnetaddress_for_tx2 = 'tb1qymzatfxg22vg8adxlnxt3hkfmzl2rpuzpk8kcf'
    memo = 'SWAP:THOR.RUNE'

    @pytest.mark.asyncio
    async def test_transfer_with_memo_and_fee_rate(self):
        self.client = Client(self.phrase, network='testnet')
        fee_rates = await self.client.get_fee_rates()
        fee_rate = fee_rates['fast']
        balance = await self.client.get_balance()
        if balance.amount > 0:
            amount = 0.0000001
            tx_id = await self.client.transfer(amount, self.testnetaddress_for_tx2, self.memo, fee_rate)
            assert tx_id
```
For additonal usage examples see <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_bitcoin/tests/test_bitcoin_clients.py">Bitcoin test cases</a>.

#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_ethereum">XChainPy-Ethereum</a>
Ethereum module for XChainPy clients. Below are two test cases. First test case gets Eth balance of a specific Ethereum address. Second test case illustrates how to call a function from an Ethereum smart contract:
```python
import pytest
from xchainpy_ethereum.client import Client

class TestClient:
    phrase = 'interest useless business glide shy verb draft bring novel initial appear grab wasp message apology welcome divert celery what predict poem infant benefit sketch'
    network = 'wss://mainnet.infura.io/ws/v3/048d536048f5410cb17411edbdb4ef2c' # get from https://infura.io/
    eth_api = 'HUV9WP6T1MDPTPZDFHY2HVX73I89YKVWXQ' # get from https://etherscan.io/ 
    test_address = "0xB1aA026725cD51700E674E240D971785F95429FD"
    thor_token_address = "0x3155BA85D5F96b2d030a4966AF206230e46849cb"

    @pytest.mark.asyncio
    async def test_get_balance(self):
        self.client = Client(phrase=self.phrase, network=self.network, network_type="mainnet",
                             ether_api=self.eth_api)
        balance = await self.client.get_balance()
        balance_address = await self.client.get_balance(address=self.test_address)
        assert balance == balance_address

    @pytest.mark.asyncio
    async def test_get_contract(self):
        self.client = Client(phrase=self.phrase, network=self.network, network_type="mainnet",
                             ether_api=self.eth_api)
        token_contract = await self.client.get_contract(self.thor_token_address, erc20=True)
        assert token_contract.functions.symbol().call() == 'RUNE'
```

Checkout additional Ethereum test cases at <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_ethereum/tests/test_mainnet_client.py">Ethereum test cases</a>.

#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_thorchain">XChainPy-Thorchain</a>
Thorchain module for XChainPy clients. Test case for getting Thorchain balance and transfering THOR.RUNE:

```python
import pytest
from xchainpy_util.asset import Asset
from xchainpy_thorchain.client import Client
class TestClient:

    phrase = 'history dice polar glad split follow tired invest lemon mask all industry'
    testnetTransfer = 'tthor1pttyuys2muhj674xpr9vutsqcxj9hepy4ddueq'
    transfer_amount = 0.01
    rune_asset = Asset('THOR', 'RUNE', 'RUNE')

    @pytest.mark.asyncio
    async def test_should_broadcast_transfer(self, client):
        self.client = Client(self.phrase, network='testnet')
        before_balance = await self.client.get_balance() # get account balance
        before_balance_amount = before_balance[0]['amount']
        await self.client.transfer(amount=self.transfer_amount, recipient=self.testnetTransfer, asset=self.rune_asset) # transfer 0.01 THOR.RUNE to self.testnetTransfer
        after_balance = await self.client.get_balance()
        after_balance_amount = after_balance[0]['amount']
```

See additional Thorchain test cases at <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_thorchain/tests/test_clients.py">Thorchain test cases</a>. 

#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_binance">XChainPy-Binance Chain</a>
Binance Chain module for XChainPy clients. Test case for getting Binance Chain balance and transfering BNB.BNB:

```python
import pytest
from xchainpy_binance.client import Client
from xchainpy_util.asset import Asset

class TestBinanceClient:

    phrase = 'wheel leg dune emerge sudden badge rough shine convince poet doll kiwi sleep labor hello'
    bnb_asset = Asset('BNB', 'BNB', 'BNB')
    transfer_amount = 0.0001
    testnetaddressForTx = 'tbnb1t95kjgmjc045l2a728z02textadd98yt339jk7'

    @pytest.mark.asyncio
    async def test_should_broadcast_transfer(self, client):
        self.client = Client(self.phrase, network='testnet')
        assert self.client.get_address() == self.testnetaddressForTx
        before_balance = await self.client.get_balance()
        before_balance_amount = before_balance[0].amount
        await self.client.transfer(asset=self.bnb_asset, amount=self.transfer_amount, recipient=self.testnetaddressForTx)
        after_balance = await self.client.get_balance()
        after_balance_amount = after_balance[0].amount
        assert round((float(before_balance_amount) - float(after_balance_amount)) * 10**8) == round((await self.client.get_fees())['average'] * 10 ** 8) # only difference should be tx fees, as transfer was sent from self.testnetaddressForTx to self.testnetaddressForTx  
```

See more Binance Chain test cases at <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_binance/tests/test_binance_clients.py">Binance Chain test cases</a>.

#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_litecoin">XChainPy-Litecoin</a>
Litecoin module for XChainPy clients. Test case for Litecoin transfer with a memo using a "fast" fee rate:

```python
import pytest
from xchainpy.xchainpy_litecoin.client import Client

class TestLiteCoinClient:

    phrase = 'atom green various power must another rent imitate gadget creek fat then'
    address_two = 'tltc1ql68zjjdjx37499luueaw09avednqtge4u23q36'
    memo = 'SWAP:THOR.RUNE' 

    @pytest.mark.asyncio
    async def test_transfer_with_memo_and_fee_rate(self, client):
        self.client = Client(self.phrase, network='testnet')
        fee_rates = await self.client.get_fee_rates()
        fee_rate = fee_rates['fast']
        balance = await self.client.get_balance()
        if balance.amount > 0:
            amount = 0.0000001
            tx_id = await self.client.transfer(amount, self.address_two, self.memo, fee_rate)
            assert tx_id
```

See additional Litecoin test cases at <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_litecoin/tests/test_litecoin_clients.py">Litecoin test cases</a>. 

#### <a href="https://github.com/xchainjs/xchainpy-lib/tree/main/xchainpy/xchainpy_bitcoincash">XChainPy-Bitcoin Cash</a>
  
Bitcoin Cash module for XChainPy clients. Test case for Bitcoin Cash transfer with a memo using a "fast" fee rate:

```python
import pytest
from xchainpy_bitcoincash.client import Client

class TestBitcoincashClient:

    phrase = 'atom green various power must another rent imitate gadget creek fat then'
    memo = 'SWAP:THOR.RUNE'

    @pytest.mark.asyncio
    async def test_transfer_with_memo_and_fee_rate(self, client):
        self.client = Client(self.phrase, network='testnet')
        fee_rates = await self.client.get_fee_rates()
        fee_rate = fee_rates['fast']
        balance = await self.client.get_balance()
        if balance.amount > 0:
            amount = 0.00000001
            tx_id = await self.client.transfer(amount, 'bchtest:qzt6sz836wdwscld0pgq2prcpck2pssmwge9q87pe9', self.memo, fee_rate)
            assert tx_id
```

See additional Bitcoin Cash test cases at <a href="https://github.com/xchainjs/xchainpy-lib/blob/main/xchainpy/xchainpy_bitcoincash/tests/test_bitcoincash_clients.py">Bitcoin Cash test cases</a>.

<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
XChainPy is a python version of <a href="https://github.com/xchainjs/xchainjs-lib">XChainJS</a> and this readme is very much inspired by the content there. 