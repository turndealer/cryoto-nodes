#Gathering Information
```
NODEIP:IP ADDRESS of your Bitcoin NODE 
NODEUSER:Username of your Bitcoin NODE  usually root
NODEPWD:Password of your Bitcoin NODE for ssh
ACCESS_IP: IP address which will access your Bitcoin node 
```
#Choosing Information for RPC
```
RPCUser: Choose RPC Username [Alpha Numeric] atleast 16 chars
RPCPass: Choose RPC Password[Alpha Numeric] Keep strong 32 chars
RPCPort:Choose uncommon port from 1000 to 65000 example 5351
```

Now login to your NODEIP with ssh root@NODEIP 

```
cd /opt/
wget https://bitcoin.org/bin/bitcoin-core-22.0/bitcoin-22.0-x86_64-linux-gnu.tar.gz
tar -xvf bitcoin-22.0-x86_64-linux-gnu.tar.gz
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-22.0/bin/*
```

Run bitcoind and stop with ctrl+c
```
cd /root/.bitcoin/
sudo nano bitcoin.conf

rpcuser=RPCUser
rpcpassword=RPCPass
rpcport=RPCPort
rpctimeout=5
rpcallowip=ACCESS_IP
rpcbind=0.0.0.0
testnet=0
server=1
prune=550
addresstype=p2sh-segwit
#daemon=1
```
This now becomes your command 
bitcoind -server -rpcbind=0.0.0.0 -rpcport=RPCPort -rpcallowip=ACCESS_IP -rpcuser=RPCUser -rpcpassword=RPCPass -prune=550 -addresstype=p2sh-segwit

or You can supervisor to run it 
```
sudo apt-get install supervisor
cd /etc/supervisor/conf.d/
sudo nano bitcoin.conf
```
Next enter following information
```
[program:bitcoin]
command=/usr/local/bin/bitcoind -datadir=/root/.bitcoin -conf=/root/.bitcoin/bitcoin.conf
autostart=true
autorestart=true
stderr_logfile=/var/log/supervisor/bitcoin.err.log
stdout_logfile=/var/log/supervisor/bitcoin.out.log
```
Now Save it and run following command
```
supervisorctl reload  
```
To check Log use command
```
supervisorctl
tail -f bitcoin
```

#Testing Connection
```
curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"getblockchaininfo","params":[]}' -H 'content-type:text/plain;' http://NODEUSER:NODEPWD@127.0.0.1:8332/
```
If above response comes empty then you can use verbose to debug

```
curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"getblockchaininfo","params":[]}' -H 'content-type:text/plain;' http://NODEUSER:NODEPWD@127.0.0.1:8332/ -v
```

#creating a wallet
```
 bitcoin-cli createwallet wallet
```


