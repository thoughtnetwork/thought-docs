# Mining How To

## Mining mainnet

* Download latest thought wallet, either thoughtd (command line) or thought-qt (graphical) 
for your operating system (Ubuntu, Mac, Windows). Build from source or get builds here: https://github.com/thoughtnetwork/thought-wallet.

The graphical (thought-qt) has different commands than thoughtd (command line). For command line you need to use
thought-cli for command line commands, for graphical there are either graphical methods, 
or you can use the Debug console under "Tools".

* Install wallet
* Let sync with blockchain
* Encrypt wallet (under Settings, or encryptwallet "passphrase" commandline)
* Restart wallet

Locate your data directory as below
* Windows: C:\Users\YourUserName\Application data\ThoughtCore
* Linux: /home/YourUserName/.thoughtcore
* Mac: UserDirectory/Library/Application Support/ThoughtCore

Continue:
* Backup wallet.dat file (in your data directory see below
* See above, backup wallet.dat file again (we are not responsible for lost coins or lost wallets)
* Generate address (Click receive and "Request Payment", or getnewaddress on commandline)

In your data directory create or modify thought.conf file (choose your own user and password)
thought.conf:

``` 
server=1
addnode=phi.thought.live:10618
addnode=phee.thought.live:10618
rpcuser=youruser
rpcpassword=yourpassword
```

Restart wallet

Install java jre (https://java.com/en/download/manual.jsp)

Download latest build from jtminer-builds repository: https://github.com/thoughtnetwork/jtminer-builds.

Create jtminer.properties file:

```
host = localhost
port = 10617
user = (your user from thought.conf)
password = (your password from thought.conf
coinbase-addr = your address from above
threads = (number of threads can start with 2, but depends on your cpu)
```

Start jtminer (command line), note: jtminer won't connect to thought wallet until blockchain is synced
You can tell if it is synced by command line command getblockchaininfo
headers and blocks should both be the same height from explorer https://exp.thought.live/insight/

Jtminer start code should look something like this (use your own paths)

```
/usr/bin/java -jar /home/thought/jtminer/target/jtminer-0.2.1-SNAPSHOT-jar-with-dependencies.jar --config /etc/thought/jtminer.properties
```

Jtminer outputs to command line or system logs you can verify it's working 
Output should look like this
```
Jun 21 15:04:58  java[12050]: [2019-06-21 15:04:58] 186527 cycles, 9 solutions, 0 errors, 12.44 kilocycles/sec
Jun 21 15:05:12  java[12050]: [2019-06-21 15:05:12] Current block is 405032
Jun 21 15:05:13  java[12050]: [2019-06-21 15:05:13] 568414 cycles, 12 solutions, 0 errors, 37.91 kilocycles/sec
Jun 21 15:05:23  java[12050]: [2019-06-21 15:05:23] Current block is 405033
```

Block rewards go to the address you generated above

## Mining testnet

same as above but port in jtminer.properties is 11617 and add testnet=1 to thought.conf









