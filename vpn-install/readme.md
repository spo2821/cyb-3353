"sudo apt install apt-transport-https ca-certificates curl software-properties-common -y"

"curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -"

"sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable" " 
   
"apt-cache policy docker-ce"

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


![download](https://user-images.githubusercontent.com/19178865/144699324-e97ecbb1-12b4-47e7-b2bd-6a5a868f159d.png)

