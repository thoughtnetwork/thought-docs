# Masternode Registration from Thought Core
This documentation describes the procedure to register an existing masternode for the DIP003 masternode list if the collateral is held in the Thought Core software full wallet. It is not possible to issue the registration transactions if DIP003 is not yet active. The commands are shown as if they were entered in the Thought Core GUI by opening the console from Tools > Debug console, but the same result can be achieved on a masternode by entering the same commands and adding the prefix ~/.thoughtcore/thought-cli to each command.
# Generate Masternode Collateral Funding Transaction
If you already have a non-deterministic masternode collateral funding transaction ID generated find the output transaction either in your masternode.conf file or with the following cli command on an active masternode:

`masternode outputs`

If you do not already have a masternode collateral funding transaction ID, create a new address and send exactly 314,000 THT to that address via gui or cli as follows:

`getnewaddress`

`sendtoaddress [address from above] 314000`

[Note if you are sending from a mining address you may need to consolidate mining coinbase transactions into larger inputs such as sets of 100,000 THT]

Record the resulting transaction ID. This is your masternode collatoral funding transaction ID

# Generate a BLS key pair
A public/private BLS key pair is required for the operator of the masternode. If you are using a hosting service, they will provide you with their public key, and you can skip this step. If you are hosting your own masternode, generate a BLS public/private keypair as follows:

`bls generate`

`{
  "secret": "565950700d7bdc6a9dbc9963920bc756551b02de6e4711eff9ba6d4af59c0101",
  "public": "01d2c43f022eeceaaf09532d84350feb49d7e72c183e56737c816076d0e803d4f86036bd4151160f5732ab4a461bd127"
}`

These keys are NOT stored by the wallet and must be kept secure, similar to the value provided in the past by the masternode genkey command.
#Add the private key to your masternode configuration
The public key will be used in following steps. The BLS secret key must be entered in the thought.conf file on the masternode. This allows the masternode to watch the blockchain for relevant Pro*Tx transactions, and will cause it to start serving as a masternode when the signed ProRegTx is broadcast by the owner (final step below). Log in to your masternode using ssh or PuTTY and edit the configuration file on your masternode as follows:

`nano ~/.thoughtcore/thought.conf`

The editor appears with the existing masternode configuration. Add this line to the end of the file, replacing the key with your BLS secret key generated above:

`masternodeblsprivkey=565950700d7bdc6a9dbc9963920bc756551b02de6e4711eff9ba6d4af59c0101`

Do not delete your old masternodeprivkey, since this is still needed while the network is in transition to the new list. Press enter to make sure there is a blank line at the end of the file, then press Ctrl + X to close the editor and Y and Enter save the file. We now need to restart the masternode for this change to take effect. Enter the following commands, waiting a few seconds in between to give Thought Core time to shut down:

`~/.thoughtcore/thought-cli stop
sleep 5
~/.thoughtcore/thoughtd`

We will now prepare the transaction used to register a DIP003 masternode on the network.

#Prepare a ProRegTx transaction
First, we need to get a new, unused address from the wallet to serve as the owner address. This is different to the collateral address. It must also be used as the voting address if Spork 15 is not yet active. Generate a new address as follows:

`getnewaddress`

`yc98KR6YQRo1qZVBhp2ZwuiNM7hcrMfGfz`

Then either generate or choose an existing second address to receive the owner’s masternode payouts. It is also possible to use an address external to the wallet:
`getnewaddress`

`ycBFJGv7V95aSs6XvMewFyp1AMngeRHBwy`

You can also optionally generate and fund a third address to pay the transaction fee. If you selected an external payout address, you must specify a fee source address. Either the payout address or fee source address must have enough balance to pay the transaction fee, or the final register_submit transaction will fail.
The private keys to the owner and fee source addresses must exist in the wallet submitting the transaction to the network. If your wallet is protected by a password, it must now be unlocked to perform the following commands. Unlock your wallet for 5 minutes:

`walletpassphrase yourSecretPassword 300`

We will now prepare an unsigned ProRegTx special transaction using the protx register_prepare command. This command has the following syntax:

`protx register_prepare collateralHash collateralIndex ipAndPort ownerKeyAddr operatorPubKey votingKeyAddr operatorReward payoutAddress (feeSourceAddress)`

Open a text editor such as notepad to prepare this command. Replace each argument to the command as follows:

    * collateralHash: The txid of the 314000 Thought collateral funding transaction (get this from non-deterministic masternodes using `masternode outputs`)
    * collateralIndex: The output index of the 314000 Thought funding transaction (get this from non-deterministic masternodes using `masternode outputs`)
    * ipAndPort: Masternode IP address and port, in the format x.x.x.x:yyyy
    * ownerKeyAddr: The new Thought address generated above for the owner/voting address
    * operatorPubKey: The BLS public key generated above (or provided by your hosting service)
    * votingKeyAddr: [[Important, this address needs to be the same as ownerKeyAddr above]] The new Thought address generated above, or the address of a delegate, used for proposal voting 
    * operatorReward: The percentage of the block reward allocated to the operator as payment
    * payoutAddress: A new or existing Thought address to receive the owner’s masternode rewards
    * feeSourceAddress: An (optional) address used to fund ProTx fee. payoutAddress will be used if not specified.
    
