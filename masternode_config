# Thought Masternode Deployment

* Update to latest build - 0.17.1, thoughtd or thought-qt
* Encrypt wallet
* Backup wallet.dat
* Backup wallet.dat
* Make sure you have 314,000 THT available for stake
* Modify your thought.conf as per the below
* External IP needs to be the IP address your masternode will be seen as from the Internet
* Set extneral port to 10618
* From debug consolue or thought-cli perform command "masternode genkey"
* Put the output key into thought.conf under masternodeprivkey
* From debug consolue or thought-cli perform command "getnewaddress"
* From debug consolue or thought-cli perform command "sendtoaddress" (address you generated in above step) 314,000
* This amount 314,000 in the above step must be exact
* Ensure above transaction is confirmed
* From debug consolue or thought-cli perform command "masternode outputs"
* In masternode.conf put the following (there is an example template in file) "alias (you choose) externalip:10618 masternodeprivkey (from above) txid and output index (from masternode outputs)"
* Refer to format in example
* Restart thoughtd or thought-qt
* Wait until masternode syncs, can do command "mnsync status"
* Once sync is complete perform masternode "masternode start-alias" (alias you chose above)
* Perform command masternode status, should say "Masternode successfully started"

```thought.conf

server=1
rpcuser=xxx
rpcpassword=xxx
addnode=phee.thought.live:10618
masternode=1
masternodeprivkey=xxx
externalip=xxx:10618
```
