# klever-blockchain-validators-program
Websites:

https://klever.finance/klever-blockchain-validators-program/

https://docs.klever.finance/

https://twitter.com/klever_io

https://discord.gg/klever-io


## **1. Introduction**
###### 1. What is Klever
Klever Finance is a trusted permissioned blockchain network for the emerging decentralized economy, providing a safer, faster and smarter cryptocurrency experience for all users globally to enter and thrive.
A blockchain network does not have an intrinsic value in and of itself, but in fact is only as valuable as what the platform offers its users in terms of usable products and opportunities to participate in the network. 
With this in mind, we present KleverChain to allow users on a global scale to access indispensable services and products. These include market-leading crypto wallet, crypto swap, web browser, exchange, staking, payment channels, loans, liquidity pools, decentralized finance, game assets, supply chains, logistic records, notary, stored value, gift cards, rewards, loyalty programs, collectibles, banking, ownership and more. 
We strive to create a network where trustless technologies empower businesses and individuals in an open global economy using secure, fast and innovative on-chain peer-to-peer applications. 
###### 2. Architecture Overview
Building and deploying blockchain apps should be simple, cheap, and easy for all developers to do, and that is exactly what Klever Blockchain enables. 
Instead of being a smart-contracts platform, Klever Blockchain will provide prebuilt and ready-to-use functionalities for developers to build decentralized applications.
Our goal is to have a secure and efficient blockchain coordinated by a Proof of Stake consensus mechanism composed of 21 validators, which are randomly selected among the masternodes for the consensus group in each Epoch.
Nodes will sign-off messages broadcasted to the network building a reliability rank of each operator. Nodes with bad behavior will be jailed. Until this jail time isn’t cleared, those ill-behaved nodes will not participate in the validators selection pool.
On top of that, a customized version of the Practical Byzantine Fault Tolerance algorithm (PBFT) was implemented for network reliability and resilience, ensuring a fast full confirmation of blocks.

## **2.System requirements**
•  A CPU with 4 cores.

•  A RAM with 8GB.

•  An SSD with 200 GB.

•  An internet connection speed of at least 100 Mbps.

•  Linux or MacOS. 

## **3.Setup node:**

#Install Docker and download the latest Klever Toolchain image.
```
docker pull kleverapp/klever-go-testnet:latest
```
#Create wallet
```
mkdir wallet

docker run -it --rm --user "$(id -u):$(id -g)" \
 -v $(pwd)/wallet:/opt/klever-blockchain \
 --entrypoint=/usr/local/bin/operator kleverapp/klever-go-testnet:latest "create-wallet"
```
**#Allways BACKUP the walletKey.pem,you will need the file all the time!!!!

#check your wallet certificate and address
```
docker run -it --rm --user "$(id -u):$(id -g)" \
 -v $(pwd)/wallet:/opt/klever-blockchain \
 --entrypoint=/usr/local/bin/operator kleverapp/klever-go-testnet:latest "getAddress"
```
#Create data folder
```
mkdir -p $(pwd)/node/config $(pwd)/node/db $(pwd)/node/logs
#Download config file:
curl -k https://backup.testnet.klever.finance/config.testnet.100008.tar.gz \
    | tar -xz -C ./node
```
#Create node key
```
docker run -it --rm -v $(pwd)/node/config:/opt/klever-blockchain \
    --user "$(id -u):$(id -g)" \
    --entrypoint='' kleverapp/klever-go-testnet:latest keygenerator
```
#Download backup DB file
```
curl -k https://backup.testnet.klever.finance/kleverchain.latest.tar.gz \
    | tar -xz -C ./node
```
#Run node with UI (use screen here):
for the first time,create a screen:

```
screen -S klever
```
or later,just restore the Klever screen :
```
screen -x klever
```
```
docker run -it --rm \
    --user "$(id -u):$(id -g)" \
    --name klever-node \
    -v $(pwd)/node/config:/opt/klever-blockchain/config/node \
    -v $(pwd)/node/db:/opt/klever-blockchain/db \
    -v $(pwd)/node/logs:/opt/klever-blockchain/logs \
    --network=host \
    --entrypoint=/usr/local/bin/validator \
    kleverapp/klever-go-testnet:latest \
    '--log-save' '--rest-api-interface=0.0.0.0:8080'
```
#Or run node in backgroud
```
docker run -it -d \
    --user "$(id -u):$(id -g)" \
    --name klever-node \
    -v $(pwd)/node/config:/opt/klever-blockchain/config/node \
    -v $(pwd)/node/db:/opt/klever-blockchain/db \
    -v $(pwd)/node/logs:/opt/klever-blockchain/logs \
    --network=host \
    --entrypoint=/usr/local/bin/validator \
    kleverapp/klever-go-testnet:latest \
    '--log-save' '--use-log-view' '--rest-api-interface=0.0.0.0:8080'
```
**#Fill out google form the apply Validtor:
>https://forms.gle/ywgv5uVJmcLuMABP7

**#After a few hours check here with your wallet address
>https://testnet.kleverscan.org/

#Balence is 1.5m klv, then go on :

