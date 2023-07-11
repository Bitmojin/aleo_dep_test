# Aleo Smart Contract Deployment

This README will guide you through the process of deploying a smart contract on the Aleo network.

## Pre-requisites

Before starting, make sure you have a Ubuntu server available, or Ubuntu desktop terminal.

## Step 1: Get Aleo Tokens

First, you need to get some Aleo tokens. You can do this via SMS as follows:

Text **"+1-867-888-5688"** with the message: **"send 50 credits to 'your-aleo-address' "**.

You will receive an SMS containing a transaction URL. Keep this handy as you'll need the "Record" value from this in the final step.

## Step 2: Install Required Software

On your Ubuntu machine, run the following commands one-by-one in terminal:

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustup install stable
rustup update stable
rustup default stable
git clone https://github.com/AleoHQ/leo
cd leo
apt install clang gcc libssl-dev pkg-config
cargo install --path .
git clone https://github.com/AleoHQ/snarkOS.git --depth 1
cd snarkOS
./build_ubuntu.sh
cargo install --path .

## Step 3: Install Wallet and Deploy App

cd $HOME
mkdir demo_deploy_Leo_app && cd demo_deploy_Leo_app
WALLETADDRESS="your-wallet-address"
APPNAME=helloworld_"${WALLETADDRESS:4:6}"
leo new "${APPNAME}"
PATHTOAPP=$(realpath -q $APPNAME)
cd $PATHTOAPP && cd ..
PRIVATEKEY="your-private-key"

### Go back to the transaction URL from your SMS and find the path:
### executions>transitions>outputs>0: type:"record", value:"recordxxxx"
### Copy the record value value and use it in the Aleo Tools website's Record tab along with your view key to get your record.
### Enter this value below in place of "".

RECORD=""
snarkos developer deploy "${APPNAME}.aleo" --private-key "${PRIVATEKEY}" --query "https://vm.aleo.org/api" --path "./${APPNAME}/build/" --broadcast "https://vm.aleo.org/api/testnet3/transaction/broadcast" --fee 600000 --record "${RECORD}"
