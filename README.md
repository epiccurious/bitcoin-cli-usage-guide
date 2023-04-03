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

#### First, install the dependencies

```bash
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
```

#### Then, clone the Bitcoin Core repository

```bash
git clone https://github.com/bitcoin/bitcoin
cd bitcoin/
```

#### Finally, perform the build process

```
## Change to the most recent version number.
git checkout $(git tag | sort -V | grep -vE "rc|test|final" | tail -1) 

## Auto-generate the configure script.
./autogen.sh

## Check the dependencies and create the Makefile.
./configure

## Build Bitcoin Core using all CPU cores.
make -j "$(($(nproc)+1))"

## Check the integrity of the build.
make -j "$(($(nproc)+1))" check
```

#### Optionally, run the functional tests

```
python3 test/functional/test_runner.py --extended
```

## macOS

### Option 1: Download from bitcoincore.org/en/download.

### Option 2: Build from Source

#### First, install the dependencies and clone the repository

```bash
## Install the Xcode command-line utilities.
xcode-select --install

## Install brew, a third-party package manager.
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

## Install dependencies
brew install automake berkeley-db@4 boost libevent libtool pkg-config qrencode qt@5
```

#### (Optional/Privacy) Install and run tor

```bash
## Install tor
brew install tor torsocks

## Start the tor service
brew services start tor

## Verify that tor is both "Running" and "Loaded". You should see a response like the following:
## <usere>@<computer> ~ % brew services info tor
## tor (homebrew.mxcl.tor)
## Running: ✔
## Loaded: ✔
## Schedulable: ✘
## User: <user_name>
## PID: <process_id>
## <user>@<computer> ~ % 
brew services info tor
```

#### Then, clone the Bitcoin Core repository

There are many different options for cloning.

We'll go through a few, each followed by a `cd` ("change directory") to change directories into the cloned repository directory.

- Basic clone (into a directory with the same name as the repository):
  ```bash
  git clone https://github.com/bitcoin/bitcoin.git
  cd bitcoin/
  ```
  
- You don't actually have to include the `.git` extension, for example:
  ```bash
  git clone https://github.com/bitcoin/bitcoin
  cd bitcoin/
  ```

- If you set up Tor, adding `torsocks ` before the git clone command for added privacy. However, macOS won't let you use the standard `git` due to Apple's System Integrity Protection. We need to install a second copy of `git` using brew, for example:
  ```bash
  ## Use brew to install an alternate copy of git
  brew install git
  
  ## Clone the repository by running git with torsocks
  torsocks /opt/homebrew/Cellar/git/*/bin/git clone https://github.com/bitcoin/bitcoin
  cd bitcoin/
  ```

- If you have limited network bandwidth or storage, add `--single-branch --depth 1` to save space by cloning only the `master` branch with no history, for example:
  ```bash
  ## Clone the master branch no history. In my test, the amount of data transferred is reduced over 90% from about 120 MB to 11MB.
  git clone --single-branch --depth 1 https://github.com/bitcoin/bitcoin.git
  cd bitcoin/
  ```

- Clone into a different directory by passing in an additional argument, for example:
  ```bash
  git clone https://github.com/bitcoin/bitcoin.git projects/bitcoin/
  cd projects/bitcoin/
  ```
  
- Combining all of these into one command, we can clone the main branch with no history over tor into a specific directory:
  ```bash
  torsocks /opt/homebrew/Cellar/git/*/bin/git clone --single-branch --depth 1 https://github.com/bitcoin/bitcoin projects/bitcoin/
  cd projects/bitcoin/
  ```

#### Perform the build process

```bash
## Change to the most recent version number.
git checkout $(git tag | sort -V | grep -vE "rc|test|final" | tail -1) 

## Auto-generate the configure script.
./autogen.sh

## Check the dependencies and create the Makefile. Tell the compiler to ignore warnings in external libraries used by Bitcoin Core.
./configure --enable-suppress-external-warnings

## Build (compile) Bitcoin Core. Speed up this step by using all CPU cores with the `-j` argument. For example, if `sysctl -n hw.physicalcpu` returns 8, use ` -j 8`.
make -j "$(($(sysctl -n hw.physicalcpu)+1))"

## (Optional/Educational) Verify that the binaries we will use exist in the src/ directory.
ls src/{bitcoind,bitcoin-cli,qt/bitcoin-qt}

## (Optional/Educational) Check the build using all of the CPU cores.
make check -j "$(($(sysctl -n hw.physicalcpu)+1))"

## (Optional/Educational) Run the extended functional tests to make sure everything is working properly. This optional step will take a long time.
python3 test/functional/test_runner.py --extended
```

#### (Optional/Educational) Install Bitcoin Core

You don't have to actually install Bitcoin Core, but it makes things easier by not having to refernce the `src/` directory every time you want to use `bitcoin-cli`.

```
## Copy the binaries to /usr/local/bin/ and additional support files to other directories.
## If you're curious, pay attention to the output from this command to see where these files are being installed.
sudo make install

## Look for the binaries in the /usr/local/bin/ directory.
ls /usr/local/bin/bitcoin*
```

#### Start up Bitcoin Core and test the command-line interface

```bash
## Run the GUI version of Bitcoin Core with server mode enabled.
## Note: After running this command, press `return` to get a prompt again.
bitcoin-qt --server & disown

## Test the cli
bitcoin-cli getblockchaininfo
```

#### (Optional/Educational) Delete the source code and build files

```bash
cd ../
rm -rf bitcoin/
```

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
