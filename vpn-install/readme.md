<h1>Digital Ocean Set Up</h1>

To start this lab, we had to go to the site Digital Ocean to set up an account that we can then access $100 in credits to utilize for this lab. We start by using the referral code from this link : https://m.do.co/c/4d7f4ff9cfe4 which grants us the $100 credit.

From here, we have to go in, and create a new ubuntu droplet for $5/month to set up the vpn server on. Next we will install docker and the vpn software.

<h1> Docker Installation </h1>

Continuing, we have to first install the certificates and dependencies required to install docker, that is done with the following command.
"sudo apt install apt-transport-https ca-certificates curl software-properties-common -y"

Next we have to run this command to pull the docker ubuntu install from docker. 
"curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -"

Next, we have to add a repository for docker, which requires running the following command.
"sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable" " 
   
After the repository is installed, we have to switch into the repo so that we can continue the installation.
"apt-cache policy docker-ce"

Next, we have to actually install docker through apt by running the following command. 
"sudo apt install docker-ce -y"

"sudo usermod -aG docker ${USER}"

"sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose"

"sudo chmod +x /usr/local/bin/docker-compose"

"mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml " 
"version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - SERVERURL=143.198.183.61
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1"
      
"cd ~/wireguard/
docker-compose up -d"

"docker-compose logs -f wireguard"

"[Interface]
Address = 10.0.0.3
PrivateKey = yI/XtR0EO8xyjWTN5y2lVbt86EfrjQXhx96O8jjd7XU=
ListenPort = 51820
DNS = 10.0.0.1

[Peer]
PublicKey = MR6Jdp2Ddx290m405v/nUIZJBPlX5r64wNLayWmTSDE=
Endpoint = 143.198.183.61:51820
AllowedIPs = 0.0.0.0/0, ::/0"



<img width="1440" alt="Screen Shot 2021-12-03 at 11 33 55 PM" src="https://user-images.githubusercontent.com/19178865/144699354-269fa696-c183-44fc-838e-536384e4aa89.png">

