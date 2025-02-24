# About

This is a cross-chain rebase token project that uses chainlink CCIP protocol. Users can deposit ETH and mint rebase tokens which have a interest rate based on when you mint the tokens, giving rewards over time.

# Getting started

## Requirements
- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
  - You'll know you did it right if you can run `git --version` and you see a response like `git version x.x.x`
- [foundry](https://getfoundry.sh/)
  - You'll know you did it right if you can run `forge --version` and you see a response like `forge x.x.x`

## Quickstart
```
git clone https://github.com/robivagner/cross-chain-rebase-token
cd cross-chain-rebase-token
```

# Usage

## Testing

There are 2 files of testing. The RebaseToken.t.sol file is mostly fuzz tests beside 1 which doesnt need to be a fuzz and the CrossChain.t.sol is basically a big test that bridges tokens over ethereum sepolia to arbitrum sepolia. If you would like to try testing the "testBridgeAllTokens" in the CrossChain.t.sol file you would need to paste your rpc urls for eth sepolia and for arb sepolia(use alchemy's site to get them) between the blank "" in the foundry.toml file.

For running every test
```
forge test
```

If you want to see the coverage of the testing scripts

```
forge coverage
```

# Deployment to a testnet or mainnet

1. Setup environment variables

You'll want to set your `SEPOLIA_RPC_URL`, `PRIVATE_KEY` etc. as environment variables. You can add them to a `.env` file, like this

```
SEPOLIA_RPC_URL=EXAMPLE_URL
ARBITRUM_RPC_URL=EXAMPLE_URL
PRIVATE_KEY=EXAMPLE_PRIVATE_KEY
ETHERSCAN_API_KEY=EXAMPLE_ETHERSCAN_API_KEY
```

Then you can type:

```
source .env
```

to use them in the command line after you saved the .env file.


!!PLEASE do NOT put your actual private key in the .env file it is NOT good practice. 
!!EITHER put the private key of a wallet you won't have actual money in OR use this command to store your private key interactively in a encrypted form:
```
cast wallet import <NAME_OF_ENCRYPTED_PRIVATE_KEY_WALLET> --interactive
```

2. Get testnet ETH

Head over to [faucets.chain.link](https://faucets.chain.link/) or [cloud.google.com](https://cloud.google.com/application/web3/faucet/ethereum/sepolia) and get some testnet ETH. You should see the ETH show up in your metamask.

3. Deploy

To deploy the contracts you can use the scripts created. 

If you would like to just deploy the contract and get some RebaseTokens:

Deploy the 3 contracts using Deployer.s.sol script then use the cast command to interact with the contract (for example):

 Get RebaseTokens
```
cast send <vault-contract-address> "deposit()" --value 0.1ether --rpc-url $SEPOLIA_RPC_URL --wallet
```

 Redeem RebaseTokens for ETH
```
cast send <vault-contractaddress> "redeem(uint256)" 10000000000000000 --rpc-url $SEPOLIA_RPC_URL --wallet
```

If you would like to also bridge tokens:

You would need to go this site [docs.chain.link](https://docs.chain.link/ccip/directory/mainnet) to see all of the information needed to pass on the arguments of the scripts (like router address etc.). First deploy the contracts on both chains of your choice(eth sepolia and arb sepolia maybe) using the the Deployer.s.sol, get the addresses for the contracts, configure the pool using the ConfigurePool.s.sol script with the right arguments (contract addresses and chain selector from the link above) and lastly use the BridgeTokens.s.sol script to bridge the tokens from one chain to another (also use the link above for information like router address etc.).

To get the etherscan api key go to their [site](https://etherscan.io/), sign in, hover over your name and go to api keys. 
Then u can click add, give it a name, copy the API key token and put it in an enviromental variable in the .env file like shown above.

## Estimate gas

You can estimate how much gas things cost by running:

```
forge snapshot
```

And you'll see an output file called `.gas-snapshot`

# Thank you!

This project has taught me some pretty cool ways of how to handle cross-chain bridging which is a pretty interesting topic. I'm happy to share this with more people.