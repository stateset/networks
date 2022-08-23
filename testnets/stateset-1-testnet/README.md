# Stateset - stateset-1-testner Testnet

The plan for this testnet is to simulate stateset `v0.0.1`

## First!

1. Stop your node
2. Reset `statesetd unsafe-reset-all`
3. Remove genesis `rm .stateset/config/genesis.json`
4. Remove gentxs `rm -r .stateset/config/gentx/`
5. Follow generate gentx as normal below

**Genesis File**

[Genesis File](/stateset-1-testnet/genesis.json):

```bash
   curl -s  https://raw.githubusercontent.com/stateset/networks/testnets/main/stateset-1-testnet/genesis.json > ~/.stateset/config/genesis.json
```

**statesetd version**

```bash
$ statesetd version
latest-8e35a4d3
```

## Setup

**Prerequisites:** Make sure to have [Golang >=1.17](https://golang.org/).

#### Build from source

You need to ensure your gopath configuration is correct. If the following **'make'** step does not work then you might have to add these lines to your .profile or .zshrc in the users home folder:

```sh
nano ~/.profile
```

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

Source update .profile

```sh
source .profile
```

```sh
git clone https://github.com/stateset/core
cd core
git checkout v1.0.0
make build && make install
```

This will build and install `statesetd` binary into `$GOBIN`.

Note: When building from source, it is important to have your `$GOPATH` set correctly. When in doubt, the following should do:

```sh
mkdir ~/go
export GOPATH=~/go
```

### Minimum hardware requirements

- 8-16GB RAM
- 100GB of disk space
- 2 cores

## Setup validator node

Below are the instructions to generate & submit your genesis transaction

### Generate genesis transaction (gentx)

1. Initialize the Stateset directories and create the local genesis file with the correct chain-id:

   ```bash
   statesetd init <moniker-name> --chain-id=stateset-1-testnet
   ```

2. Create a local key pair:

   ```sh
   > statesetd keys add <key-name>
   ```

3. Add your account to your local genesis file with a given amount and the key you just created. Use only `10000000000ustate`, other amounts will be ignored.

   ```bash
   statesetd add-genesis-account $(statesetd keys show <key-name> -a) 10000000000ustate
   ```

4. Create the gentx, use only `9000000000ustate`:

   ```bash
   statesetd gentx <key-name> 9000000000ustate --chain-id=stateset-1-testnet
   ```

   If all goes well, you will see a message similar to the following:

   ```bash
   Genesis transaction written to "/home/user/.stateset/config/gentx/gentx-******.json"
   ```

### Submit genesis transaction

- Fork [the networks repo](https://github.com/stateset/networks) into your Github account

- Clone your repo using

  ```bash
  git clone https://github.com/<your-github-username>/networks
  ```

- Copy the generated gentx json file to `<repo_path>/networks/testnets/stateset-1-testnet/gentx/`

  ```sh
  > cd testnets
  > cp ~/.stateset/config/gentx/gentx*.json ./stateset-1-testnet/gentx/
  ```

- Commit and push to your repo
- Create a PR onto https://github.com/stateset/networks
- Only PRs from individuals / groups with a history successfully running nodes will be accepted. This is to ensure the network successfully starts on time.