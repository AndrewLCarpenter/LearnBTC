#Installing the Bitcoin client and basic commands

##Downloading and Installing

    git clone github..... (you know where that is)
    git tag

Now choose your version of the client. (Note: the versions with suffix rc are RELEASE CANDIDATES)

In order to pick your favorite version:

    git checkout v0.9.....

Now build the client:

    ./autogen.sh
    ./configure  (There are a ton of options here. Maybe figure out what they mean?)
    (Use ./configure --with-incompatible-bdb if you get the Berkley error)

    make
    sudo make install
    
    which bitcoind	These commands will show you the location of the client. (Default: /usr/local/bin/)
    which bitcoin-cli


##Starting up
Upon starting your client you will need to set a user name and password. Copy whatever they suggest to ~/.bitcoin/bitcoin.conf
    
    bitcoind 	(starts your client)
    
    #Now you should receive the message about rpc username and password
    echo 'copied text' > ~/.bitcoin/bitcoin.conf

Now your client is ready to work

	bitcoind -daemon	#starts the client in the background

Use bitcoin-cli (bitcoin command line interface) in order to interact with the server

	bitcoin-cli help 	#displays a help page with list of available commands (there are a lot)
	bitcoin-cli getinfo 	#Use this to check on your clients progress in downloading the blockchain


##Encryption
Your download is finished and you have a wallet. Sick. Encryption time.

	bitcoin-cli encryptwallet foo 	(foo is the password)

Restart your bitcoind instance and you should see the 'unlocked_until' stat when you pass

	bitcoin-cli getinfo
	bitcoin-cli walletpassphrase foo 360 	(Adds 360 seconds to the lock timer)


##Wallet Backup, Plain-text Dump, and Restore
Relevant commands: backupwallet, importwallet, dumpwallet

	bitcoin-cli backupwallet wallet.backup
	#You can load it back in with:
	bitcoin-cli importwallet wallet.backup

##Your first transaction
This assumes that you have bitcoin in an external wallet (ex: Coinbase)

	bitcoin-cli getnewaddress	#Send your bitcoin to the output of this guy
	bitcoin-cli getreceivedbyaddress *address from the previous line* 0 	# this will show how much that address is holding
Note: the 0 tells the client how many confirmations are required. (If we omit the zero it wont show any balance because we just initiated the transaction)

	bitcoin-cli getaddressesbyaccount ''
	#lists all of the addresses that we have


##Exploring and Decoding Transactions
Time to explore the transaction that we just completed last section

	bitcoin-cli listtransactions 	#this will list all of the transactions that have been completed
	
	#Now take the txid from the previous command and paste into here:
	bitcoin-cli gettransaction 6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2
	
