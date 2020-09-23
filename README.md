# Uniswap Interface

[![Tests](https://github.com/Uniswap/uniswap-interface/workflows/Tests/badge.svg)](https://github.com/Uniswap/uniswap-interface/actions?query=workflow%3ATests)
[![Styled With Prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)](https://prettier.io/)

An open source interface for Uniswap -- a protocol for decentralized exchange of Ethereum tokens.

- Website: [uniswap.org](https://uniswap.org/)
- Interface: [app.uniswap.org](https://app.uniswap.org)
- Docs: [uniswap.org/docs/](https://uniswap.org/docs/)
- Twitter: [@UniswapProtocol](https://twitter.com/UniswapProtocol)
- Reddit: [/r/Uniswap](https://www.reddit.com/r/Uniswap/)
- Email: [contact@uniswap.org](mailto:contact@uniswap.org)
- Discord: [Uniswap](https://discord.gg/Y7TF6QA)
- Whitepaper: [Link](https://hackmd.io/C-DvwDSfSxuh-Gd4WKE_ig)

## Accessing the Uniswap Interface

To access the Uniswap Interface, use an IPFS gateway link from the
[latest release](https://github.com/Uniswap/uniswap-interface/releases/latest), 
or visit [app.uniswap.org](https://app.uniswap.org).

## Listing a token

Please see the
[@uniswap/default-token-list](https://github.com/uniswap/default-token-list) 
repository.

## Development

### Install Dependencies

```bash
yarn
```

### Run

```bash
yarn start
```

### Configuring the environment (optional)

To have the interface default to a different network when a wallet is not connected:

1. Make a copy of `.env` named `.env.local`
2. Change `REACT_APP_NETWORK_ID` to `"{YOUR_NETWORK_ID}"`
3. Change `REACT_APP_NETWORK_URL` to e.g. `"https://{YOUR_NETWORK_ID}.infura.io/v3/{YOUR_INFURA_KEY}"` 

Note that the interface only works on testnets where both 
[Uniswap V2](https://uniswap.org/docs/v2/smart-contracts/factory/) and 
[multicall](https://github.com/makerdao/multicall) are deployed.
The interface will not work on other networks.

## Contributions

**Please open all pull requests against the `master` branch.** 
CI checks will run against all PRs.

## Accessing Uniswap Interface V1

The Uniswap Interface supports swapping against, and migrating or removing liquidity from Uniswap V1. However,
if you would like to use Uniswap V1, the Uniswap V1 interface for mainnet and testnets is accessible via IPFS gateways 
linked from the [v1.0.0 release](https://github.com/Uniswap/uniswap-interface/releases/tag/v1.0.0).

## Changes required for V1 interface deployment on Oasis Demo
Remove the `MigrateBannerSmall`, `MigrateBannerLarge`, `VersionToggle` and `TestnetWrapper` from the `src/components/Header/index.js` file.

Change the URL of Uniswap endpoint from default to `http://uniswap.oasiseth.org`, also in the `src/components/Header/index.js` file.

Update the `getEtherscanLink` function in the `src/utils/index.js` file to look like the following

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

Alter the `src/contexts/Tokens.js` page to remove Ethereum mainnet tokens and then add Oasis demo tokens i.e. add token address and V1 exchange address.

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
