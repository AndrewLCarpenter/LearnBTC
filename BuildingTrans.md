#Building a transaction?

Show all of the unspent CONFIRMED inputs to our wallet:

	$ bitcoin-cli listunspent
	
	[
	    {
		"txid" : "6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2",
		"vout" : 0,
		"address" : "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om",
		"account" : "",
		"scriptPubKey" : "76a914afd8779e66162c542e91e9176bade5ed803668b688ac",
		"amount" : 0.05000000,
		"confirmations" : 9
	    }
	]




Transactions use previously created outputs as their inputs by referring to them by the previous txid and vout index. Now lets create a transaction that will spend the 0th vout of txid 6857... as its ipnut and assign it to a new output that sends value to a new address:

	$ bitcoin-cli gettxout 6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2 0
	
	{
	    "bestblock" : "00000000000000000db7ef23f1376fe0ba65dee6e556dd588e041af341fb8aa6",
	    "confirmations" : 10,
	    "value" : 0.05000000,
	    "scriptPubKey" : {
		"asm" : "OP_DUP OP_HASH160 afd8779e66162c542e91e9176bade5ed803668b6 OP_EQUALVERIFY OP_CHECKSIG",
		"hex" : "76a914afd8779e66162c542e91e9176bade5ed803668b688ac",
		"reqSigs" : 1,
		"type" : "pubkeyhash",
		"addresses" : [
		    "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om"
		]
	    },
	    "version" : 1,
	    "coinbase" : false
	}

Above, is the output that assigned 50 millibits to our address, 1H2..., that we created earlier. In order to spend this output lets create a new transaction. After we generate a new address, we will send .025 BTC to our new address and .0245 back to ourselves as change. The difference will be taken by the miners as a transaction fee.
	
	#First create a new address to send some money to:
	$bitcoin-cli getnewaddress
	1KV5BvGiMcRuVmkzSaZRMQCTAB2iUATt7t
	
	#The createrawtransaction command produces a raw hex string that encodes the trasaction details we supplied.
	$ bitcoin-cli createrawtransaction '[{"txid": "6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2", "vout":0}]' '{"1KV5BvGiMcRuVmkzSaZRMQCTAB2iUATt7t":0.025, "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om":0.0245}'
	0100000001d22b6563c44e3e42da803d0f03c1c3aabfdb62e6dfd817b55e8148d1ffcd57680000000000ffffffff02a0252600000000001976a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac50622500000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac00000000

We can confirm that we encoded everything correctly by using the decoderawtransaction command:
	
	$ bitcoin-cli decoderawtransaction 0100000001d22b6563c44e3e42da803d0f03c1c3aabfdb62e6dfd817b55e8148d1ffcd57680000000000ffffffff02a0252600000000001976a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac50622500000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac00000000
	{
	    "txid" : "1e1dd43d8da43f4dbdb170f470a77ac7d210e043461c46082f2a18df217b62cb",
	    "version" : 1,
	    "locktime" : 0,
	    "vin" : [
		{
		    "txid" : "6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2",
		    "vout" : 0,
		    "scriptSig" : {
			"asm" : "",
			"hex" : ""
		    },
		    "sequence" : 4294967295
		}
	    ],
	    "vout" : [
		{
		    "value" : 0.02500000,
		    "n" : 0,
		    "scriptPubKey" : {
			"asm" : "OP_DUP OP_HASH160 cac1683f15f72fd04ab2225ecbc68c7351140acf OP_EQUALVERIFY OP_CHECKSIG",
			"hex" : "76a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac",
			"reqSigs" : 1,
			"type" : "pubkeyhash",
			"addresses" : [
			    "1KV5BvGiMcRuVmkzSaZRMQCTAB2iUATt7t"
			]
		    }
		},
		{
		    "value" : 0.02450000,
		    "n" : 1,
		    "scriptPubKey" : {
			"asm" : "OP_DUP OP_HASH160 afd8779e66162c542e91e9176bade5ed803668b6 OP_EQUALVERIFY OP_CHECKSIG",
			"hex" : "76a914afd8779e66162c542e91e9176bade5ed803668b688ac",
			"reqSigs" : 1,
			"type" : "pubkeyhash",
			"addresses" : [
			    "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om"
			]
		    }
		}
	    ]
	}