Note that the operator is responsible for specifying their own reward address in a separate update_service transaction if you specify a non-zero operatorReward. The owner of the masternode collateral does not specify the operator’s payout address.
Example (remove line breaks if copying):

`protx register_prepare 2c499e3862e5aa5f220278f42f9dfac32566d50f1e70ae0585dd13290227fdc7 1 140.82.59.51:9999 yc98KR6YQRo1qZVBhp2ZwuiNM7hcrMfGfz 01d2c43f022eeceaaf09532d84350feb49d7e72c183e56737c816076d0e803d4f86036bd4151160f5732ab4a461bd127 yc98KR6YQRo1qZVBhp2ZwuiNM7hcrMfGfz 0 ycBFJGv7V95aSs6XvMewFyp1AMngeRHBwy`

Output:

`{
  "tx": "030001000191def1f8bb265861f92e9984ac25c5142ebeda44901334e304c447dad5adf6070000000000feffffff0121dff505000000001976a9149e2deda2452b57e999685cb7dabdd6f4c3937f0788ac00000000d1010000000000c7fd27022913dd8505ae701e0fd56625c3fa9d2ff47802225faae562389e492c0100000000000000000000000000ffff8c523b334e1fad8e6259e14db7d05431ef4333d94b70df1391c601d2c43f022eeceaaf09532d84350feb49d7e72c183e56737c816076d0e803d4f86036bd4151160f5732ab4a461bd127ad8e6259e14db7d05431ef4333d94b70df1391c600001976a914adf50b01774202a184a2c7150593442b89c212e788acf8d42b331ae7a29076b464e61fdbcfc0b13f611d3d7f88bbe066e6ebabdfab7700", "collateralAddress": "yPd75LrstM268Sr4hD7RfQe5SHtn9UMSEG", "signMessage": "ycBFJGv7V95aSs6XvMewFyp1AMngeRHBwy|0|yc98KR6YQRo1qZVBhp2ZwuiNM7hcrMfGfz|yc98KR6YQRo1qZVBhp2ZwuiNM7hcrMfGfz|54e34b8b996839c32f91e28a9e5806ec5ba5a1dadcffe47719f5b808219acf84"
  }`
  
Next we will use the collateralAddress and signMessage fields to sign the transaction, and the output of the tx field to submit the transaction.

#Sign the ProRegTx transaction
Now we will sign the content of the signMessage field using the private key for the collateral address as specified in collateralAddress. Note that no internet connection is required for this step, meaning that the wallet can remain disconnected from the internet in cold storage to sign the message. In this example we will again use Thought Core, but it is equally possible to use the signing function of a hardware wallet. The command takes the following syntax:

`signmessage collateralAddress signMessage`

Example:

`signmessage yPd75LrstM268Sr4hD7RfQe5SHtn9UMSEG ycBFJGv7V95aSs6XvMewFyp1AMngeRHBwy|0|yc98KR6YQRo1qZVBhp2ZwuiNM7hcrMfGfz|yc98KR6YQRo1qZVBhp2ZwuiNM7hcrMfGfz|54e34b8b996839c32f91e28a9e5806ec5ba5a1dadcffe47719f5b808219acf84`

Output:

`IMf5P6WT60E+QcA5+ixors38umHuhTxx6TNHMsf9gLTIPcpilXkm1jDglMpK+JND0W3k/Z+NzEWUxvRy71NEDns=`

#Submit the signed message

We will now create the ProRegTx special transaction to register the masternode on the blockchain. This command must be sent from a Thought Core wallet holding a balance, since a standard transaction fee is involved. The command takes the following syntax:

`protx register_submit tx sig`

Where:
    * tx: The serialized transaction previously returned in the tx output field from the protx register_prepare command
    * sig: The message signed with the collateral key from the signmessage command

Example:

`protx register_submit 030001000191def1f8bb265861f92e9984ac25c5142ebeda44901334e304c447dad5adf6070000000000feffffff0121dff505000000001976a9149e2deda2452b57e999685cb7dabdd6f4c3937f0788ac00000000d1010000000000c7fd27022913dd8505ae701e0fd56625c3fa9d2ff47802225faae562389e492c0100000000000000000000000000ffff8c523b334e1fad8e6259e14db7d05431ef4333d94b70df1391c601d2c43f022eeceaaf09532d84350feb49d7e72c183e56737c816076d0e803d4f86036bd4151160f5732ab4a461bd127ad8e6259e14db7d05431ef4333d94b70df1391c600001976a914adf50b01774202a184a2c7150593442b89c212e788acf8d42b331ae7a29076b464e61fdbcfc0b13f611d3d7f88bbe066e6ebabdfab7700 IMf5P6WT60E+QcA5+ixors38umHuhTxx6TNHMsf9gLTIPcpilXkm1jDglMpK+JND0W3k/Z+NzEWUxvRy71NEDns=`

Output:

`9f5ec7540baeefc4b7581d88d236792851f26b4b754684a31ee35d09bdfb7fb6`

Your masternode is now upgraded to DIP003 and will appear on the Deterministic Masternode List after the transaction is mined to a block. You can view this list on the Masternodes -> DIP3 Masternodes tab of the Thought Core wallet, or in the console using the command protx list valid, where the txid of the final protx register_submit transaction identifies your DIP003 masternode. Note again that all functions related to DIP003 will only take effect once Spork 15 is enabled on the network. You can view the spork status using the spork active command.
