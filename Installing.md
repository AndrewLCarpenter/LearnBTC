Installing the Bitcoin client and basic commands

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

    which bitcoind      These commands will show you the location of the client. (Default: /usr/local/bin/)
    which bitcoin-cli


##Starting up
Upon starting your client you will need to set a user name and password. Copy whatever they suggest to ~/.bitcoin/bitcoin.conf

    bitcoind    (starts your client)

    #Now you should receive the message about rpc username and password
    echo 'copied text' > ~/.bitcoin/bitcoin.conf

Now your client is ready to work

        bitcoind -daemon        #starts the client in the background

Use bitcoin-cli (bitcoin command line interface) in order to interact with the server

        bitcoin-cli help        #displays a help page with list of available commands (there are a lot)
        bitcoin-cli getinfo     #Use this to check on your clients progress in downloading the blockchain