Everything looks correct. This transaction will 'consume' the unspent output from our confirmed transaction and then spends it in the two inputs. One will send 25 millibits to our new address and one will send 24.5 millibits back to us as change. The transaction fee will be credited to the miner that finds the block that our transaction is included in. 

NOTE: The transaction contains an empty scriptSig field because we have not signed it yet. Without a signature, this transaction is meaningless. We have not proven that we OWN the address from which the unspent output is sourced. 

We use signrawtransaction to sign the transaction. It takes the raw hex string as a parameter. If the wallet is encrypted, you must unlock it before you can sign.

	$ bitcoin-cli walletpassphrase foo 3000
	$ bitcoin-cli signrawtransaction 0100000001d22b6563c44e3e42da803d0f03c1c3aabfdb62e6dfd817b55e8148d1ffcd57680000000000ffffffff02a0252600000000001976a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac50622500000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac00000000
	{
	    "hex" : "0100000001d22b6563c44e3e42da803d0f03c1c3aabfdb62e6dfd817b55e8148d1ffcd5768000000006a4730440220365255603f050193357ed1ec635f42bcd27b9b19245fc1ba8c23b3074ca4b81f02205d3ce9f59f558c05154b291264c6c8736aa4e196e8a77538885c79b3d2c2cb38012103b438730c36495422ce46ccb1df25faff7ac6d3c328b2e73b97b20d93435e5952ffffffff02a0252600000000001976a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac50622500000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac00000000",
	    "complete" : true
	}


So we have been given another hex string. We can use decoderawtransaction to see what has changed:

	$ bitcoin-cli decoderawtransaction 0100000001d22b6563c44e3e42da803d0f03c1c3aabfdb62e6dfd817b55e8148d1ffcd5768000000006a4730440220365255603f050193357ed1ec635f42bcd27b9b19245fc1ba8c23b3074ca4b81f02205d3ce9f59f558c05154b291264c6c8736aa4e196e8a77538885c79b3d2c2cb38012103b438730c36495422ce46ccb1df25faff7ac6d3c328b2e73b97b20d93435e5952ffffffff02a0252600000000001976a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac50622500000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac00000000
	{
	    "txid" : "de48066b02069d76a9b579e8da92fbd65c3599d71c88cbd582ad24249b647e3a",
	    "version" : 1,
	    "locktime" : 0,
	    "vin" : [
		{
		    "txid" : "6857cdffd148815eb517d8dfe662dbbfaac3c1030f3d80da423e4ec463652bd2",
		    "vout" : 0,
		    "scriptSig" : {
			"asm" : "30440220365255603f050193357ed1ec635f42bcd27b9b19245fc1ba8c23b3074ca4b81f02205d3ce9f59f558c05154b291264c6c8736aa4e196e8a77538885c79b3d2c2cb3801 03b438730c36495422ce46ccb1df25faff7ac6d3c328b2e73b97b20d93435e5952",
			"hex" : "4730440220365255603f050193357ed1ec635f42bcd27b9b19245fc1ba8c23b3074ca4b81f02205d3ce9f59f558c05154b291264c6c8736aa4e196e8a77538885c79b3d2c2cb38012103b438730c36495422ce46ccb1df25faff7ac6d3c328b2e73b97b20d93435e5952"
		    },
		    "sequence" : 4294967295
		}
	    ],
	    "vout" : [
		{
		    "value" : 0.02500000,
		    "n" : 0,
		    "scriptPubKey" : {
			"asm" : "OP_DUP OP_HASH160 cac1683f15f72fd04ab2225ecbc68c7351140acf OP_EQUALVERIFY OP_CHECKSIG",
			"hex" : "76a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac",
			"reqSigs" : 1,
			"type" : "pubkeyhash",
			"addresses" : [
			    "1KV5BvGiMcRuVmkzSaZRMQCTAB2iUATt7t"
			]
		    }
		},
		{
		    "value" : 0.02450000,
		    "n" : 1,
		    "scriptPubKey" : {
			"asm" : "OP_DUP OP_HASH160 afd8779e66162c542e91e9176bade5ed803668b6 OP_EQUALVERIFY OP_CHECKSIG",
			"hex" : "76a914afd8779e66162c542e91e9176bade5ed803668b688ac",
			"reqSigs" : 1,
			"type" : "pubkeyhash",
			"addresses" : [
			    "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om"
			]
		    }
		}
	    ]
	}



