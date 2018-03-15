# Private-Ethereum

The private-ethereum project contains a few utility scripts for running your private ethereum chain. Provided you have installed [geth](https://geth.ethereum.org/downloads/).

The easy way of running a blockchain node on our private chain is by using the predefined scripts:
```shell
$ ./scripts/init.sh        # only the first time
$ ./scripts/run.sh         # every time you wish to run a blockchain node
$ ./scripts/reset.sh       # deletes your current chain and starts a new one
```

## Using the pre-funded wallet
This repository comes with the following wallets that both have 500 Ether:

1.  `42f072e76bdebc8f55a371565fee13d293c8696f` with password `""` (empty string)
2.  `5ad91bf68720b9281824df87680c0db60ee843ef` with the password that is hardcoded as the default password in the stemapp project (see the WalletFactory class)

Wallet 2 also has about 0.5 Ether on the Ropsten testnet at the moment of writing. If you need more, use [this faucet](http://ipfs.b9lab.com:8080/ipfs/QmTHdYEYiJPmbkcth3mQvEQQgEamFypLhc9zapsBatQW7Y/throttled_faucet.html).

## Manually running geth
To setup a private blockchain you need to initialize a blockchain with a new genesis block. This repository already contains such a block which can be modified to your requirements. 

```shell
$ geth --datadir data/ init genesis.json
```

`datadir` is used to determine the location where the blockchain data is stored. This directory can be used by your first blockchain node as its blockchain directory.

### Starting an Ethereum Node

```shell
$ geth --datadir data/ --nodiscover --identity "mainNode" --networkid 9351 --rpcapi="eth, web3, personal" --rpc --rpcaddr "0.0.0.0" --rpccorsdomain "*"
```

Notice some additional flags:
* `nodiscover` is used to keep other nodes from automatically connecting with your node (manual connection should be possible)
* `identity` gives a pseudonym to your node so it is more easily recognizable (instead of using a long address)
* `rpc` opens up the possibility to talk with this node. The additional RPC options let the node listen to RPC commands from any address (so be careful not to leave an account with real Ether unlocked!)
* `networkid` refers to the id of your chain, which is defined in the genesis block. Were the default value '1' used, your Ethereum node would connect with the original ethereum blockchain. In the default genesis block, this id is set to '9351'.
* `rpcapi` determines the visible API calls for this node (to be used by a DAPP).

The easiest way to interact with your running geth node is to open a second terminal and run the `geth attach` command.
