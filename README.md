# Bitcoin Core Command Line Interface Usage Guide
Introductory guide to using Bitcoin Core through the command line

This document is under development.

# Overview of Bitcoin Core Binaries

What is a binary?

Difference between a binary and an "app".

What binaries are included with Bitcoin Core? Depends on which OS.

How to find the binaries?

## What Does the bitcoind Binary Do?

## What Does bitcoin-qt Binary Do?

## What Does bitcoin-cli Binary Do?

## Summarizing bitcoind vs bitcoin-qt vs bitcoin-cli

Differences between the bitcoin-qt and bitcoind binaries

https://bitcoin.stackexchange.com/questions/13368/whats-the-difference-between-bitcoind-and-bitcoin-qt-different-commands

# How to Install and Run Bitcoin Core

## Linux

### Option 1: apt install

### Option 2: download from bitcoincore.org/en/download

### Option 3: Build from Source

```bash
FIRST INSTALL THE DEPENDENCIES

## Install the core dependencies
sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3

## Install the second set of dependencies
sudo apt-get install libevent-dev libboost-dev

## Install sqlite (for descriptor wallets)
sudo apt install libsqlite3-dev

## Install the qt graphical user interface dependencies
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools

sudo apt-get install -y build-essential libtool autotools-dev automake pkg-config bsdmainutils python3 libevent-dev libboost-dev libsqlite3-dev libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools

#sudo apt-get install libboost-all-dev

THEN PERFORM THE BUILD PROCESS

./autogen.sh
./configure
make -j "$(($(nproc)+1))"
make check
```

## macOS

### Option 1: Download from bitcoincore.org/en/download.

### Option 2: Build from Source

Open Terminal and run the following commands.

```bash
## Install brew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

## Install dependencies
brew install automake boost libevent libtool pkg-config qt@5

## Clone the repository and change to the directory
git clone https://github.com/bitcoin/bitcoin.git
cd bitcoin/

## Generate the configure script
./autogen.sh

## Create the Makefile
./configure

## Compile the bitcoind, bitcoin-cli, and bitcoin-qt binaries 
make -j "$(($(sysctl -n hw.physicalcpu)+1))" src/{bitcoind,bitcoin-cli,qt/bitcoin-qt}

## (Optional/Educational) Verify that the binaries exist
ls src/{bitcoind,bitcoin-cli,qt/bitcoin-qt}

## (Optional/Educational) Check the build
make check

## Install Bitcoin Core
make install

## (Optional/Educational) Verify the installation
ls /usr/local/bin/bitcoin*

## Run bitcoind
bitcoind --daemonwait

## Test the cli
bitcoin-cli getblockchaininfo
```

## (Optional/Educational) Delete the source code and build files
cd ../
rm -rf bitcoin/

## Windows

Meh.

# Ways to Check if Bitcoin Core is Running

## Look for the app icon

## Look for a Terminal window

## Try Sending an RPC Request

## Try to Run It Again

## Check for the process ID

## Check for the lock file

# Interact with Bitcoin Core with the Command Line

## Use the Integrated RPC Console

## Use Shell/Bash Commands from the Terminal

## Example Script to Check the Initial Sync Status

## Example Script to Check the Prune Status

The following script will tell you how far back your Bitcoin Core blocks data goes.
```bash
## Set "asdf" on the next line to your binary directory
## For example, if your binary is located at ~/bitcoin/bin/,
## set binary_directory to $HOME/bitcoin/bin
binary_directory="$HOME/bitcoin/bin"

## Use bitcoin-cli to find the blockchain info
## Save the response as a variable blockchain_info
blockchain_info="$binary_directory"/bitcoin-cli getblockchaininfo

## asdf

```
