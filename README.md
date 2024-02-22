* EigenLayer Node validator setup :

1. Introduction :
EigenLayer is a re-staking protocol that has received funding of over $64.4 million from various top VCs like Coinbase, polychain, hack vc, blockchain capital and more
Since Ethereum transitioned to Proof of Stake (POS) in September 2022, numerous liquid staking protocols have emerged. In practice, these protocols allow participants to engage in staking and receive a tokenized version of their stake, making the process more liquid.

The protocol supports staking tokens from liquid staking projects such as Lido’s stETH, RocketPool’s rETH, and Coinbase’s cbETH. By doing so, users earn additional returns on their ETH deposited for staking.You can participate in EigenLayer’s testnet as a restaker, operator, or AVS developer.

You can find the official documentation here :

https://docs.eigenlayer.xyz/operator-guides/operator-installation

To complete the third step, which involves having your AVS validator node, you need to have a sufficient amount of restaked Geth to be among the top 60 validators. This won’t be possible for most of us, but you can interact with the testnet by restaking your Geth and deploying your operator
To set up your masternode, you have the choice of hosting it on your own computer or opting for a Virtual Private Server (VPS), which is ideal for hosting websites, applications, or other online services, such as nodes.

In my case, I opted for Contabo, a reputable VPS rental solution. The EigenLayer testnet node requires intermediate storage capacity.

So, I recommend you to choose the Cloud VPS 2

2. Restakers
The first step to interact with the Eigenlayer testnet is to restake your gETH. But to do so, you must first obtain Goerli ETH in your EVM wallet. You can connect to Eigenlayer with the wallet you will create on Eigenlayer (in this case, start with step 2 of the guide, then return to this step).

To acquire Goerli ETH, you can gradually claim them from faucets such as https://goerlifaucet.com/ or directly bridge them using LayerZero: https://testnetbridge.com/
After receiving your gETH, you will need to swap them. You can use Tabio.finance or Uniswap DEX to swap some stETH (ETH staked with Lido) or Ankr to stack your gETH.

I advise you to use Tabio, which has better liquidity than Uniswap. Go to their website, click on Swap, and swap some stETH. And then, visit https://goerli.eigenlayer.xyz/token to restake your stETH.


3. API Service :
Now it’s time to create an account on an API service dedicated to the Ethereum Goerli blockchain. Since Eigenlayer is a Layer 2 network on the Ethereum Goerli blockchain, your node must be able to communicate with the Ethereum Goerli layer to ensure its proper functioning.

By following this essential step, you ensure a seamless integration between Eigenlayer and Ethereum Goerli, promoting a smooth user experience and optimal participation in the Eigenlayer network.

You can use Free RPC like ankr, blockpi or alchemy.


4. Installation of Essential Components:
Before diving into the installation of your node, it is crucial to update your VPS. To do this, simply execute the following command in your VPS terminal:

4.1. open your terminal or Putty and connect to your contabo ip address
4.2. copy paste all the command below one by one. do not modify the code if you not understand! just copy and paste it!

- Start by refreshing the local package index to incorporate the most recent modifications made in the repositories.

sudo apt-get update && sudo apt-get upgrade -y

- Now, proceed to install Docker.
  sudo apt install docker.io

  sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

  sudo chmod +x /usr/local/bin/docker-compose

  docker-compose --version

- You will also need to install the Go language. Start by downloading the Go folder (version 1.24.4 here) :
  wget https://golang.org/dl/go1.21.4.linux-amd64.tar.gz

  tar -C /usr/local -xzf go1.21.4.linux-amd64.tar.gz

  export PATH=$PATH:/usr/local/go/bin

  go version
-  Eigenlayer Folder Installation ;

   git clone https://github.com/Layr-Labs/eigenlayer-cli.git
   
   cd eigenlayer-cli
   
   mkdir -p build
   
   go build -o build/eigenlayer cmd/eigenlayer/main.go

   cp ./build/eigenlayer /usr/local/bin/

   eigenlayer

- Creation of Your Private Keys :
 You can generate your ECDSA and BLS encrypted private keys using the command-line interface (CLI).
  Replace <YOUR_NAME> with the name you want to give to your keys : example eigenlayer operator keys create --key-type ecdsa cryptoboys27

  eigenlayer operator keys create --key-type ecdsa <YOUR_NAME>

  eigenlayer operator keys create --key-type bls <YOUR_NAME>

