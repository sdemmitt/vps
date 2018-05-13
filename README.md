# Airin Masternode - VPS Installation - Multiple IPv4

This masternode installation script vastly simplifies the setup of a Airin masternode running on a virtual private server (VPS), and it also adds a number of other powerful features, including:

* Installs 1-3 Airin masternodes in parallel on one VPS, with individual airin.conf and data directories
* Support for multiple IPv4 Addresses - **Note: At this time Airin does not support IPV6 so IPV4 is enabled by default.**
* It can install masternodes for other coins on the same VPS as Airin
* 100% auto-compilation and 99% of configuration on the masternode side of things
* Automatically compiling from the latest Airin release tag, or another tag can be specified
* Some security hardening is done, including firewalling and a separate user, increasing security
* Automatic startup for all masternode daemons

Some notes and requirements:

* Script has only been tested on a Vultr VPS, but should work almost anywhere where IPV4 addresses are available
* Additional IPV4 addresses can be added for multiple masternodes on a single vps but this process must be manually configured
* Currently only Ubuntu 16.04 Linux is supported
* This script needs to run as root or with sudo, the masternodes will and should not!

This project was forked from https://github.com/masternodes/vps. @marsmensch (Florian) is the primary author behind this VPS installation script for masternodes. If you would like to donate to him, you can use the BTC address below

**Have fun, this is crypto after all!**

```
BTC  33ENWZ9RCYBG7nv6ac8KxBUSuQX64Hx3x3
```

# Install Guide on Vultr

## How to Get a VPS Server

For new masternode owners, **Vultr** is recommended as a VPS hosting provider, but other providers that allow direct root SSH login access and offer Ubunto 16.04 may work.

You can use the following referral link to sign up with Vultr for VPS hosting:

<a href="https://www.vultr.com/?ref=7316561"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>

## Deploy a New System

First, create a new VPS by clicking that small "+" button.

<img src="docs/images/masternode_vps/deploy-a-new-system.png" alt="VPS creation" class="inline"/>

## Location Choice

You can choose any location. You may wish to have it hosted in a city/country near you, or choose a different area to help with the global decentralization of the Airin masternode network.

<img src="docs/images/masternode_vps/location-choice.png" alt="VPS location choice" class="inline"/>

## Linux Distribution (Ubuntu 16.04 LTS)

Select Ubuntu 16.04.

<img src="docs/images/masternode_vps/linux-distribution--ubuntu-1604-lts-.png" alt="VPS location choice" class="inline"/>

## VPS Size

The 25 GB SSD / 1024MBB Memory instance is enough for 2-3 masternodes. You may need more memory as the Airin blockchain grows over time, or if you want to run more masternodes.

<img src="docs/images/masternode_vps/vps-size.png" alt="VPS sizing" class="inline"/>

## Hostname

Choose 1 instance and click "Deploy Now".

<img src="docs/images/masternode_vps/hostnames--amp--number-of-vps.png" alt="VPS sizing" class="inline"/>

## Installation of PuTTY as SSH client (Windows)

If you are running your wallet from Windows, install PuTTY while the server is being set up. You can download PuTTY from here: http://www.putty.org/. Skip this step if you are using a Mac--you will use the built in Terminal application instead.

Once PuTTY is installed, return to the Vultr dashboard to get the login details by clicking on the ... to the right of your server, and select Server Details.

## Activating Additional IPv4 Addresses

Go to Settings on the server details page. The IPv4 section will appear. 

Click 'Add Another IPv4 Address'. When prompted, click 'Add IPv4 Address'.

Repeat for each additional masternode. The maximum number of IPv4 addresses is 3 per VPS.

Note: There is an additional charge for each IPv4 address added. 

<img src="docs/images/masternode_vps/activating-additional-features--ipv6-.png" alt="VPS sizing" class="inline"/>

## Retrieve Networking Configuration

Click the networking configuration link located under the IPv4 section.

Scroll down to Ubuntu 16.xx, Ubuntu 17.04

Copy and paste the configuration shown into a text editor.

Save this for later. This will replace the /etc/network/interfaces configuration file on the vps.

## Accessing your VPS via SSH

Copy your password for SSH access from the server details page.

<img src="docs/images/masternode_vps/accessing-your-vps-via-ssh.png" alt="check hostname and password" class="inline"/>

Now open PuTTY to add the server.

<img src="docs/images/masternode_vps/login-to-vps-via-PuTTY.png" alt="login to VPS" class="inline"/>