Now our inputs contain a scriptSig , which is the digital signature that proves ownership of the address 1H2n... It also removes the encumbrance on the output so it can be spent. The signature makes this transaction verifiable by any node in the bitcoin network.


Now we can submit the newly signed transaction to the network:

	$ bitcoin-cli sendrawtransaction 0100000001d22b6563c44e3e42da803d0f03c1c3aabfdb62e6dfd817b55e8148d1ffcd5768000000006a4730440220365255603f050193357ed1ec635f42bcd27b9b19245fc1ba8c23b3074ca4b81f02205d3ce9f59f558c05154b291264c6c8736aa4e196e8a77538885c79b3d2c2cb38012103b438730c36495422ce46ccb1df25faff7ac6d3c328b2e73b97b20d93435e5952ffffffff02a0252600000000001976a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac50622500000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac00000000
	de48066b02069d76a9b579e8da92fbd65c3599d71c88cbd582ad24249b647e3a    #THIS IS THE NEW TRANSACTION ID

	#Lets use the txid that was returned to see what our transaction looks like to the network:

	$ bitcoin-cli gettransaction de48066b02069d76a9b579e8da92fbd65c3599d71c88cbd582ad24249b647e3a
	{
	    "amount" : 0.00000000,
	    "fee" : -0.00050000,
	    "confirmations" : 0,
	    "txid" : "de48066b02069d76a9b579e8da92fbd65c3599d71c88cbd582ad24249b647e3a",
	    "walletconflicts" : [
	    ],
	    "time" : 1434312763,
	    "timereceived" : 1434312763,
	    "details" : [
		{
		    "account" : "",
		    "address" : "1KV5BvGiMcRuVmkzSaZRMQCTAB2iUATt7t",
		    "category" : "send",
		    "amount" : -0.02500000,
		    "fee" : -0.00050000
		},
		{
		    "account" : "",
		    "address" : "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om",
		    "category" : "send",
		    "amount" : -0.02450000,
		    "fee" : -0.00050000
		},
		{
		    "account" : "",
		    "address" : "1KV5BvGiMcRuVmkzSaZRMQCTAB2iUATt7t",
		    "category" : "receive",
		    "amount" : 0.02500000
		},
		{
		    "account" : "",
		    "address" : "1H2nZyoHDwgT62CHDWQc6UVRpyiWCgz4om",
		    "category" : "receive",
		    "amount" : 0.02450000
		}
	    ],
	    "hex" : "0100000001d22b6563c44e3e42da803d0f03c1c3aabfdb62e6dfd817b55e8148d1ffcd5768000000006a4730440220365255603f050193357ed1ec635f42bcd27b9b19245fc1ba8c23b3074ca4b81f02205d3ce9f59f558c05154b291264c6c8736aa4e196e8a77538885c79b3d2c2cb38012103b438730c36495422ce46ccb1df25faff7ac6d3c328b2e73b97b20d93435e5952ffffffff02a0252600000000001976a914cac1683f15f72fd04ab2225ecbc68c7351140acf88ac50622500000000001976a914afd8779e66162c542e91e9176bade5ed803668b688ac00000000"
	}


We can examine this in more detail using decoderawtransaction. These should return exactly what we produced before we sent it out to the network.


