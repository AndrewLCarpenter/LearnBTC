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

	#Notice the blockhash and blockindex fields. (Our transaction was the 105 included in the block))


##Exploring blocks
Commands: getblock and getblockhash

Lets start with the transaction that we just completed. Use the getblock command with the block hash as the parameter

	bitcoin-cli getblock 00000000000000000269583cbe049e0b0aac580bb0621d2554006246671106ab


	{
	    "hash" : "00000000000000000269583cbe049e0b0aac580bb0621d2554006246671106ab",
	    "confirmations" : 9,
	    "size" : 934433,
	    "height" : 360935,
	    "version" : 3,
	    "merkleroot" : "3a1378f50f9ba2db4da018825388f6d6d6b74d24be17d67a03a81fd5304c73e4",
	    "tx" : [
		"a0dc1055730960ffe10dbfa6f434c6ae93df849c5e0fed3ebc7e9fcfb5994b63",
		"87e60c2f518ac4a92a7ab24a43ecbdc2d8fe6ffbd6fb807e13966a5e7d310919",
		"10e8d8b1ab07861420ee49b162448f4a6644746a9c1da9db91a6eba5885e6648",
		"09af4ce004b0c5912e779aad64b75453fc787b2d94f8b5926c4a5a00f6139b93",
		"341d549544387434e12792c39a79533f62e5dee8884594a3f0c51290869f4d0b",
		"70d7fb8b1871b9721ab11b5a69fdedf1daefac637a56746ee7eab0d761b3dc95",
		"b366e9badfc36a51ba00f2e441f86ac3f785d435f830c7c820d470883cee74cc",
		"2f70736424d92190d35a7d405d13c9366b5146b9789bccd4ca546d1d6051ff9c",
		................(Shit load of other transactions)..................     
		"38c423a64bc176dda7638186b6c9940fd5004cf68abf26922c656bb6ca0aae66",
		"923d093d19fde5795fbd3b77edc1a9bbb3aa8dcd8d67ded146ca06545953cca7",
		"1cee2ae18cfed1d385accb2de8ccfc7c6a12e19b0c6372ca181fbfa53e52d0d1",
		"c8561603df30bf7ccf46ad5f2e0902d49e7408c71522d12a3c722d06d468e689",
		"a19ddd4c0155f1b43097b4c4d4104b524c4e9252e7956e4532355597d9443a8f",
		"81e2bbbcc05a19c4257e6814754898321790a9b8bfa97eb63b4230104acd5384",
		"94a233337cd4798c54fe03704861f0ee00950128eac130e9887919446aba9c04",
		"0aca85d6160ba34d1fa4a15ede6310dabb2f3a9d6073e7fadab4d262e74065fa",
		"e2cc2f8870ae38d60d995fa69d851500ea7c0e1acaedd90a56e2e8932a338391",
		"badd22a697dd3e72c28b0c3c24c14f0994985db72ffef97ee4196da78ba0eb5e",
		"61422b9d8684df93b3f692c668e3c9ea75ab5bb083cb2a7919676bcc99e2b1a3"
	    ],
	    "time" : 1434305585,
	    "nonce" : 4124825129,
	    "bits" : "18162043",
	    "difficulty" : 49692386354.89383698,
	    "chainwork" : "00000000000000000000000000000000000000000007a6951e7550bcacc2f688",
	    "previousblockhash" : "00000000000000001360173ecbef99fd8731155cff220cc650bba8d587715b49",
	    "nextblockhash" : "000000000000000010caace07a9e1464aa9da458e049cfc9c63a37554ce939f0"
	}
~                                                                                                                







