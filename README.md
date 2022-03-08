1. We didn't upload an impage to IPFS ourselves 
2. Why is IPFS decentralized?
3. Anyone can mint an NFT here - not verifiably scarce or random



> Update: You should use `ipfs://` instead of `https://ipfs.io/` for your tokenURI

# nft-mix

<br/>
<p align="center">
<a href="https://chain.link" target="_blank">
<img src="https://raw.githubusercontent.com/PatrickAlphaC/nft-mix/main/img/shiba-inu.png" width="225" alt="NFT Shiba Inu">
<img src="https://raw.githubusercontent.com/PatrickAlphaC/nft-mix/main/img/pug.png" width="225" alt="NFT Pug">
<img src="https://raw.githubusercontent.com/PatrickAlphaC/nft-mix/main/img/st-bernard.png" width="225" alt="NFT St.Bernard">
</a>
</p>
<br/>

This is a repo to work with and use NFTs smart contracts in a python environment, using the Chainlink-mix as a starting point. 


## Prerequisites

Please install or have installed the following:

- [nodejs and npm](https://nodejs.org/en/download/)
- [python](https://www.python.org/downloads/)
## Installation

1. [Install Brownie](https://eth-brownie.readthedocs.io/en/stable/install.html), if you haven't already. Here is a simple way to install brownie.

```bash
pip install eth-brownie
```
Or via pipx
```bash
pip install --user pipx
pipx ensurepath
# restart your terminal
pipx install eth-brownie
```

2. Clone this repo
```
brownie bake nft-mix
cd nft
```

1. [Install ganache-cli](https://www.npmjs.com/package/ganache-cli)

```bash
npm install -g ganache-cli
```

In order to deploy to testnets, do the following. 

4. Set your environment variables

Set your `WEB3_INFURA_PROJECT_ID`, and `PRIVATE_KEY` [environment variables](https://www.twilio.com/blog/2017/01/how-to-set-environment-variables.html). 

You can get a `WEB3_INFURA_PROJECT_ID` with free trial of [Infura](https://infura.io/). Can find your `PRIVATE_KEY` from your ethereum wallet like [metamask](https://metamask.io/). 

You'll also need testnet rinkeby ETH and LINK. You can get LINK and ETH into your wallet by using the [rinkeby faucets located here](https://faucets.chain.link/rinkeby).
You can add your environment variables to the `.env` file:

```
export WEB3_INFURA_PROJECT_ID=<PROJECT_ID>
export PRIVATE_KEY=<PRIVATE_KEY>
```

Then, make sure your `brownie-config.yaml` has:

```
dotenv: .env
```

You can also [learn how to set environment variables easier](https://www.twilio.com/blog/2017/01/how-to-set-environment-variables.html)


Or you can run the above in your shell. 


# Usage

There are 2 types of NFTs here. 
1. `SimpleCollectibles.sol`
2. `AdvancedCollectibles.sol`

They each deploy unique dogs. The advanced version gives you a random breed (out of a Pug, Shiba Inu, and St. Bernard).

The advanced collection uses a [Chainlink VRF](https://docs.chain.link/docs/get-a-random-number) to deploy the random dog. 

You can 100% use the rinkeby testnet to see your NFTs rendered on opensea, but it's suggested that you test and build on a local development network so you don't have to wait as long for transactions. 

### Running Scripts

The simple collectibles work on a local network,  however the advanced requires a testnet. We default to rinkeby since that seems to be the testing standard for NFT platforms. You will need testnet rinkeby ETH and testnet Rinkeby LINK. You can find faucets for both in the [Chainlink documentation](https://docs.chain.link/docs/link-token-contracts#rinkeby). 

# For the Simple ERC721
```
brownie run scripts/simple_collectible/deploy_simple.py --network rinkeby
brownie run scripts/simple_collectible/create_collectible.py --network rinkeby
```

# For the Advanced ERC721

You'll need [testnet Rinkeby](https://faucet.rinkeby.io/) and [testnet LINK](https://rinkeby.chain.link/) in the wallet associated with your private key. 

```
brownie run scripts/advanced_collectible/deploy_advanced.py --network rinkeby
brownie run scripts/advanced_collectible/create_collectible.py --network rinkeby
```
Then:
```
brownie run scripts/advanced_collectible/create_metadata.py --network rinkeby
brownie run scripts/advanced_collectible/set_tokenuri.py --network rinkeby
```

# Verify on Etherscan

The simple contract and the advanced contract can be verified if you just set your `ETHERSCAN_TOKEN`. 

## Misc
There are some helpful scripts in `helpful_scripts.py`.

# Viewing on OpenSea
After running the scripts from the `For the Advanced ERC721` section

1. Create the metadata

Metadata is the URI needed to upload data. You can either:
- Upload to IPFS yourself
- Use the metadata already created when you cloned this repo. 

### If you want to upload to IPFS yourself

Download [IPFS](https://ipfs.io/) 
Set `export IPFS_URL=http://127.0.0.1:5001` and `export UPLOAD_IPFS=true` environment variables
Run the IPFS daemon: `ipfs daemon`
Then Run
```
brownie run scripts/advanced_collectible/create_metadata.py --network rinkeby
```

Alternatively, you could upload the uri manually:
Add the file created in `metadata/rinkeby/NAME.json` to [IPFS](https://ipfs.io/) or [Pinata](https://pinata.cloud/). 
### If you want to use the metadata from this repo

Just run:
```
brownie run scripts/advanced_collectible/create_metadata.py --network rinkeby
```

2. Set the tokenURI 
Run
```
brownie run scripts/advanced_collectible/set_tokenuri.py --network rinkeby
```
And after some time, (you may have to wait up to 20 minutes for it to render on opensea), you should see your NFT on opensea! [It'll look something like this.](https://testnets.opensea.io/assets/0x8acb7ca932892eb83e4411b59309d44dddbc4cdf/0)

## *NEW* Pinata

If you want to auto-upload to pinata instead of IPFS automatically, you can do so by getting a [Pinata API Key.](https://pinata.cloud/documentation#GettingStarted)

You'll need the following environment variables (you can get them from Pinata)
```
PINATA_API_KEY
PINATA_API_SECRET
```
Then run:
```
python scripts/upload_to_pinata.py
```

## Testing

```
brownie test
```

## Linting

```
pip install black 
pip install autoflake
autoflake --in-place --remove-unused-variables -r .
black .
```
