# ICON2.0 Migration Guide

## Important <a href="docs-internal-guid-8fefb1fa-7fff-cca3-4ba3-bc0c7975719e" id="docs-internal-guid-8fefb1fa-7fff-cca3-4ba3-bc0c7975719e"></a>

{% hint style="warning" %}
* The ICON1 node server must be maintained after STAGE3 until the migration is completed.
* It should be turned off when the Foundation announces that the migration is complete.
* If you are P-Rep, you must use the Keystore file used in ICON1.
  * _If you’re not using a dual key setup, use your original registration key._
  * _If you’re using a dual key setup, use the second key (not the registration key)._
{% endhint %}

## Migration Plan

{% hint style="danger" %}
Top 30 P-Reps participating in Stage2 must fill up this form before starting [https://forms.gle/PvyMVDH98DBzn2op7](https://forms.gle/PvyMVDH98DBzn2op7)

Before Stage2, top 30 P-Reps must update to the latest ICON1 node version (iconloop/prep-node:20211012.0).
{% endhint %}

* Other node operators can wait for further instructions to setup ICON2 - goloop node once the network migration is fully completed. Until then please keep your ICON1 node running as before.
* There are three stages in the migration.

### Summary of stages

* ~~\[Completed] STAGE1: Migrate data from ICON1.~~
* ~~\[Complete] STAGE2: Download the migrated data and Start an ICON2 node. \[Action required - PRep]~~
* **\[In Progress] STAGE3: \[Action required - PRep and Exchanges ]**
  * Send a Proposal (Foundation) → Vote a Proposal (PReps) → Complete consensus
  * The Foundation announces the changes. ICON1 node will be stopped.\
    Exchanges should stop deposits and withdrawals. \[Action required - Exchanges ]
  * Switch to the ICON2 network
  * Complete migration

### Timetable of migration

| Stage time (KST)                                                                                        | Actions                                                                                          |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| <p><del>10/25 10:00 | Stage 1-2</del></p><p><del>Who: <em>Foundation</em></del></p>                     | ~~Backup the current snapshot db and upload~~                                                    |
| <p><del>10/26</del></p><p><del>Who: <em>Foundation</em></del></p>                                       | ~~Test with migrated real ICON2 data~~                                                           |
| <p><del>10/26</del></p><p><del>Who: <em>Foundation</em></del></p>                                       | ~~Deploy to guide documents~~                                                                    |
| <p><del>10/27 11:00 | Stage2</del></p><p><del>Who: <em>P-Rep</em></del></p>                             | ~~Start the stage2~~                                                                             |
| <p><del>11/03 22:00 | Stage3-1</del></p><p><del>Who: Exchange / Citizen</del></p>                       | ~~Stop deposits and withdrawals on exchanges. Stop all ICON1 citizens~~                          |
| <p><del>11/03 23:00 | Stage3-2</del></p><p><del>Who: <em>Foundation</em></del></p>                      | ~~Send a proposal.~~                                                                             |
| <p><del>11/03 23:30 | Stage3-3</del></p><p><del>Who: <em>P-Rep</em></del></p>                           | ~~Wait for consensus proposal to be completed.~~                                                 |
| <p><del>11/03 23:40 | Stage 3-4</del></p><p><del><em>Who: P-Rep</em></del></p>                          | ~~ICON1 node and Citizen node will enter `suspend` state~~                                       |
| <p><del>11/03 ~23:59 | Stage3-5</del></p><p><del>Who: <em>P-Rep</em></del></p>                          | ~~Wait for all ICON2 node reached to `import_icon xxx finished` state~~                          |
| <p><del>11/04 00:00 | <strong>Announcement</strong></del></p><p><del>Who: Foundation</del></p>          | ~~The Foundation notifies P-Reps to enter a command: `chain stop`~~                              |
| <p><del>11/04 00:00 | Stage3-6</del></p><p><del>Who: <em>P-Re</em>p</del></p>                           | ~~\[**Action required**] Command to confirm end of migration. see the next section (Stage 3-6)~~ |
| <p><del>11/04 00:30</del></p><p><del><em>Who: Foundation</em></del></p>                                 | ~~Wait for all ICON2 nodes reached `stopped` state~~                                             |
| <p><del>11/04 00:30</del></p><p><del><em>Who: Foundation</em></del></p>                                 | ~~Stop Migrators~~                                                                               |
| <p><del>11/04 01:00</del></p><p><del><em>Who: Foundation</em></del></p>                                 | ~~Start Migrators~~                                                                              |
| <p><del>11/04 01:10 | <strong>Announcement</strong></del></p><p><del><em>Who: Foundation</em></del></p> | ~~The Foundation notifies P-Reps to enter a command: `chain start`~~                             |
| <p><del>11/04 01:10 | Stage3-7</del></p><p><del>Who: <em>P-Rep</em></del></p>                           | ~~\[**Action required**] Command to confirm end of migration. see the next section (Stage 3-7)~~ |
| <p><del>11/04 01:10 | Stage 3-8</del></p><p><del>Who: P-Rep &#x26; Foundation</del></p>                 | ~~Monitor whether consensus is reached and blocks are created~~                                  |
| <p>11/04 01:40</p><p>Who: Foundation</p>                                                                | Start Citizen node                                                                               |
| <p>11/04 02:00 | <strong>Announcement</strong></p><p><em>Who: Foundation</em></p>                       | Announce recovery endpoint.                                                                      |
| <p>11/04 02:00 |</p><p><em>Who: Foundation</em></p>                                                     | Upload the latest ICON2 DB. We'll let you know, If we ready.                                     |
| <p>11/04 02:00 | <strong>Announcement</strong></p><p>Who: <em>Foundation</em></p>                       | Announce a successful migration.                                                                 |
| Migration complete                                                                                      | Migration complete                                                                               |
| <p>11/04 05:00</p><p>Who: <em>Exchange / Citizen</em></p>                                               | <p>If you run a citizen,</p><p>Download a new ICON2 snapshot and run the node.</p>               |

## Migration details

{% hint style="danger" %}
Top 30 P-Reps **must** update their loopchain node before starting Stage2. This update will make ICON1 stop creating blocks after Revision 13.
{% endhint %}

Please update citizen node following guideline before October 26th. Please try to update your node right after your leader turn if you're Main P-Rep.

* Using docker
  * Check your docker image tag settings in docker-compose.yml and change the tag to below
    * `image: iconloop/prep-node:20211012.0`
  * Enter the following commands in order

```
docker-compose pull
docker-compose down
docker-compose up -d
```

* Using snap
  * [https://snapcraft.io/loopchain](https://snapcraft.io/loopchain)
* Using citizen pack or binary
  * [https://github.com/icon-project/icon-release/releases/tag/20211012.0](https://github.com/icon-project/icon-release/releases/tag/20211012.0)

### STAGE1

Migrate data to 5 node servers provided by the Foundation. When the current block height of ICON1 is reached, upload a backup file of migrated ICON2 data. We tested the compressed backup file and original file as follows.&#x20;

|                                     | Using compressed database file                                                                                                            | Using original database file                                                                                                         |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Information of migrated backup file | <p>file size : 1.2TB over<br>file count: 31,000 over files</p>                                                                            |                                                                                                                                      |
| Download time                       | <ul><li>900GB (compress with zstd)</li><li>02 hour 08 min (1Gbps)</li><li>01 hour 04 min (2Gbps)</li><li>00 hour 32 min (4Gbps)</li></ul> | <ul><li>1.2 TB (uncompressed)</li><li>02 hour 51 min (1Gbps)</li><li>01 hour 25 min (2Gbps)</li><li>00 hour 42 min (4Gbps)</li></ul> |
| Decompress time                     | <ul><li>900GB (compress with zstd on c5.4xlarge)</li><li>02 hour 10 min</li></ul>                                                         | 0 min                                                                                                                                |
| Total estimated time                | <ul><li>04 hour 18 min (1Gbps)</li><li>03 hour 14 min (2Gbps)</li><li>02 hour 46min (4Gbps)</li></ul>                                     | <ul><li>02 hour 51 min (1Gbps)</li><li>01 hour 25 min (2Gbps)</li><li>00 hour 42 min (4Gbps)</li></ul>                               |



### STAGE2

STAGE2 Each PRep prepares a new server for ICON2 and copy the Keystore file from another ICON1 server.

If you are P-Rep, you must use the Keystore file used in ICON1.

#### Action items of STAGE2 for P-Reps

1. Provide the new ICON2 node’s Public IP address for monitoring [https://forms.gle/PvyMVDH98DBzn2op7](https://forms.gle/PvyMVDH98DBzn2op7)
2. Prepare ICON2 node ([https://github.com/icon-project/icon2-docker](https://github.com/icon-project/icon2-docker))
   1. Create a new server for ICON2
   2. Copy keystore file from ICON1 node to ICON2 node
   3. Download the ICON2 image or build the image. (`iconloop/icon2-node`)
   4. Start the new ICON2 node
   5. The following tasks are automatically executed as environment variables of docker
      1. Download the migrated ICON2 block data
      2. Start ICON2 node in container
      3. Join the ICON2 Network
      4. Start import (The blocks after the backup DB are additionally synchronized)
   6. Monitor whether it reaches latest height of ICON1 with below command
3. Wait and keep the icon1

```
# docker exec -it icon2-node goloop chain ls
[
  {
    "cid": "0x1",
    "nid": "0x1",
    "channel": "icon_dex",
    "state": "import_icon 39314952 running",
    "height": 4830,
    "lastError": ""
  }
]
```

### STAGE3

Switch the ICON2 Network

#### Action items of STAGE3

{% hint style="danger" %}
During this last stage, exchanges should stop deposits and withdrawals.

The Foundation will let you know when you can setup your ICON2 citizen nodes. Once you are ready, you can resume normal operations.
{% endhint %}

#### Instructions for the ICON Foundation

| Stage 3-1        | <p>Block the network endpoint(<a href="http://ctz.solidwallet.io">ctz.solidwallet.io</a>, wallet.icon.foundation) and prevent the creation of new Transactions.<br>Stop all ICON1 citizens.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| STAGE3-2         | <p></p><p>Send a transaction for a new revision (ICON2 Revision).</p><ul><li>Send a Proposal</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| STAGE3-3         | Wait for consensus proposal to be completed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| STAGE3-4         | ICON1 node and Citizen node will enter `suspend` state.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| STAGE3-5         | <p>ICON2 node is changed to <code>import_icon xxx finished</code> state.</p><p>Wait for all ICON2 nodes reach to the target.<br>a. ICON1 consensus will stop at the target.<br>b. ICON2 import will stop at the target</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **Announcement** | **The Foundation notifies PReps to enter a command**: `chain stop`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| STAGE3-6         | <p>Enter the following command <strong>to confirm end of migration</strong></p><p></p><p><code>$ docker exec -it icon2-node goloop chain ls</code></p><p><code></code></p><p><code>[ { "cid": "0x1", "nid": "0x1", "channel": "icon_dex", "state": "import_icon 40123455 finished", "height": 6830, "lastError": "" } ]</code></p><p></p><p>Stop ICON2 migration task</p><ul><li><p>Stop chain</p><p><code>docker exec -it icon2-node goloop chain stop 0x1</code></p></li></ul><p>Then ICON2 will fall in <code>import_icon finished</code> status after erasing all temporal data. To see the last block height, you need to stop again.</p><ul><li><p>Stop chain</p><p><code>docker exec -it icon2-node goloop chain stop 0x1</code></p></li></ul><p>Wait for all ICON2 nodes reach to the target.<br></p> |
| **Announcement** | **The Foundation notifies PReps to enter a command**: `chain start`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| STAGE3-7         | <p>Start chain</p><p></p><p><code>docker exec -it icon2-node goloop chain start 0x1</code></p><p></p><p>Switch the ICON2 network and Start ICON2 consenesus.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| STAGE3-8         | PRep Completed migrations                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| STAGE3-9         | Foundation will make backup data with backup node(ICON2 Real data). We'll let you know If we're ready.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **Announcement** | Announce a successful migration                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| End of migration | :rocket:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

#### Instructions for P-Reps

| Stage 3-1        | If you are running a citizen node, stop it.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| STAGE3-2         | <p>Vote on the proposal with preptools.</p><p></p><p>P-Reps can't vote on icon.community. Use preptools to change the endpoint to your own ICON1 node P-Reps and vote.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| STAGE3-3         | Wait for consensus proposal to be completed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| STAGE3-4         | ICON1 node and Citizen node will enter `suspend` state.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| STAGE3-5         | <p>ICON2 node is changed to <code>import_icon xxx finished</code> state.</p><p>Wait for all ICON2 nodes reach to the target.<br>a. ICON1 consensus will stop at the target.<br>b. ICON2 import will stop at the target</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **Announcement** | **The Foundation notifies PReps to enter a command**: `chain stop`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| STAGE3-6         | <p>Enter the following command <code>to confirm end of migration</code><br></p><p><code>$ docker exec -it icon2-node goloop chain ls</code></p><p><code></code></p><p><code>[ { "cid": "0x1", "nid": "0x1", "channel": "icon_dex", "state": "import_icon 40123455 finished", "height": 6830, "lastError": "" } ]</code></p><p></p><p>Stop ICON2 migration task</p><ul><li><p>Stop chain</p><p><code>docker exec -it icon2-node goloop chain stop 0x1</code></p></li></ul><p>Then ICON2 will fall in “import_icon finished” status after erasing all temporal data. To see the last block height, you need to stop again.</p><ul><li><p>Stop chain</p><p><code>docker exec -it icon2-node goloop chain stop 0x1</code></p></li></ul><p>Wait for all ICON2 nodes reach to the target.</p> |
| **Announcement** | **The Foundation notifies PReps to enter a command**: `chain start`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| STAGE3-7         | <p>Start chain</p><p></p><p><code>docker exec -it icon2-node goloop chain start 0x1</code></p><p></p><p>Switch the ICON2 network and Start ICON2 consenesus.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| STAGE3-8         | PRep Completed migrations                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| STAGE3-9         | Monitor whether consensus is reached and blocks are created.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Announcement** | Shut down old ICON1 server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| End of migration | :rocket:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

#### Instructions for Citizen node (Exchanges)

| Stage 3-1        | Exchanges should stop deposits and withdrawals.                                                                                                |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| STAGE3-2         | No action is required.                                                                                                                         |
| STAGE3-3         | No action is required.                                                                                                                         |
| STAGE3-4         | ICON1(Citizen) node will enter `suspend` state. ICON1 citizen node can’t generate the new block. But It is possible to query the transactions. |
| STAGE3-5         | No action is required.                                                                                                                         |
| **Announcement** | **The Foundation notifies PReps to enter a command**: `chain stop`                                                                             |
| STAGE3-6         | <p>Wait and keep the icon1 citizen node.</p><p>No action is required.</p>                                                                      |
| **Announcement** | **The Foundation notifies PReps to enter a command**: `chain start`                                                                            |
| STAGE3-7         | <p>Wait and keep the icon1 citizen node.</p><p>No action is required.</p>                                                                      |
| STAGE3-8         | <p>Wait and keep the icon1 citizen node.</p><p>No action is required.</p>                                                                      |
| STAGE3-9         | If we ready, let you know. Recover citizens with the backup. Start the ICON2 node.                                                             |
| **Announcement** | If you are ready, please resume deposits and withdrawals.                                                                                      |
| End of migration | :rocket:                                                                                                                                       |

#### &#x20;
