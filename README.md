# Building a Private Blockchain using Ethereum

## Geth Installation for Windows
```url
https://geth.ethereum.org/
```
Note: Tick all the boxes during installation.
## Invoking Installed Geth in cmd
```cmd
geth console
```

## Creating a new Folder Ethereum
```cmd
mkdir go-ethereum
cd go-ethereum
mkdir node1
mkdir node2
mkdir bnode
```

## Creation of New Accounts
### Account 1
```cmd
cd node1
geth --datadir "./data" account new
```
### Account 2
```cmd
cd node2
geth --datadir "./data" account new
```

## Create Info.txt
### Primarily used for storing info such as Account ID, passwords, and enode.

## Verifing the ChainID
```url
https://chainlist.org/
```
Note: ChainId must be unique or else our <b>Private Blockchain could merge</b> with Public Blockchain Network.

## Genesis Block Code
```json
//Genesis_File

{
  "config": {
    "chainId": { CHAIN_ID },
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "berlinBlock": 0,
    "clique": {
      "period": 5,
      "epoch": 30000
    }
  },
  "difficulty": "1",
  "gasLimit": "8000000",
  "extradata": "0x0000000000000000000000000000000000000000000000000000000000000000{ INITIAL_SIGNER_ADDRESS }0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "alloc": {
    "{ FIRST_NODE_ADDRESS }": { "balance": "{ ETHER_AMOUNT }" },
    "{ SECOND_NODE_ADDRESS }": { "balance": "{ EHTER_AMOUNT }" }
  }
}

```

## Initializing Accounts with Genesis File
```cmd
geth --datadir ./data init ../genesis.json
```

## Initializing of BootNode
```cmd
cd bnode
```
### Generation of BootKey
```cmd
bootnode -genkey boot.key
bootnode -nodekey boot.key -verbosity 7 -addr "127.0.0.1:30302"
``` 
Note: Copy and save it to info.txt <b>enode value from</b> cmd.

## Initializing Nodes in the Blockchain
### Signer Node
```cmd
geth --datadir "./data"  --port 30304 --bootnodes enode://{ YOUR_VALUE } --authrpc.port 8547 --ipcdisable --allow-insecure-unlock  --http --http.corsdomain="https://remix.ethereum.org" --http.api web3,eth,debug,personal,net --networkid { NETWORK_ID } --unlock { ADDRESS_NODE1 } --password { PASSWORD_FILE_NAME_EXTENSION }  --mine --miner.etherbase= { SIGNER_ADDRESS }
```
Note: Store the password in Ethereum\node1\data\password.txt

### Sender Node
```cmd
geth --datadir "./data"  --port 30306 --bootnodes enode://{ YOUR_VALUE }  -authrpc.port 8546 --networkid { NETWORK_ID } --unlock { ADDRESS_NODE2 } --password { PASSWORD_FILE_WITH_EXTENSION }
```
Note:  Store the password in Ethereum\node2\data\password.txt

## Remix
```url
https://remix.ethereum.org/
```

## Conduting a Simple Peer-to-peer Transfer
```solidity
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.19;

contract New{
	string name;

	function setName(string memory _name)public {
		name=_name;
	} 
	function getName()public view returns(string memory){
		return name;
	}
}
```
