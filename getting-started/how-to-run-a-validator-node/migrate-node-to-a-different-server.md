# Tutorial: Migrating Your Node to a Different Server with Minimal Downtime

Migrating your node to a different server is a crucial task, whether you're changing service providers, upgrading your server specs, or for any other reason. To ensure a seamless transition with minimal downtime, follow the steps outlined in this guide.

## Step 1: Set Up a New API Node on the New Server

The first step in migrating your node is to create a new API node on your new server. This will serve as the foundation for the migration process. Make sure to configure this new node to use the fast sync option for quicker synchronization with the blockchain.

On your new server setup a working folder for the node, for this example it would be a folder named `icon-node` in the home folder:

```bash
mkdir ~/icon-node
cd ~/icon-node
```

Install [Docker Desktop](https://www.docker.com/get-started/). The installation of the Docker Desktop package will install the Docker Engine and also `docker compose` which is a requirement to run the node.

To verify the installation has been done correctly run the following commands:

```bash
docker compose version
Docker Compose version v2.17.3

docker --version
Docker version 23.0.5, build bc4487a

docker version
Client: Docker Engine - Community
 Cloud integration: v1.0.31
  Version:           23.0.5
   API version:       1.42
   <...>
```
After finishing the installation of Docker, move into the working folder for the node (`~/icon-node`) and create a new wallet for the API node, you can easily do this with a software wallet like Hana or you can follow the instructions in the following tutorial.

https://docs.icon.community/getting-started/how-to-create-a-wallet-account

Once you have the keystore file for the wallet create a new folder called `config` inside the root folder of your new API node (`~/icon-node/`)

```
mkdir config
```

Inside the root folder of the node create a `docker-compose.yml` file with the following content:

```yaml
version: "3"
services:
   prep:
     image: iconloop/icon2-node:v1.3.9 # make sure to use latest version
     container_name: "icon2-node"
     restart: "on-failure"
     environment:
         SERVICE: "MainNet"  # MainNet, SeJong
         GOLOOP_LOG_LEVEL: "debug" # trace, debug, info, warn, error, fatal, panic
         KEY_STORE_FILENAME: "KEYSTORE_FILE_NAME" # e.g. keystore.json read a config/keystore.json
         # e.g. "/goloop/config/keystore.json" read a "config/keystore.json" of host machine
         KEY_PASSWORD: "KEYSTORE_PASSWORD"
         FASTEST_START: "true"    # It can be restored from latest Snapshot DB.
         ROLE: 0 # Validator = 3, API endpoint = 0

     cap_add:
      - SYS_TIME

     volumes:
      - ./data:/goloop/data # mount a data volumes
      - ./config:/goloop/config # mount a config volumes ,Put your used keystore file here.
      - ./logs:/goloop/logs
     ports:
      - 9000:9000
      - 7100:7100
```

Remember to add the correct values for the name of the keystore file and the password in the `KEY_STORE_FILENAME` and `KEY_PASSWORD` params inside the `docker-compose.yml` file.

Inside this folder now run the following commands to start the node:

```bash
docker compose pull
docker compose up -d
```

Now we need to check the node periodically until is fully sync before doing the migration.

The following commands can be used to monitor the synchronization process and to troubleshoot any issues:

```bash
curl localhost:9000/admin/chain
docker exec -it icon2-node goloop chain ls
tail -f logs/booting.log
tail -f logs/goloop.log
du -h
```

Once the node has reached a certain point in the sync process you can see the following info when running the command `tail -f logs/booting.log`

```
$ tail -f logs/booting.log
I|20230927-17:37:52.000641|-|BOOTING.LOG|configure_setter.py:87| Start create_icon_config
I|20230927-17:37:52.000641|-|BOOTING.LOG|configure_setter.py:118| Start create_db
I|20230927-17:37:52.000641|-|BOOTING.LOG|configure_setter.py:119| [RESTORE] FASTEST_START = True
I|20230927-17:37:52.000641|-|BOOTING.LOG|configure_setter.py:123| [RESTORE] DOWNLOAD from ICON2 DB
I|20230927-17:37:52.000648|-|BOOTING.LOG|restore_v3.py:238| [RESTORE PASS] Already restored. If you want to start over, delete the '/goloop/data/restore/RESTORED & /goloop/data/restore/checksum_result.json' file.
I|20230927-17:37:52.000648|-|BOOTING.LOG|restore_v3.py:241| [RESTORE PASS] CHECKSUM RESULT CHECK is 'OK'
I|20230927-17:37:52.000000|-|/run/s6/services/02-manager| Start ICON2 Manager - /goloop, /goloop/data/cli.sock, /goloop/logs, /goloop/logs/booting.log
I|20230927-17:37:52.000000|-|/goloop| Start goloop server (file, /goloop, /goloop/config/server.json)
I|20230927-17:37:53.000200|-|BOOTING.LOG|configure.py:103| Load config_from_file
```

The steps to setup a node are also detailed in the following guide:

* [How to run a validator node](https://docs.icon.community/getting-started/how-to-run-a-validator-node)


## Step 2: Synchronize the New API Node with the Blockchain

Allow your new API node to sync up with the blockchain. This process may take some time, but it's essential to ensure that your new node is up to date with the latest data.

To verify the current height of the node you can run the following curl command inside the server:

```bash
curl localhost:9000/admin/chain/0x1
```

The response will be a JSON object like the following:
```json
{
 "cid":"0x1",
 "nid":"0x1",
 "channel":"icon_dex",
 "state":"started",
 "height":71477403,
 "lastError":"",
 "genesisTx":{
    "accounts":[{"address":"hx54f7853dc6481b670caf69c5a27c7c8fe5be8269","balance":"0x2961fff8ca4a62327800000","name":"god"},{"address":"hx1000000000000000000000000000000000000000","balance":"0x0","name":"treasury"}],"message":"A rhizome has no beginning or end; it is always in the middle, between things, interbeing, intermezzo. The tree is filiation, but the rhizome is alliance, uniquely alliance. The tree imposes the verb \"to be\" but the fabric of the rhizome is the conjunction, \"and ... and ...and...\"This conjunction carries enough force to shake and uproot the verb \"to be.\" Where are you going? Where are you coming from? What are you heading for? These are totally useless questions.\n\n - Mille Plateaux, Gilles Deleuze \u0026 Felix Guattari\n\n\"Hyperconnect the world\""}
 <...>
}
```

## Step 3: Prepare for Migration

Before proceeding further, ensure that your new API node is fully synchronized with the blockchain.

In the meantime while the new node is synchronizing, copy the following files from your old server to your new server, put these file in a temp folder while the sync process is ongoing:

* `docker-compose.yml`. This file can be find in the root folder of the node, for the case of this guide the folder is `~/icon-node/docker-compose.yml`.
* `keystore.json`. The keystore file (wallet) in your old server can be found in the `config` folder of your node working folder, in the case of this guide the folder is `~/icon-node/config/keystore.json`.

## Step 4: Stop the Current Node

On your old server, stop the main node that you intend to migrate. This will temporarily halt the node's operation.

Run the following command in your node working folder.
```bash
docker compose down
```

## Step 5: Copy Configuration Files

Stop the new API node the same way you stopped your main node by running `docker compose down` inside the working folder of the new server.

Before copying the configuration files of your node make a copy of the configuration files of the API node you have in the new server, you can find them in these locations:

* `~/icon-node/docker-compose.yml`
* `~/icon-node/config/keystore.json`

Now you can copy the configuration files that you already saved in a temporary folder into the respective folder paths in your new server (the same path described previosly):

* `~/icon-node/docker-compose.yml`
* `~/icon-node/config/keystore.json`

## Step 6: Start the New Node

With the configuration files successfully copied to the new server, start the new node on the new server.

Run the following commands to start the new node as a validator running with your validator wallet:

```bash
docker compose up -d
```

## Step 7: Monitor for Smooth Operation

Monitor the newly migrated node to ensure that it operates smoothly without any issues. Keep a close eye on its performance and connectivity.

By following these steps carefully, you can migrate your node to a different server while minimizing downtime and ensuring a seamless transition.

## Troubleshooting

If you've followed the steps outlined in this tutorial, you should encounter no major issues during the server migration process. It is crucial to ensure that you proceed with the final steps (starting at step #4) only after the new server node, initially configured as an API node, has fully synchronized with the blockchain. This guarantees that your node on the new server will have the fewest blocks to catch up with during the brief downtime window while migrating.

In the event that the new server fails to validate blocks or synchronize with the blockchain for any reason, we recommend the following steps:

1. Return to the original server and restore it to its previous configuration.
2. Resume troubleshooting or seek assistance in the [Discord channels](https://icon.community/icondiscord/).

Additionally, you can utilize the following resources to aid in troubleshooting and monitoring the migration process:

- **Validator Block Check:** You can refer to this tutorial to check if a validator is missing blocks, which can be helpful during the migration process: [Validator Block Check Tutorial](https://docs.icon.community/support/advanced-topics/validator-nodes/how-to-check-if-a-validator-is-missing-blocks).

- **Monitoring Tool:** To monitor the state of your node and ensure the migration is completed correctly, you can request access to the monitoring tool via the Discord channels: [Monitoring Tool](https://icon2.mon.solidwallet.io/).

- For further guidance on troubleshooting the migration process and executing the tasks described in this tutorial, the following documentation can be of assistance:

- [How to Set Up a Keystore on a Validator Node](https://docs.icon.community/support/advanced-topics/validator-nodes/how-to-setup-a-keystore-on-a-validator-node).

- [Node Operation and Configuration](https://docs.icon.community/support/advanced-topics/validator-nodes/node-operation-and-configuration).