After creating a password for your key, you will see your private key and your wallet address displayed. Store your private key securely, as you won’t be able to access it later.
If you need to retrieve your public keys and wallet address :

 eigenlayer operator keys list

- import wallet
  You will need to import the private key of your ECDSA wallet to your metamask wallet. Don’t forget to transfer some Geth to it to interact with the blockchain later.

- Creation of Your Configuration File

   eigenlayer operator config create

   nano metadata.json

  {
  "name": "<OPERATEUR_NAME>",
  "website": "<WEBSITE>",
  "description": "<DESCRIPTION>",
  "logo": "<https://www.example.com/logo.png>",
  "twitter": "<YOUR_TWITTER>"
  }
  
  Modify the metadata.json file with your data.
  . For the logo, you will need to find a logo in the format (http(s) + .png) using postimages or imgbb
  . For the website, you can use your preferred site

- open your github or create new account:
  . After creating your GitHub account, create a repository.
  
  ![image](https://github.com/aswin9393/EigenLayer/assets/36094932/0b746524-ee01-48cb-af9a-65ae81c10e11)
  
  . create any name of your project : example : eigenlayer
  
  ![image](https://github.com/aswin9393/EigenLayer/assets/36094932/33f398cc-95fe-41db-a836-d87681a983a3)
  
  . And create your METADATA file
  
  ![image](https://github.com/aswin9393/EigenLayer/assets/36094932/ba088d92-b4e9-44f7-9ca2-449018bd9d15)
  
  . Name it ‘metadata.json’, and paste the following text with your info into it:
   {
  "name": "<OPERATEUR_NAME>",
  "website": "<WEBSITE>",
  "description": "<DESCRIPTION>",
  "logo": "<https://www.example.com/logo.png>",
  "twitter": "<YOUR_TWITTER>"
  }
  . After completing your file, click on Raw, and paste the link into your ‘operator.yaml’ file.
  
  ![image](https://github.com/aswin9393/EigenLayer/assets/36094932/6f75b959-ef69-4936-be03-b751d5cfb903)

  - Edit the ‘operator.yaml’ file :
    nano operator.yaml

    . Edit your file with the following data:
   Replace <YOUR_ADDRESS> with the ECDSA Ethereum address you generated 
   Replace <URL_RAW_GITHUB> with the URL of your GitHub RAW
   Replace <API_GOERLI> with an API provider from the following list: https://www.alchemy.com/list-of/rpc-node-providers-on-ethereum
   Replace <NAME_WALLET> with the name you gave to your wallet

   operator:
  address: <your_ADRESSE>
  earnings_receiver_address: <your_ADRESSE>
  delegation_approver_address: "0x0000000000000000000000000000000000000000"
  staker_opt_out_window_blocks: 0
  metadata_url: <URL_RAW_GITHUB>
  el_delegation_manager_address: 0x1b7b8F6b258f95Cf9596EabB9aa18B62940Eb0a8
  eth_rpc_url: https://rpc.ankr.com/eth_goerli
  private_key_store_path: /root/.eigenlayer/operator_keys/<NAME_WALLET>.ecdsa.key.json
  signer_type: local_keystore
  chain_id: 5
  
  . You can close the file by pressing CTRL+X, then Y, and ENTER.

- Registration
 However, to cover the gas fees on Goerli, you will need some Geth in your generated wallet.
 . Once you have Geth in your wallet, initiate the registration :

 eigenlayer operator register operator.yaml

 . To verify that your operator has been successfully registered :

 eigenlayer operator status operator.yaml
. You can also view your Operator on the Eigenlayer website!
 https://goerli.eigenlayer.xyz/operator
 
 <img width="1512" alt="image" src="https://github.com/aswin9393/EigenLayer/assets/36094932/57f5b496-1f6b-4cb9-b44b-fa0e9b7164b5">

 - Delegate your restaked token :
   after restaking your stETH or other lst into https://goerli.eigenlayer.xyz/token you cand now go to operator tab , search you node name or your node address here https://goerli.eigenlayer.xyz/operator then click Delegate

   <img width="1512" alt="image" src="https://github.com/aswin9393/EigenLayer/assets/36094932/41d4e984-1a55-4862-a0a3-3d7c3ae0f1f5">

- Done . congratulation you have run your first Eigenlayer Node. 







  
