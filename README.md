# Airin Masternode VPS Installation

This masternode installation script vastly simplifies the setup of a Airin masternode running on a virtual private server (VPS), and it also adds a number of other powerful features, including:

* IPv6 Support - **Note: At this time Airin does not support IPV6 so IPV4 is enabled by default. Additional IPV4 addresses can be added for multiple masternodes on a single vps but this process must be manually configured.**
* Installs 1-100 (or more!) Airin masternodes in parallel on one VPS, with individual airin.conf and data directories
* It can install masternodes for other coins on the same VPS as Airin
* 100% auto-compilation and 99% of configuration on the masternode side of things
* Automatically compiling from the latest Airin release tag, or another tag can be specified
* Some security hardening is done, including firewalling and a separate user, increasing security
* Automatic startup for all masternode daemons

Some notes and requirements:

* Script has only been tested on a Vultr VPS, but should work almost anywhere where IPV4 or IPv6 addresses are available
* Currently only Ubuntu 16.04 Linux is supported
* This script needs to run as root or with sudo, the masternodes will and should not!

This project was forked from https://github.com/masternodes/vps. @marsmensch (Florian) is the primary author behind this VPS installation script for masternodes. If you would like to donate to him, you can use the BTC address below

**Have fun, this is crypto after all!**

```
BTC  33ENWZ9RCYBG7nv6ac8KxBUSuQX64Hx3x3
```

## Installation on VPS

SSH to your VPS and clone the Github repository:

```bash
git clone https://github.com/sdemmitt/vps.git && cd vps
```

Typical users should use the following script:

**Install & configure a single master node using IPV4 with Sentinel monitoring and activate node after installation:**

```bash
./install.sh -p airin -n 4 -s -x
```

## Options

The _install.sh_ script support the following parameters:

| Long Option  | Short Option | Values              | description                                                         |
| :----------- | :----------- | ------------------- | ------------------------------------------------------------------- |
| --project    | -p           | project, e.g. "pix" | shortname for the project                                           |
| --net        | -n           | "4" / "6"           | ip type for masternode. (ipv)6 is default                           |
| --release    | -r           | e.g. "tags/v3.0.4"  | a specific git tag/branch, defaults to latest tested                |
| --count      | -c           | number              | amount of masternodes to be configured                              |
| --update     | -u           | --                  | update specified masternode daemon, combine with -p flag            |
| --sentinel   | -s           | --                  | install and configure sentinel for node monitoring                  |
| --wipe       | -w           | --                  | uninstall & wipe all related master node data, combine with -p flag |
| --help       | -h           | --                  | print help info                                                     |
| --startnodes | -x           | --                  | starts masternode(s) after installation                             |

## After Installation on VPS

**Generate Private Key**
Note: The private key will need to be added to the airin_n1.conf file (located on the vps) and the masternode.conf file (located on the Airin Wallet) 

```bash
/usr/local/bin/airin-cli -conf=/etc/masternodes/airin_n1.conf masternode genkey
```

**Edit Configuration File**

```bash
nano /etc/masternodes/airin_n1.conf
```

**Add IP**
Replace the following lines with your information:

```
bind=YOUR_VPS_IPV4_ADDRESS:18808
```

**Add Private Key**
Replace the following lines with your information:

```
masternodeprivkey=YOUR_MASTERNODE_PRIVATE_KEY
```

**Save and Close the File**

```
CTRL+X → Y → ENTER
```

**Reboot**

```bash
sudo reboot
```

To retrieve masternode info:

```bash
/usr/local/bin/airin-cli -conf=/etc/masternodes/airin_n1.conf getinfo
```

To retrieve masternode status:

```bash
/usr/local/bin/airin-cli -conf=/etc/masternodes/airin_n1.conf masternode status
```

## After Installation on Airin Wallet

**Create Collateral Transaction**

Once the wallet is open on your local computer, generate a new receive address and label it however you want to identify your masternode rewards (e.g., Airin-MN-1). This label will show up in your transactions each time you receive a block reward.
Click the Request payment button, and copy the address.

Now go to the Send tab, paste the copied address, and send exactly 1,000 AIRIN to it in a single transaction. Wait for it to confirm on the blockchain. This is the collateral transaction that will be locked and paired with your new masternode.

**Go to Tools → Debug Console**

In text box below run the following command
masternode outputs

It should print something like this:

```
{
“e73c2368c132b1334d1c4e2346adcdf81dc37a7002e6fr6295a1ae90026a35ce”:“1”
}
```
The long string is the transaction id of your payment and the number is transaction index. Save them too, you will use them shortly.
Note that transaction id and index number do not contain quotes. In the example above transaction id is: e73c2368c132b1334d1c4e2346adcdf81dc37a7002e6fr6295a1ae90026a35ce.

**Go to Tools → Masternode configuration file. It will open your masternode config file.**

In a new line type your masternode data, the syntax is as following:

```
ALIAS YOUR_VPS_IPV4_ADDRESS:18808 YOUR_MASTERNODE_PRIVATE_KEY TRX_ID_OF_YOUR_PAYMENT TRX_INDEX
```

You can copy paste the line above and replace with your own ip, privkey, transaction id and index. Note that each data is separated by space, so do not introduce any space yourself. For example, giving the following alias is wrong.
My alias
Remember that transaction id and index do not contain any quotes
Save and close the config file. Restart the wallet.

**Go to «Masternodes» tab (Settings → Options → Wallet → Show Masternodes Tab). You should see your configured masternode as missing. Click on «Start Missing», give your passphrase and confirm.**

Now you should see your masternode as «Pre_Enabled» or «Enabled»
If so, you are all set. It will start getting rewards in around 24 hours.
Happy Collecting your Masternode Rewards!

## Troubleshooting the Masternode on the VPS

If you want to check the status of your masternode, the best way is currently running the cli e.g. for $AIRIN via

```
/usr/local/bin/mue-cli -conf=/etc/masternodes/airin_n1.conf getinfo

{
  "version": 1000302,
  "protocolversion": 70701,
  "walletversion": 61000,
  "balance": 0.00000000,
  "privatesend_balance": 0.00000000,
  "blocks": 209481,
  "timeoffset": 0,
  "connections": 5,
  "proxy": "",
  "difficulty": 42882.54964804553,
  "testnet": false,
  "keypoololdest": 1511380627,
  "keypoolsize": 1001,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "errors": ""
}
```

# Help, Issues and Questions

I activated the "[issues](https://github.com/masternodes/vps/issues)" option on github to give you a way to document defects and feature wishes. Feel free top [open issues](https://github.com/masternodes/vps/issues) for problems / features you are missing here: [https://github.com/masternodes/vps/issues](https://github.com/masternodes/vps/issues).

I might not be able to reply immediately, but i do usually within a couple of days at worst. I will also happily take any pull requests that make masternode installations easier for everyone ;-)

If this script helped you in any way, please contribute some feedback. BTC donations also welcome and never forget:

**Have fun, this is crypto after all!**

```
BTC  33ENWZ9RCYBG7nv6ac8KxBUSuQX64Hx3x3
```