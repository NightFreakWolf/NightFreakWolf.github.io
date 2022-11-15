### Downloading Docker Files
- First I used password to access my machine so go to the 3 dots on the right and click the access console command to gain access to typing into your machine then enter the commands as follows.
- https://thematrix.dev/install-docker-and-docker-compose-on-ubuntu-20-04/
- Install anything necessary for docker to install. Then type in the following commands to get docker installed onto your virtual machine.
```sudo apt install apt-transport-https ca-certificates curl software-properties-common -y```
- ```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```
- ```
  sudo add-apt-repository \ 
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable
  ```
- Use this command to switch you to the correct repo.
- ```apt-cache policy docker-ce```
- The command to install the docker-compose part.
- ```sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose```
- ```sudo chmod +x /usr/local/bin/docker-compose```
- This will allow you to use docker-compose without saying access denied.
- After running all these commands docker-compose should work on your droplet machine and now we can start installing WireGuard.
### Downloading WireGuard
- https://thematrix.dev/setup-wireguard-vpn-server-with-docker/
- So now we need to set up the folders for the wireguard docker container.
- ```mkdir -p ~/wiregaurd/```
- ```mkdir -p ~/wireguard/config/```
- ```nano ~/wireguard/docker-compose.yml```
- After you nano the file paste the following into the file.
```
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
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
      - net.ipv4.conf.all.src_valid_mark=1
```
- After running all these commands docker-compose should work on your droplet machine.
```
cd ~/wiregaurd/
  docker-compose up -d
```
- this brings your WireGaurd container up and now your VPN should be running.
- ```docker-compose logs -f wiregaurd```
- Install the WireGuard app on your phone and scan the QR code and now you are connected with a VPN :)
- To connect on your PC navigate to the peer_pc1.conf file and copy the text in the file and paste it into the "Add empty tunnel option"
