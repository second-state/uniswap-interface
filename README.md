//** Please note this is a draft and this code is under heavy development. Not to be used in production **

# Uniswap 

--- 

## Background
This article provides step-by-step instructions for installing [Uniswap](https://uniswap.org/) V1 on any [Ethereum](https://ethereum.org/en/) compatible blockchain.

Some [Ethereum](https://ethereum.org/en/) compatible blockchains include:
* [ParaTime](https://docs.oasis.dev/general/faq/oasis-network-faq) - built in collaboration by [Oasis Foundation](https://oasisprotocol.org/) & [SecondState](https://www.secondstate.io/) as announced [here](https://medium.com/oasis-protocol-project/ethereum-support-on-the-oasis-blockchain-3add9e13556).
* [CyberMiles](https://www.cybermiles.io/en-us/) - a public blockchain for E-commerce.
* [Ethereum Classic](https://ethereumclassic.org/) - Ethereum (ETH) blockchain network initially released on 30 July 2015, now known as Ethereum Classic (ETC).

## Uniswap V1
[Uniswap V1](https://uniswap.org/docs/v1/), was [launched in November 2018](https://twitter.com/haydenzadams/status/1058376395108376577) on the Ethereum mainnet as a pair of [Vyper](https://vyper.readthedocs.io/en/stable/) smart contracts. Uniswap is an automated liquidity protocol which facilitates token exchanges on the Ethereum blockchain network. Uniswap has seen daily trading volumes of well beyond four hundred million dollars ($400, 000, 000 / day). 

[This tweet](https://twitter.com/haydenzadams/status/1300034164830408704) shows that Uniswap surpasses the daily Coinbase trade volume on occasion.

### Smart contracts
The [Factory](https://github.com/Uniswap/uniswap-v1/blob/master/contracts/uniswap_factory.vy) and [Exchange](https://github.com/Uniswap/uniswap-v1/blob/master/contracts/uniswap_exchange.vy) Vyper smart contracts of Uniswap V1 contain all of the logic for the Uniswap protocol. These smart contracts are permissionless in nature and therefore Uniswap V1 will exist for as long as Ethereum does.

--- 

## Housekeeping

The following updates, upgrades and dependencies are required before we start.
```
sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get install npm
npm install fs
npm install web3
npm install truffle-hdwallet-provider
sudo apt-get install apache2
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
```
Update version of node
```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

--- 

## Cloning this repository

Clone this Uniswap Interface code using Git.
```
cd ~
git clone https://github.com/second-state/uniswap-interface.git
```
Change into the Uniswap Interface directory.
```
cd ~
cd uniswap-interface
```
Checkout the specific V1 release
```
git checkout tags/v1.0.0
```
Now, whilst in the `~/uniswap-interface directory`, clone the "How To Install Uniswap" code using Git.
```
git clone https://github.com/second-state/how_to_deploy_uniswap.git
```
Change into the `how_to_deploy_uniswap/` directory.
```
cd how_to_deploy_uniswap/
```

--- 

## Accounts

Create 3 Ethereum compatible addresses using any method that you are comfortable with i.e. [web3js](https://web3js.readthedocs.io/en/v1.2.11/web3-eth-accounts.html) etc.

Now paste the **private** and **public** keys of those addresses into the installation_data.json file as shown below.
```
vi ~/uniswap-interface/how_to_deploy_uniswap/installation_data.json
```
Here is an example of the `private_key` and `public_key` sections of that file.
```
  "private_key": {
    "alice": "your_new_key_here",
    "bob": "your_new_key_here",
    "charlie": "your_new_key_here"
  },
  "public_key": {
    "alice": "your_new_key_here",
    "bob": "your_new_key_here",
    "charlie": "your_new_key_here"
  }
```
You will need to fund these accounts with network tokens. So depending on your network, please go ahead and send at least 20 network tokens (from a Faucet etc.) to all three of these accounts.

Now place the RPC URL to your Ethereum compatible network in that same `installation_data.json` file.
```
vi ~/uniswap-interface/how_to_deploy_uniswap/installation_data.json
```
Here is an example of the `rpc_endpoint` section of that file.
```
"provider": {
	"rpc_endpoint": "http://rpc_url:port"
}
```

--- 

## Deploying Uniswap V1 smart contracts

Now run the Uniswap V1 smart contract installer.
```
cd ~/uniswap-interface/how_to_deploy_uniswap/uniswap_v1
```
```
node deploy_uniswap_v1.js
```
If you open the `../installation_data.json` file, you will see that the Uniswap V1 contract addresses and the Alice and Bob (ERC20 and ERC20 Exchange) addresses have been automatically filled in.
```
"contract_address": {
    "uniswap_factory": "0x2DF5e651be537bB564005340EA5D8f6fA763b530",
    "weth": "",
    "uniswap_exchange_template": "0x68Fc886B0ca3D65AE8Ad21Fde01d8C4E2AD9d86c",
    "alice_exchange": "0x4A8f21726434951f5C1baA0F067d50fdA2a297e2",
    "bob_exchange": "0x61d82A90455EC7cDEdF7cF7F5267c0aF6657c626",
    "alice_erc20_token": "0x240Fc9370709bad1F4402186701C76e36a20848b",
    "bob_erc20_token": "0x09cB0AE6dddF68Aaad81b8f6B83c30dfdaA65b48",
    "uniswap_v2": "",
    "multicall": "",
    "migrator": "",
    "router": "",
    "ens_registry": "",
    "unisocks": ""
}
```

---

## Deploying the Uniswap Interface for V1

Please perform the following changes to the source code in order to implement Uniswap Interface V1 for your Ethereum compatible blockchain.

Remove the `MigrateBannerSmall`, `MigrateBannerLarge`, `VersionToggle` and `TestnetWrapper` from the `src/components/Header/index.js` file.

Change the URL of Uniswap endpoint from default to `http://uniswap.oasiseth.org`, also in the `src/components/Header/index.js` file.

Update the `getEtherscanLink` function in the `src/utils/index.js` file, as per the following example. Please note, we are setting the block explorer to `http://52.237.119.138:4000` but you will of course use your own block explorer's URL here.

```
export function getEtherscanLink(networkId, data, type) {
    console.log(networkId);
    const prefix = "http://52.237.119.138:4000"
    switch (type) {
        case 'transaction': {
            return `${prefix}/tx/${data}`
        }
        case 'address':
        default: {
            return `${prefix}/account/${data}`
        }
    }
}
```

Alter the `src/contexts/Tokens.js` page to remove Ethereum mainnet tokens and then add your own demo tokens i.e. the token addresses and exchange addresses which were just written to the `installation_data.json` file when we ran the `node deploy_uniswap_v1.js` command.

```
export const INITIAL_TOKENS_CONTEXT = {
        1: {
            '0x984718904f853A004F145d133dEAb0c1dE50466B': {
                [NAME]: 'Alice Token',
                [SYMBOL]: 'ALICE',
                [DECIMALS]: 18,
                [EXCHANGE_ADDRESS]: '0x5AeC86734172C5C257Eb2a86e705EF375207c5c8'
            },
            '0x61950E81019caDC560036125B262EF0CAa705896': {
                [NAME]: 'Bob Token',
                [SYMBOL]: 'BOB',
                [DECIMALS]: 18,
                [EXCHANGE_ADDRESS]: '0x92570E6E10fa2196952B039c4A59b5e617Eb9A50'
            }
        }
```

Return to the Uniswap Interface directory and then build the application's dependencies.
```
cd ~/uniswap-interface
yarn
```

### Environment variables

Open the `.env` and `.env.production` files and update the `REACT_APP_CHAIN_ID` and the `REACT_APP_NETWORK_URL` to suite your needs.

### Modify hard-coded addresses

Now change into the `~/uniswap-interface/how_to_deploy_uniswap/uniswap_interface` directory.
```
cd ~/uniswap-interface/how_to_deploy_uniswap/uniswap_interface
```
Now run the `modify_addresses.py` script 
```
python3 modify_addresses.py
```

### Change the hard-coded RPC

Open the `change_rpc.py` file for editing.
```
cd ~/uniswap-interface/how_to_deploy_uniswap/uniswap_interface
```

```
vi change_rpc.py
```
Change the values in the `new_rpc` line to suit your needs.
```
infura = "https\:\/\/mainnet\.infura\.io\/v3\/faa4639b090f46499f29d894da0551a0"
new_rpc = "http\:\/\/oasis-ssvm-demo\.secondstate\.io\:8545"
```

**Be sure to escape** `/` and `.` and `:` (as shown above) **because these will break the command when executed**.

Now run this script
```
python3.6 change_rpc.py
```

### Build

Change back to the following directory
```
cd ~/uniswap-interface
```
Now run the following command
```
yarn run build
```
This will generate a new `build` directory as well as some new files, as shown below.
```
  450.29 KB  build/static/js/4.047da443.chunk.js
  276.48 KB  build/static/js/5.5c7ecd7f.chunk.js
  155.14 KB  build/static/js/9.1f17fe1e.chunk.js
  95.38 KB   build/static/js/main.38d70569.chunk.js
  67.14 KB   build/static/js/0.1e3e94eb.chunk.js
  66.33 KB   build/static/js/6.e890ef53.chunk.js
  6.56 KB    build/static/js/1.5f3a35e8.chunk.js
  1.24 KB    build/static/js/runtime-main.c8bb7174.js
  907 B      build/static/css/4.996ad921.chunk.css
  165 B      build/static/js/8.81f9e545.chunk.js
  164 B      build/static/js/7.494e051e.chunk.js
```
You will remember that we just ran the `modify_addresses.py` script. We are now going to run that **again** (but this time, over the build folder, which the above build command just created). This is just to make sure that there are no addresses which relate to the original Uniswap source code (but rather our newly created contract addresses).
```
cd how_to_deploy_uniswap/uniswap_interface/ && python3 modify_addresses.py
```

Now run the `change_rpc.py` again also.
```
python3.6 change_rpc.py
```

### Apache 2 deployment

Now, we return to the Uniswap directory to copy the modified `build` files over to our Apache2 server, where they will be deployed for the end users.
```
cd ../../ && sudo cp -rp build/* /var/www/html/ && sudo /etc/init.d/apache2 restart
```
---

A demo of the above installation can be found at [http://uniswap.oasiseth.org/](http://uniswap.oasiseth.org/)
