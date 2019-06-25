# How to Migrate v16 wallet to v17 wallet

* start v16 wallet e.g. thought-qt -datadir=.thought -wallet=(yourwalletname)
* go to debug console (or use thought-cli)
* type walletpassphrase "yourpassphrase" 360 (360 in seconds amount of time to unlock)
* type dumpwallet "dumpwalletfilename" (caution this puts a plaintext of your public/private keys in your data directory
* close v16 wallet
* run v17 wallet e.g. thought-qtv17 -datadir=.thoughtcore
* copy dumpwalletfile to .thoughcore data directory
* go to debug console (or thought-cli)
* type importwallet "dumpwalletfilename"
* encryptwallet "yourpassword" (CAUTION: If using command line thought-cli make sure your wallet passphrase is not cached in commandline history
* IMPORTANT: Delete dumpwalletfile completely (check trash bin)
* keep backups of old v16 wallet file and new v17 wallet file
* backup both v16 and v17 wallet files
* backup both v16 and v17 wallet files
