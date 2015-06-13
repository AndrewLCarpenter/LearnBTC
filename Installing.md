#Installing the Bitcoin client and basic commands

    git clone github..... (you know where that is)
    git tag

Now choose your version of the client. (Note: the versions with suffix rc are RELEASE CANDIDATES)

In order to pick your favorite version:

    git checkout v0.9......

Now build the client:

    ./autogen.sh
    ./configure  //(There are a ton of options here. Maybe figure out what they mean?)
    //(Use ./configure --with-incompatible-bdb if you get the Berkley error)

    make
    sudo make install
    
    which bitcoind
    which bitcoin-cli


##Starting up
Upon starting your client you will need to set a user name and password. Copy whatever they suggest to ~/.bitcoin/bitcoin.conf

    echo 'copied text' > ~/.bitcoin/bitcoin.conf


