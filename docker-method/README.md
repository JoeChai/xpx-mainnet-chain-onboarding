# ProximaX Sirius MAINNET Onboarding

## OS Requirements
Ensure that your local network allows inbound/outbound traffic on these ports:
- 3000/tcp
- 7900/tcp
- 7901/tcp
- 7902/tcp

A note on System Requirements:
Theoretically, this dockerized Sirius Peer package can run on any OS running Docker version 19.03.3 and docker-compose version 1.24.0.

But if you really need a minimum benchmark, we have seen the Sirius Blockchain Peer to work with a minimum Hardware of 1 CPU core and 2GB RAM.

This README was prepared by testing the package on:
- Debian 10 ++
- Ubuntu 19.04 ++
- CentOS 7 ++

## Pre-requisite
This onboarding method requires `docker` and `docker-compose`.  

Run the following command to install `docker`:
```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sh get-docker.sh
```

As per docker post-installation note, we recommend using Docker as a non-root user.  Therefore, you should now consider adding your user to the "docker" group with something like:
```
$ sudo usermod -aG docker $your_user
```
Remember that you will have to log out and back in for this to take effect!"

Installation instructions for docker-compose can be found [here](https://docs.docker.com/compose/install/). 

Enable and start Docker:
```
$ sudo systemctl enable docker.service
$ sudo systemctl start docker.service
$ sudo systemctl status docker.service
```

## Download and Extract the package
```
$ wget https://github.com/proximax-storage/xpx-mainnet-chain-onboarding/releases/download/release-v0.4.3-buster/public-mainnet-peer-package-v0.4.3.tar.gz
$ tar -xvf public-mainnet-peer-package-v0.4.3.tar.gz
$ cd public-mainnet-peer-package-v0.4.3
```

## Generate a keypair
```
$ tools/gen_key_pair_addr
```

## Insert private key in [config-user.properties](resources/config-user.properties)

Replace `BOOTKEY_PRIVATE_KEY` with the generated private key. This is the account which holds the node reputation.

```
[account]

bootKey = BOOTKEY_PRIVATE_KEY 

[storage]

dataDirectory = /data
pluginsDirectory = 
```

## Assign a friendly name in  [config-node.properties](resources/config-node.properties) (OPTIONAL)

Add the domain name or public IP address to the `host` parameter, leave empty to auto-detect. Add a `friendlyName` to assign a name to your node (like: `mainnet-looneytoons-01`).

```
[localnode]

host =
friendlyName =
version = 0
roles = Peer
```

## Delegated Validating
You may activate your account for delegated validating by running the following tool:
```
$ tools/delegated_validating_mainnet
```

After running the above tool, add the delegated remote account private key in the [config-harvesting.properties](resources/config-harvesting.properties):
```
[harvesting]
# private keys are 64 characters
harvestKey = REMOTE_ACCOUNT_PRIVATE_KEY
beneficiary = 0000000000000000000000000000000000000000000000000000000000000000
isAutoHarvestingEnabled = true
maxUnlockedAccounts = 5
```

Verify that your account has successfully activated delegated validation by checking ProximaX online [explorer](https://explorer.xpxsirius.io) using the generated transaction hash or using your account address.

Please note that if your account does not have any XPX or previously linked to another remote account, the transaction will be unsuccessful.

**For more info, please read our online documentations [here](https://bcdocs.xpxsirius.io/docs/protocol/validating/)**

## Start the Peer Node
```
$ docker-compose up -d
```

## Check if container is running
```
$ docker container ls
```

## Stop the Peer Node
```
$ docker-compose down
```

## Restart the Peer Node
```
$ docker-compose restart
```

## Check logs
There are 2 ways to view the logs:
1. docker logs
```
$ docker-compose logs --tail=100 -f
# Press Ctrl-C to stop tailing the logs
```

2. log files in `logs` directory


## Helpdesk
We have a [telegram helpdesk](https://t.me/proximaxhelpdesk) to assist general queries.

For validation-specific queries, you may discuss it at [ProximaX Blockchain Validators Group](https://t.me/xpxtestnetvalidator)