Here is an example of inputting the hex value into decoderawtransaction in order to explore the transaction further:
	bitcoin-cli decoderawtransaction *hex string*

		{
	    "txid" : "6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2",
	    "version" : 1,
	    "locktime" : 0,
	    "vin" : [	##Everything in these bracket block are the inputs for our transaction.
	        {
	            "txid" : "30a9b2a7ed537da12992e6b7fb117a62891bd6502de1e1d0a06bca9326c3a171",
	            "vout" : 0,
	            "scriptSig" : {
	                "asm" : "304402201404d230ab149f8b858d1573f4c2d501d58236131ff925e237ab8b366f71dd0c022017e43c95d82d9ed6914e4456f0aa529255ab35f213972979436456f61c93d5be01 0347727b56202df3874d9c655636fbb9fff137d9894938c50bdbdb8b446d10659c",
	                "hex" : "47304402201404d230ab149f8b858d1573f4c2d501d58236131ff925e237ab8b366f71dd0c022017e43c95d82d9ed6914e4456f0aa529255ab35f213972979436456f61c93d5be01210347727b56202df3874d9c655636fbb9fff137d9894938c50bdbdb8b446d10659c"
	            },
	            "sequence" : 4294967295
	        },
	        {
	            "txid" : "a8dd3a03ed3da5c64e6503253afcba2fd00596914b376f0c606f05709b686dd6",
	            "vout" : 1,
	            "scriptSig" : {
	                "asm" : "3045022100cc1be702fa94f8c0ff8e7cee219040d0de8b108138117036d2b15d455efde2c202201e9901128fabbb6c064d244e217e1b3f811d0650a2772ce6f36197b9a841d61401 02dd91e5b0edf062d453e109da0aee1f4abec97bbd5359b4f43f7d8a1f15196833",
	                "hex" : "483045022100cc1be702fa94f8c0ff8e7cee219040d0de8b108138117036d2b15d455efde2c202201e9901128fabbb6c064d244e217e1b3f811d0650a2772ce6f36197b9a841d614012102dd91e5b0edf062d453e109da0aee1f4abec97bbd5359b4f43f7d8a1f15196833"
	            },
	            "sequence" : 4294967295
	        }
	    ],
   	 "vout" : [
   	     {
   	         "value" : 0.05000000,
   	         "n" : 0,
   	         "scriptPubKey" : {
   	             "asm" : "OP_DUP OP_HASH160 afd8779e66162c542e91e9176bade5ed803668b6 OP_EQUALVERIFY OP_CHECKSIG",
   	             "hex" : "76a914afd8779e66162c542e91e9176bade5ed803668b688ac",
   	             "reqSigs" : 1,
   	             "type" : "pubkeyhash",
   	             "addresses" : [
   	                 "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om"
   	             ]
   	         }
   	     },
   	     {
   	         "value" : 0.00020822,
   	         "n" : 1,
   	         "scriptPubKey" : {
   	             "asm" : "OP_DUP OP_HASH160 4fba505af60fd1cb2638b59d888825ac8ad37e8a OP_EQUALVERIFY OP_CHECKSIG",
   	             "hex" : "76a9144fba505af60fd1cb2638b59d888825ac8ad37e8a88ac",
   	             "reqSigs" : 1,
   	             "type" : "pubkeyhash",
  	     	        "addresses" : [
        	            "18GZaUHiHNfsbWcTtqJr3xFn3NsuZw4j7w"
        	        ]
        	    }
        	}
   	 ]
	}



Alright. We have explored our transaction. We could theoretically do this this all the way back to the genesis block by viewing transaction info our inputs, then the info of their inputs, etc...

Once the transaction we received has been included in a block, we will get more info from the gettransaction command. (ie the blockhash or identifier of the transaction)

	bitcoin-cli gettransaction 6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2
'''	
{
    "amount" : 0.05000000,
    "confirmations" : 6,
    "blockhash" : "00000000000000000269583cbe049e0b0aac580bb0621d2554006246671106ab",
    "blockindex" : 105,
    "blocktime" : 1434305585,
    "txid" : "6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2",
    "walletconflicts" : [
    ],
    "time" : 1434305519,
    "timereceived" : 1434305519,
    "details" : [
        {
            "account" : "",
            "address" : "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om",
            "category" : "receive",
            "amount" : 0.05000000
        }
    ],
    "hex" : "010000000271a1c32693ca6ba0d0e1e12d50d61b89627a11fbb7e69229a17d53eda7b2a930000000006a47304402201404d230ab149f8b858d1573f4c2d501d58236131ff925e237ab8b366f71dd0c022017e43c95d82d9ed6914e4456f0aa529255ab35f213972979436456f61c93d5be01210347727b56202df3874d9c655636fbb9fff137d9894938c50bdbdb8b446d10659cffffffffd66d689b70056f600c6f374b919605d02fbafc3a2503654ec6a53ded033adda8010000006b483045022100cc1be702fa94f8c0ff8e7cee219040d0de8b108138117036d2b15d455efde2c202201e9901128fabbb6c064d244e217e1b3f811d0650a2772ce6f36197b9a841d614012102dd91e5b0edf062d453e109da0aee1f4abec97bbd5359b4f43f7d8a1f15196833ffffffff02404b4c00000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac56510000000000001976a9144fba505af60fd1cb2638b59d888825ac8ad37e8a88ac00000000"
}
'''