#Create validator:
```
docker run -it --rm --user "$(id -u):$(id -g)" \
   -v $(pwd)/wallet:/opt/klever-blockchain \
   --network=host \
   --entrypoint=/usr/local/bin/operator \
   kleverapp/klever-go-testnet:latest \
   --key-file=./walletKey.pem \
   create-validator \Your_ValidatorKey  10 10000000 LOGO  Your_Wallet_key Your_Wallet_key Your_Validtor_Name
```
#Freeze KLV token
```
docker run -it --rm --user "$(id -u):$(id -g)" \
   -v $(pwd)/wallet:/opt/klever-blockchain \
   --network=host \
   --entrypoint=/usr/local/bin/operator \
   kleverapp/klever-go-testnet:latest \
   --key-file=./walletKey.pem freeze 1500000
```
#copy the txHash
```
docker run -it --rm --user "$(id -u):$(id -g)" \
    -v $(pwd)/wallet:/opt/klever-blockchain \
    --network=host \
    --entrypoint=/usr/local/bin/operator \
    kleverapp/klever-go-testnet:latest \
    --key-file=./walletKey.pem \
    tx-by-id \
    Your_copied_txHash
```
#And copy the bucket id：
#Delegate the fronze token to your node
```
docker run -it --rm --user "$(id -u):$(id -g)" \
    -v $(pwd)/wallet:/opt/klever-blockchain \
    --network=host \
    --entrypoint=/usr/local/bin/operator \
    kleverapp/klever-go-testnet:latest \
    --key-file=./walletKey.pem \
    delegate \
    Your_wallet_address  \
    Your_bucket_id
```
## **4.Monitor you node:**

#open 8080,3000,9090,9100 ports:
```
ufw allow 8080/tcp
ufw allow 3000/tcp
ufw allow 9090/tcp
ufw allow 9100/tcp
```
#check statistics and metrics:
```
http://yournodeip:8080/node/statistics

http://yournodip:8080/node/metrics
```
#monitor node with Grafana+Prometheus+node_exporter

#Setup grafana
```
apt update && apt upgrade –y
apt-get install -y apt-transport-https
apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | tee -a /etc/apt/sources.list.d/grafana.list deb https://packages.grafana.com/oss/deb stable main
apt update
apt install grafana –y
systemctl enable grafana-server
systemctl start grafana-server
```
#Setup prometheus：
```
wget https://github.com/prometheus/prometheus/releases/download/v2.34.0-rc.0/prometheus-2.34.0-rc.0.linux-amd64.tar.gz
tar xvfz prometheus-2.34.0-rc.0.linux-amd64.tar.gz
cd prometheus-2.34.0-rc.0.linux-amd64
```
#creat service
```
printf "[Unit]
Description=Prometheus
After=network.target

[Service]
User=root
WorkingDirectory=$HOME/prometheus-2.34.0-rc.0.linux-amd64
ExecStart=$HOME/prometheus-2.34.0-rc.0.linux-amd64/prometheus --config.file=prometheus.yml
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/prometheus.service

sudo systemctl enable prometheus
sudo systemctl daemon-reload
sudo systemctl restart prometheus
```

#setup node_exporter
#adduser
useradd -rs /bin/false node_exporter
#download node_exporter and unzip
```
wget https://github.com/prometheus/node_exporter/releases/download/v1.1.1/node_exporter-1.1.1.linux-amd64.tar.gz

tar xvfz node_exporter-1.1.1.linux-amd64.tar.gz

cd node_exporter-1.1.1.linux-amd64
```
#Create service
```
printf "[Unit]
Description=Node Exporter
After=network.target

[Service]
User=root
WorkingDirectory=$HOME/node_exporter-1.1.1.linux-amd64
ExecStart=$HOME/node_exporter-1.1.1.linux-amd64/node_exporter
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/node_exporter.service

sudo systemctl enable node_exporter
sudo systemctl daemon-reload
sudo systemctl restart node_exporter
sudo systemctl status node_exporter
```
#modify prometheus.yml
```
cd
cd prometheus-2.34.0-rc.0.linux-amd64
mv prometheus.yml prometheus_old.yml
wget https://github.com/gmkevinren/klever/blob/main/prometheus.yml
```

#Restart all three service
```
sudo systemctl restart grafana-server.service
sudo systemctl restart prometheus.service
sudo systemctl restart node_exporter.service
```
#open up your node’s grafana: http://Your_node_ip:3000
#user name and password :admin
#Setup data source
click left side "Configuration" icon and "data sources" and prometheus,then input URL :"http://localhost:9090" and scroll down click "Save & Test" button

#Download [klevermonitor.json](https://github.com/gmkevinren/klever/blob/main/klevermonitor.json) or [klevermonitor2.json](https://github.com/gmkevinren/klever/blob/main/klevermonitor2.json)

#click “+” icon and “import” ,”Upload JSON file” and choose the json file you just download
 
** # finish monitor **
###### 5. Fill the phase one feedback form  **
>https://forms.gle/VgsQX2ba8iMvXUK77
 


