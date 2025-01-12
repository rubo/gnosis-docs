---
description: A quickstart guide for getting up and running with Swarm as quick as possible with minimal additional tooling.
keywords: [swarm, storage, decentralized, decentralised]
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Swarm Quickstart Shell Script

The following is a guide to get you started running a Bee full node with staking on Swarm using [the official shell script provided by Swarm](https://github.com/ethersphere/bee/blob/master/install.sh) which automatically detects your system and installs the correct version of Bee. 


:::warning
Note that we append 127.0.0.1 (localhost) to our Bee API's port (1633 by default), since we do not want to expose our Bee API endpoint to the public internet, as that would allow anyone to control our node. Make sure you do the same, and it's also recommended to use a  firewall to protect access to your node(s).
:::

:::info
The guide below is for a full Bee node with staking. To run a light node (uploads and downloads only), set `--full-node` to false, or to run in ultra light (downloads only) mode you can set both `--full-node` and `--swap-enable` to false.
:::



## Prerequisites

### Hardware

:::warning
If you are running on a home Wi-Fi you may need to configure your router to use [port forwarding](https://www.noip.com/support/knowledgebase/general-port-forwarding-guide) or take other steps to ensure your node is reachable by other nodes on the network. See [here](https://docs.ethswarm.org/docs/bee/installation/connectivity/#navigating-through-the-nat) for more guidance. If you are running on a VPS or cloud based server you will likely have no issues.
:::

:::caution
While it is possible to run multiple Bee nodes on a single machine, due to the high rate of I/O operations required by a full Bee node in operation, it is not recommended to run more than a handful of Bee nodes on the same physical disk (depending on the disk speed). 
:::

* Dual core, recent generation, 2ghz processor
* 4gb RAM
* 30gb SSD
* Stable internet connection 

### Software 

* A computer running a supported version of Linux (almost all commonly used distros should work). macOS will also work but you may need to slightly modify some commands.
* A Gnosis Chain RPC endpoint (either by running your own node or the [free RPC endpoint](https://xdai.fairdatasociety.org) offered from the Fair Data Society. Other free public options are available at the [Gnosis Chain docs](https://docs.gnosischain.com/tools/RPC%20Providers/). 
* (Optional) [jq utility](https://jqlang.github.io/jq/) for formatting API output. 
* (Optional) [bashtop utility] for monitoring processes (such as our Bee node).


:::info
The [`jq` utility](https://jqlang.github.io/jq/) is used in this guide to automatically format the output from the Bee API. It can help make API output much more readable, however it is totally optional. 
:::

### Tokens

* A small amount of xDAI to pay for Gnosis Chain transactions, 0.1 xDAI should be enough
* 10 xBZZ (BZZ on Gnosis Chain) is required for staking 

## Full node setup process 

Run the install shell script using either `curl` or `wget`:

:::caution
In the example below, the version is specified using `TAG=v2.2.0`, make sure that you [check if there is a newer tagged version of Bee](https://github.com/ethersphere/bee/tags) and if so, modify the commands below to use the most recent tag number so that you have the latest version of Bee.
:::


<Tabs
defaultValue="curl"
values={[
{label: 'Curl', value: 'curl'},
{label: 'Wget', value: 'wget'},
]}>

<TabItem value="curl">

```bash
curl -s https://raw.githubusercontent.com/ethersphere/bee/master/install.sh | TAG=v2.2.0 bash
```
</TabItem>
<TabItem value="wget">

**wget**

```bash
wget -q -O - https://raw.githubusercontent.com/ethersphere/bee/master/install.sh | TAG=v2.2.0 bash
```
</TabItem>

</Tabs>
 
Let's check that the script ran properly:

```bash=
bee 
```

If the script ran without any problems you should see this:

```bash=
Ethereum Swarm Bee

Usage:
  bee [command]

Available Commands:
  start       Start a Swarm node
  dev         Start a Swarm node in development mode
  init        Initialise a Swarm node
  deploy      Deploy and fund the chequebook contract
  version     Print version number
  db          Perform basic DB related operations
  split       Split a file into chunks
  printconfig Print default or provided configuration in yaml format
  help        Help about any command
  completion  Generate the autocompletion script for the specified shell

Flags:
      --config string   config file (default is $HOME/.bee.yaml)
  -h, --help            help for bee

Use "bee [command] --help" for more information about a command.
```


### Start node

Let's try starting up our node for the first time with the command below, make sure to pick a [strong password](https://xkcd.com/936/) of your own:

```bash
bee start \
  --password flummoxedgranitecarrot \
  --full-node \
  --swap-enable \
  --api-addr 127.0.0.1:1633 \
  --blockchain-rpc-endpoint https://xdai.fairdatasociety.org
```
:::info

Command explained:

1. **`bee start`**: This is the command to start the Bee node.

2. **`--password flummoxedgranitecarrot`**: The password to decrypt the private key associated with the node. Replace "flummoxedgranitecarrot" with your actual password.

3. **`--full-node`**: This option enables the node to run in full mode, sharing its disk with the network, and becoming eligible for staking.

4. **`--swap-enable`**: This flag enables SWAP, which is the bandwidth incentives scheme for Swarm. It will initiate a transaction to set up the SWAP chequebook on Gnosis Chain (required for light and full nodes).

5. **`--api-addr 127.0.0.1:1633`**: Specifies that the Bee API will be accessible locally only via `127.0.0.1` on port `1633` and not accessible to the public. 

6. **`--blockchain-rpc-endpoint https://xdai.fairdatasociety.org`**: Sets the RPC endpoint for interacting with the Gnosis blockchain (required for light and full nodes).
:::


Logs will begin printing to the terminal, and should look like this:

```bash
Welcome to Swarm.... Bzzz Bzzzz Bzzzz
                \     /
            \    o ^ o    /
              \ (     ) /
   ____________(%%%%%%%)____________
  (     /   /  )%%%%%%%(  \   \     )
  (___/___/__/           \__\___\___)
     (     /  /(%%%%%%%)\  \     )
      (__/___/ (%%%%%%%) \___\__)
              /(       )\
            /   (%%%%%)   \
                 (%%%)
                   !

DISCLAIMER:
This software is provided to you "as is", use at your own risk and without warranties of any kind.
It is your responsibility to read and understand how Swarm works and the implications of running this software.
The usage of Bee involves various risks, including, but not limited to:
damage to hardware or loss of funds associated with the Ethereum account connected to your node.
No developers or entity involved will be liable for any claims and damages associated with your use,
inability to use, or your interaction with other nodes or the software.

version: 2.2.0-06a0aca7 - planned to be supported until 11 December 2024, please follow https://ethswarm.org/

"time"="2024-09-24 18:15:34.383102" "level"="info" "logger"="node" "msg"="bee version" "version"="2.2.0-06a0aca7"
"time"="2024-09-24 18:15:34.428546" "level"="info" "logger"="node" "msg"="swarm public key" "public_key"="0373fe2ab33ab836635fc35864cf708fa0f4a775c0cf76ca851551e7787b58d040"
"time"="2024-09-24 18:15:34.520686" "level"="info" "logger"="node" "msg"="pss public key" "public_key"="03a341032724f1f9bb04f1d9b22607db485cccd74174331c701f3a6957d94d95c1"
"time"="2024-09-24 18:15:34.520716" "level"="info" "logger"="node" "msg"="using ethereum address" "address"="0x1A801dd3ec955E905ca424a85C3423599bfb0E66"
"time"="2024-09-24 18:15:34.533789" "level"="info" "logger"="node" "msg"="fetching target neighborhood from suggester" "url"="https://api.swarmscan.io/v1/network/neighborhoods/suggestion"
"time"="2024-09-24 18:15:36.773501" "level"="info" "logger"="node" "msg"="mining a new overlay address to target the selected neighborhood" "target"="00100010110"
"time"="2024-09-24 18:15:36.776550" "level"="info" "logger"="node" "msg"="using overlay address" "address"="22d502d022de0f8e9d477bc61144d0d842d9d82b8241568c6fe4e41f0b466615"
"time"="2024-09-24 18:15:36.776576" "level"="info" "logger"="node" "msg"="starting with an enabled chain backend"
"time"="2024-09-24 18:15:37.388997" "level"="info" "logger"="node" "msg"="connected to blockchain backend" "version"="erigon/2.60.7/linux-amd64/go1.21.5"
"time"="2024-09-24 18:15:37.577840" "level"="info" "logger"="node" "msg"="using chain with network network" "chain_id"=100 "network_id"=1
"time"="2024-09-24 18:15:37.593747" "level"="info" "logger"="node" "msg"="starting debug & api server" "address"="127.0.0.1:1633"
"time"="2024-09-24 18:15:37.969782" "level"="info" "logger"="node" "msg"="using default factory address" "chain_id"=100 "factory_address"="0xC2d5A532cf69AA9A1378737D8ccDEF884B6E7420"
"time"="2024-09-24 18:15:38.160249" "level"="info" "logger"="node/chequebook" "msg"="no chequebook found, deploying new one."
"time"="2024-09-24 18:15:38.728534" "level"="warning" "logger"="node/chequebook" "msg"="cannot continue until there is at least min xDAI (for Gas) available on address" "min_amount"="0.0003750000017" "address"="0x1A801dd3ec955E905ca424a85C3423599bfb0E66"
```

Here you can see that the node has started up successfully, but our node still needs to be funded with xDAI and xBZZ (xDAI for Gnosis Chain transactions and xBZZ for uploads/downloads and staking). 

### Fund node

Check the logs from the previous step. Look for the line which says: 

```
"time"="2024-09-24 18:15:34.520716" "level"="info" "logger"="node" "msg"="using ethereum address" "address"="0x1A801dd3ec955E905ca424a85C3423599bfb0E66"
```
That address is your node's address on Gnosis Chain which needs to be funded with xDAI and xBZZ. Copy it and save it for the next step.

xDAI is widely available from many different centralized and decentralized exchanges, just make sure that you are getting xDAI on Gnosis Chain, and not DAI on some other chain. See [this page](https://www.ethswarm.org/get-bzz) for a list of resources for getting xBZZ (again, make certain that you are getting the Gnosis Chain version, and not BZZ on Ethereum).  

After acquiring some xDAI and some xBZZ, send them to the address you copied above.

***How Much to Send?***

Only a very small amount of xDAI is needed to get started, 0.1 is more than enough.
 
You can start with just 2 or 3 xBZZ for uploading small amounts of data, but you will need at least 10 xBZZ if you plan on staking.


### Initialize full node

After sending the required tokens (~0.1 xDAI and 10 xBZZ) to your node's Gnosis Chain address, close the bee process in your terminal (`Ctrl + C`). Then start it again with the same command:

```bash
bee start \
  --password flummoxedgranitecarrot \
  --full-node \
  --swap-enable \
  --api-addr 127.0.0.1:1633 \
  --blockchain-rpc-endpoint https://xdai.fairdatasociety.org
```
After funding and restarting your node, the logs printed to the terminal should look something like this:

```bash
Welcome to Swarm.... Bzzz Bzzzz Bzzzz
                \     /
            \    o ^ o    /
              \ (     ) /
   ____________(%%%%%%%)____________
  (     /   /  )%%%%%%%(  \   \     )
  (___/___/__/           \__\___\___)
     (     /  /(%%%%%%%)\  \     )
      (__/___/ (%%%%%%%) \___\__)
              /(       )\
            /   (%%%%%)   \
                 (%%%)
                   !

DISCLAIMER:
This software is provided to you "as is", use at your own risk and without warranties of any kind.
It is your responsibility to read and understand how Swarm works and the implications of running this software.
The usage of Bee involves various risks, including, but not limited to:
damage to hardware or loss of funds associated with the Ethereum account connected to your node.
No developers or entity involved will be liable for any claims and damages associated with your use,
inability to use, or your interaction with other nodes or the software.

version: 2.2.0-06a0aca7 - planned to be supported until 11 December 2024, please follow https://ethswarm.org/

"time"="2024-09-24 18:57:16.710417" "level"="info" "logger"="node" "msg"="bee version" "version"="2.2.0-06a0aca7"
"time"="2024-09-24 18:57:16.760154" "level"="info" "logger"="node" "msg"="swarm public key" "public_key"="0373fe2ab33ab836635fc35864cf708fa0f4a775c0cf76ca851551e7787b58d040"
"time"="2024-09-24 18:57:16.854594" "level"="info" "logger"="node" "msg"="pss public key" "public_key"="03a341032724f1f9bb04f1d9b22607db485cccd74174331c701f3a6957d94d95c1"
"time"="2024-09-24 18:57:16.854651" "level"="info" "logger"="node" "msg"="using ethereum address" "address"="0x1A801dd3ec955E905ca424a85C3423599bfb0E66"
"time"="2024-09-24 18:57:16.866697" "level"="info" "logger"="node" "msg"="using overlay address" "address"="22d502d022de0f8e9d477bc61144d0d842d9d82b8241568c6fe4e41f0b466615"
"time"="2024-09-24 18:57:16.866730" "level"="info" "logger"="node" "msg"="starting with an enabled chain backend"
"time"="2024-09-24 18:57:17.485408" "level"="info" "logger"="node" "msg"="connected to blockchain backend" "version"="erigon/2.60.1/linux-amd64/go1.21.5"
"time"="2024-09-24 18:57:17.672282" "level"="info" "logger"="node" "msg"="using chain with network network" "chain_id"=100 "network_id"=1
"time"="2024-09-24 18:57:17.686479" "level"="info" "logger"="node" "msg"="starting debug & api server" "address"="127.0.0.1:1633"
"time"="2024-09-24 18:57:18.065029" "level"="info" "logger"="node" "msg"="using default factory address" "chain_id"=100 "factory_address"="0xC2d5A532cf69AA9A1378737D8ccDEF884B6E7420"
"time"="2024-09-24 18:57:18.252410" "level"="info" "logger"="node/chequebook" "msg"="no chequebook found, deploying new one."
"time"="2024-09-24 18:57:19.576100" "level"="info" "logger"="node/chequebook" "msg"="deploying new chequebook" "tx"="0xf7bc9c5b04e96954c7f70cecfe717cad9cdc5d64b6ec080b2cbe712166ce262a"
"time"="2024-09-24 18:57:27.619377" "level"="info" "logger"="node/transaction" "msg"="pending transaction confirmed" "sender_address"="0x1A801dd3ec955E905ca424a85C3423599bfb0E66" "tx"="0xf7bc9c5b04e96954c7f70cecfe717cad9cdc5d64b6ec080b2cbe712166ce262a"
"time"="2024-09-24 18:57:27.619437" "level"="info" "logger"="node/chequebook" "msg"="chequebook deployed" "chequebook_address"="0x261a07a63dC1e7200d51106155C8929b432181fb"
```

Here we can see that after our node has been funded, it was able to issue the transactions for deploying the chequebook contract, which is a prerequisite for running a staking node.

Next your node will begin to sync [postage stamp data](https://docs.ethswarm.org/docs/develop/access-the-swarm/buy-a-stamp-batch), which can take ~5 to 10 minutes. You will see this log message while your node is syncing postage stamp data:

```bash
"time"="2024-09-24 22:21:19.664897" "level"="info" "logger"="node" "msg"="waiting to sync postage contract data, this may take a while... more info available in Debug loglevel"
```

After your node finishes syncing postage stamp data it will start in full node mode and begin to sync all the chunks of data it is responsible for storing as a full node:


```bash
"time"="2024-09-24 22:30:19.154067" "level"="info" "logger"="node" "msg"="starting in full mode"
"time"="2024-09-24 22:30:19.155320" "level"="info" "logger"="node/multiresolver" "msg"="name resolver: no name resolution service provided"
"time"="2024-09-24 22:30:19.341032" "level"="info" "logger"="node/storageincentives" "msg"="entered new phase" "phase"="reveal" "round"=237974 "block"=36172090
"time"="2024-09-24 22:30:33.610825" "level"="info" "logger"="node/kademlia" "msg"="disconnected peer" "peer_address"="6ceb30c7afc11716f866d19b7eeda9836757031ed056b61961e949f6e705b49e"
```

This process can take a while, up to several hours depending on your system and network. You can check the progress of your node through the logs which print out to the Bee API:

You check your node's progress with the `/status` endpoint:

:::info
The [`jq` utility](https://jqlang.github.io/jq/) is used here to automatically format the output from the Bee API. It can help make API output more readable. You may need to install it, the exact steps will depend on your Linux distro and package manager of choice. Also feel free to remove the `| jq` from the command as it is only a convenience, not a requirement.
:::

```bash
curl -s  http://localhost:1633/status | jq
```

```bash
{
  "overlay": "22dc155fe072e131449ec7ea2f77de16f4735f06257ebaa5daf2fdcf14267fd9",
  "proximity": 256,
  "beeMode": "full",
  "reserveSize": 686217,
  "reserveSizeWithinRadius": 321888,
  "pullsyncRate": 497.8747754074074,
  "storageRadius": 11,
  "connectedPeers": 148,
  "neighborhoodSize": 4,
  "batchCommitment": 74510761984,
  "isReachable": false,
  "lastSyncedBlock": 36172390
}
```
We can see that our node has not yet finished syncing chunks since the `pullsyncRate` is around 497 chunks per second. Once the node is fully synced, this value will go to zero. However, we do not need to wait until our node is fully synced in order to stake our node, so we can now move immediately to the next step.


### Stake node

Now we're ready to begin staking, we will slightly modify our startup command so that it now runs in the background instead of taking control of our terminal:

```bash
nohup bee start \
  --password flummoxedgranitecarrot \
  --full-node \
  --swap-enable \
  --api-addr 127.0.0.1:1633 \
  --blockchain-rpc-endpoint https://xdai.fairdatasociety.org > bee.log 2>&1 &
```

:::info
1. **`nohup`**: This ensures that the `bee start` process will continue even after the terminal is closed.

2. **`> bee.log 2>&1`**: Redirects both standard output and standard error to a log file called `bee.log`. 

3. **`&`**: This sends the process to the background, allowing the terminal to be used for other commands while the Bee node continues running.
:::

Let's check the Bee API to confirm the node is running:

```
curl localhost:1633
```
If the node is running we should see:
```
Ethereum Swarm Bee
```

Now with our node properly running in the background, we're ready to stake our node. You can use the following command to stake 10 xBZZ:

```bash
curl -XPOST localhost:1633/stake/100000000000000000
```

If the staking transaction is successful a `txHash` will be returned:

```
{"txHash":"0x258d64720fe7abade794f14ef3261534ff823ef3e2e0011c431c31aea75c2dd5"}
```

We can also confirm that our node has been staked with the `/stake` endpoint:

```bash
curl localhost:1633/stake
```

The results will be displayed in PLUR units (1 PLUR is equal to 1e-16 xBZZ). If you have properly staked the minimum 10 xBZZ, you should see the output below:

```bash
{"stakedAmount":"100000000000000000"}
```
 
Congratulations! You have now installed your Bee node and are connected to the network as a full staking node. Your node will now be in the process of syncing chunks from the network. Once it is fully synced, your node will finally be eligible for earning staking rewards. 

### Logs and monitoring

With our previously modified command, our Bee node will now be running in the background and the logs will be written to the `bee.log` file. To review our node's logs we can simply view the file contents:

```bash
cat bee.log
```

The file will continue to update with all the latest logs as they are output:

```bash
"time"="2024-09-27 18:05:34.096641" "level"="info" "logger"="node/kademlia" "msg"="connected to peer" "peer_address"="03b48e678938d63c0761c74a805fbe0446684c9c417330c2bec600ecfd6c492f" "proximity_order"=8
"time"="2024-09-27 18:05:35.168425" "level"="info" "logger"="node/kademlia" "msg"="connected to peer" "peer_address"="0e9388fff473a9c74535337c32cc74d8f921514d2635d0c4a49c6e8022f5594e" "proximity_order"=4
"time"="2024-09-27 18:05:35.532723" "level"="info" "logger"="node/kademlia" "msg"="disconnected peer" "peer_address"="3c195cd8882ee537d170e92d959ad6bd72a76a50097a671c72646e83b45a1832"
```

There are many different ways to monitor your Bee node's process, but one convenient way to do so is the [bashtop command line tool](https://github.com/aristocratos/bashtop). The method of [installation](https://github.com/aristocratos/bashtop?tab=readme-ov-file#installation) will vary depending on your system.

After installation, we can launch it with the `bashtop` command:

```bash
bashtop
```

![](/img/node/bashtop_01.png)

We can use the `f` key to filter for our Bee node's specific process by searching for the `bee` keyword (use the arrow keys to navigate and `enter` to select). From here we can view info about our node's process, or shut it down using the `t` key (for "terminate").

![](/img/node/bashtop_02.png)

**Checking the Node's status with the Bee API**

To check your node's status as a staking node, we can use the `/redistributionstate` endpoint:

```bash
curl -s http://localhost:1633/redistributionstate | jq
```

Below is the output for a node which has been running for several days:

```bash
{
  "minimumGasFunds": "11080889201250000",
  "hasSufficientFunds": true,
  "isFrozen": false,
  "isFullySynced": true,
  "phase": "claim",
  "round": 212859,
  "lastWonRound": 207391,
  "lastPlayedRound": 210941,
  "lastFrozenRound": 210942,
  "lastSelectedRound": 212553,
  "lastSampleDuration": 491687776653,
  "block": 32354719,
  "reward": "1804537795127017472",
  "fees": "592679945236926714",
  "isHealthy": true
}
```

For a complete breakdown of this output, check out [this section in the Bee docs](https://docs.ethswarm.org/docs/bee/working-with-bee/bee-api#redistributionstate).

You can read more other important endpoints for monitoring your Bee node in the [official Bee docs](https://docs.ethswarm.org/docs/bee/working-with-bee/bee-api), and you can find complete information about all available endpoints in [the API reference docs](https://docs.ethswarm.org/api/).
