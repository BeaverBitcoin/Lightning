# Bitcoin Lightning Setup Intstructions

Brought to you by [Beaver Bitcoin](www.beaverbitcoin.com)

## Important Resources

* [Setting up Bitcoin lightning Network Node on AWS - Bolaj](https://thebolaji.hashnode.dev/setting-up-bitcoin-lightning-network-node-on-aws)
* [Run LND from Bos](https://github.com/alexbosworth/run-lnd)
* [Github for LND](https://github.com/lightningnetwork/lnd)
* [Google Doc For Setting Up Bitcoin Full Node on Ubunut](https://docs.google.com/document/d/1onvHPDCM00BsEoKa4AXp8cvCwymSCp6q6s8pyKd4k9g/edit)
* [Beginner's Guide To Lightning](https://blog.bitfinex.com/trading/the-lightning-nodes-a-beginners-guide/)
* [LND and etcd](https://docs.lightning.engineering/lightning-network-tools/lnd/etcd)

## The steps we take

99% of it is directly from [Run LND from Bos](https://github.com/alexbosworth/run-lnd)

1. Launch Ubuntu EC2 Instance on AWS
    1. We went with t2.medium which is $0.0464 per hour or about $35 a month.
    1. Select and download a new keypair
    1. Standard gp2 drive of 8 GB and a gp3 volume of 700GB to store the full blockchain
    1. To connect to the host, you will use ssh -i ~/path_to_downloaded_pem_file ubuntu@IP_OF_INSTANCE
        1. sudo chmod 600 ~/PATH_TO_PEM_FILE - must do this first
1. Allocate an elastic ip under Network and Security in the EC2 console
1. Under actions on the elastic IP you can associate with the EC2 instance you spun up previously
1. Begin walking through [Initial Setup](https://github.com/alexbosworth/run-lnd#initial-setup)
    1. To change file descriptor limit, sudo is required. i.e. `sudo vi /etc/sysctl.conf`
    1. The `sudo file -s /dev/nvme1n1` command should actually be `sudo file -s /dev/<name_of_700G_block>`
        1. Similar with other references to `nvme1n1`
    1. Version of Bitcoin was 24.1 as of writing this guide so downloaded the github v24.1. `git clone -b v24.1 https://github.com/bitcoin/bitcoin.git`
1. To install Go only change is to use latest version which is 1.20.4
1. Also security group in AWS should allow inbound and outbound connections on port 9735 and 10009

## Setting up a channel

1. It is recommended to use the ranking on [Lightning Engineering's Ranks](https://terminal.lightning.engineering/)
    1. If you go to a given node (For example, [Kraken](https://terminal.lightning.engineering/02f1a8c87607f415c8f22c00593002775941dea48869ce23096af27b0cfdcc0b69/)), it will have the connect instructions listed.
        1. `lncli connect 02f1a8c87607f415c8f22c00593002775941dea48869ce23096af27b0cfdcc0b69@52.13.118.208:9735`
        1. `lncli newaddress p2tr`
        1. Send some sats to your wallet address which can be listed with `lncli wallet addresses list`
        1. Open a channel with `lncli openchannel 02f1a8c87607f415c8f22c00593002775941dea48869ce23096af27b0cfdcc0b69 <SATS>`
1. And you're good to go!!