Enter the IP address in the Host Name field, and enter the server name you wish to use for this VPS (e.g., MN01) to Saved Sessions. Click save.

Click the open button. When the console has opened, click Yes in the PuTTY Security Alert box.

<img src="docs/images/masternode_vps/PuTTY-Security-Alert.png" alt="Alert from PuTTY" class="inline"/>

Now enter your server login details provided in your Vultr account.
You cannot Ctrl+V to paste in the console. Either right click the mouse or type shift+insert (sometimes
on keyboard it will just be INS key)

User: root
Password: (paste or type password)

When you paste it will not display, so don't try to paste again.
Just paste once and press Enter.

For Mac users, open Terminal (e.g., Press Command-Space and type Terminal and press Enter). Then type:

```
ssh -l root <IP address>
```

## Installation on VPS

After logging into VPS through SSH:

**Edit Network Interfaces**

```bash
nano /etc/network/interfaces
```

Delete each line shown. (Press Del Key)

Copy and paste the network configuration that was previously saved in the text editor. (Retrieve Networking Configuration Step)

Remember: You cannot Ctrl+V to paste in the console. Either right click the mouse or type shift+insert (sometimes
on keyboard it will just be INS key)

**Save and Close the File**

```
CTRL+X → Y → ENTER
```

**Configure Networking Changes**

```bash
ifup ens3:1
ifup ens3:2
```

**Clone the Github Repository to Install Masternodes**

```bash
git clone https://github.com/sdemmitt/vps.git && cd vps
```

**Install 3 Airin masternodes using IPv4 and configure sentinel monitoring:**

```bash
./install.sh -p airin -n 4 -c 3 -s 
```

## Additional examples

**Install 2 Airin masternodes using IPv4 and configure sentinel monitoring:**

```bash
./install.sh -p airin -n 4 -c 2 -s
```

**Install 1 Airin masternode using IPv4 and configure sentinel monitoring:**

```bash
./install.sh -p airin -n 4 -c 1 -s
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

After the masternodes have been installed. 

**A) Generate Private Key**

Note: The private key will need to be added to the airin_n1.conf file (located on the vps) and the masternode.conf file (located on the Airin Wallet) 

```bash
/usr/local/bin/airin-cli -conf=/etc/masternodes/airin_n1.conf masternode genkey
```

**B) Edit Configuration File**

```bash
nano /etc/masternodes/airin_n1.conf
```

**C) Add IP**

Replace YOUR_VPS_IPV4_ADDRESS with ip address of your vps:

```
bind=YOUR_VPS_IPV4_ADDRESS:18808
```

**D) Add Private Key**

Replace YOUR_MASTERNODE_PRIVATE_KEY with your private key:

```
masternodeprivkey=YOUR_MASTERNODE_PRIVATE_KEY
```

**E) Save and Close the File**

```
CTRL+X → Y → ENTER
```

**F) Repeat Steps A - E**

For each additional masternode created, repeat steps A - E with the following changes:

* Replace 'airin_n1.conf' with 'airin_n2.conf' or 'airin_n3.conf'

* Use the added IPv4 addresses created in Vultr earlier for each added masternode. 

Note: IP addresses cannot not be used more than once.

**G) Reboot**

```bash
sudo reboot
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

If you want to check the info of your masternode, the best way is currently running the cli e.g. for $AIRIN via

```
/usr/local/bin/airin-cli -conf=/etc/masternodes/airin_n1.conf getinfo

{
  "version": 120201,
  "protocolversion": 70206,
  "walletversion": 61000,
  "balance": 0.00000000,
  "privatesend_balance": 0.00000000,
  "blocks": 8679,
  "timeoffset": 0,
  "connections": 19,
  "proxy": "",
  "difficulty": 45.94715393716314,
  "testnet": false,
  "keypoololdest": 1526099573,
  "keypoolsize": 1001,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "errors": ""
}
```

To retrieve masternode status:

```bash
/usr/local/bin/airin-cli -conf=/etc/masternodes/airin_n1.conf masternode status
```

The rpcport used in airin_n1.conf (airin_n2.conf, airin_n3.conf) files must be unique. Increase the port number +1 if any values match across each masternode. 

## Issues and Questions
Please open a GitHub Issue if there are problems with this installation method. Check the Airin Discord channel for support or assistance.
Here is a Discord invitation:

https://discord.gg/6emAcHj