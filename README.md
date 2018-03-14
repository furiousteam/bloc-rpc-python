turtlecoin-walletd-rpc-python
=============================

A Python wrapper for the TurtleCoin walletd JSON-RPC interface

Requires Python 3.6

**In development**

TODO:

* send_transaction: prevent payment_id being sent when extra is set
* create_address: creating address from a spend key doesnt work
* Implement Fusion transactions (send/estimate)
* Add documentation (sphinx)
* Add tests

Quickstart
----------

You need to have walletd running with rpc enabled:

```
# generate a test wallet
$ ./walletd -w test.wallet -p any_password_you_want -g

# launch walletd with test wallet
$ ./walletd -w test.wallet -p any_password_you_want --local --rpc-password test
```

Usage
-----

Example for creating and deleting an address:

```python
import turtlecoin

wallet = TurtleCoinWallet(password='test')

address = wallet.create_address()

wallet.get_balance(address)
wallet.get_view_key()
wallet.get_spend_keys(address)

wallet.delete_address(address)
```

Sending a transaction:

```python
wallet = TurtleCoinWallet(password='test')

# transfer 10.00 TRTL
recipients = [{'address': destination_address, 'amount': 1000}]

# mixin/anonymity is 3 and fee is 0.1 TRTL
wallet.send_transaction(
    anonymity=3,
    transfers=recipients,
    fee=10
)
'aebb47d5a975f0ac0c27806b7abf9107adbd7a8a0c7c8ea91ca363eacda7f79x'
```

Sending a transaction will return a transactionHash. You can enter the hash on the https://turtle-coin.com block explorer to see more details.

Delayed Transactions
--------------------

```python
# Create a delayed transaction
>>> wallet.create_delayed_transaction(
    anonymity=3,
    transfers=[
        {
            'address': 'TRTL...',
            'amount': 50
        }
    ]
)
'bfcc4735a975f0ac0c27806b7abf9107adbd7a8a0c7c8ea91ca363eacda7f79x'

# List all delayed transactions
>>> wallet.get_delayed_transaction_hashes()
['bfcc4735a975f0ac0c27806b7abf9107adbd7a8a0c7c8ea91ca363eacda7f79x']

# Send delayed transaction
>>> wallet.send_delayed_transaction('bfcc4735a975f0ac0c27806b7abf9107adbd7a8a0c7c8ea91ca363eacda7f79x')
```

Developer setup
---------------

Install dependencies with pipenv:

```
pipenv install
```
