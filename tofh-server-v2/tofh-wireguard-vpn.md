# TOFH Wireguard VPN

Building and installing from source

Ref:

```text
sudo su

apt install raspberrypi-kernel-headers libelf-dev libmnl-dev build-essential git

git clone https://git.zx2c4.com/wireguard-linux-compat

git clone https://git.zx2c4.com/wireguard-tools
```

#### Compile and install the module

```text
sudo make -C wireguard-linux-compat/src -j$(nproc)

sudo make -C wireguard-linux-compat/src install
```

#### Compile and install the wg\(8\) tool

```text
sudo make -C wireguard-tools/src -j$(nproc)

sudo make -C wireguard-tools/src install
```

#### Enable IP forwarding

```text
sudo perl -pi -e 's/#{1,}?net.ipv4.ip_forward ?= ?(0|1)/net.ipv4.ip_forward = 1/g' /etc/sysctl.conf

sudo reboot
```

#### Create  Key Pairs

```text
sudo su

cd /etc/wireguard

umask 077


wg genkey | tee server_privatekey | wg pubkey > server_publickey

wg genkey | tee peer1_privatekey | wg pubkey > peer1_publickey

ls  
```

#### Create a a wg0.conf file in ‘/etc/wireguard/’

```text
[Interface]
Address = 10.100.0.1/24
ListenPort = 65111
DNS = 1.1.1.1
PrivateKey = <server private>

PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]
#Peer-1 iPhone XR
PublicKey = <peer1 public key>
AllowedIPs = 10.100.0.2/32
#PersistentkeepAlive = 60

[Peer]
#Peer-2
PublicKey = <peer2 public key>
AllowedIPs = 10.100.0.3/32
#PersistentkeepAlive = 60

[Peer]
#Peer-3
PublicKey = <peer3 public key> 
AllowedIPs = 10.100.0.4/32
#PersistentkeepAlive = 60

```

#### Create client config file for peers \(non-spilit and then split\)

```text
[Interface]
Address = 10.100.0.2/32
DNS = 1.1.1.1
PrivateKey = <private key for peer>
[Peer]
PublicKey = < server public key>
Endpoint = wg.meridian-research.com:65111
AllowedIPs = 0.0.0.0/0
#PersistentkeepAlive = 60

```

```text
[Interface]
Address = 10.100.0.2/32
DNS = 1.1.1.1
PrivateKey = <private key for client>

[Peer]
PublicKey = < server public key >
Endpoint = wg.meridian-research.com:65111
AllowedIPs = 192.168.77.0/24, 192.168.69.0/24
#PersistentkeepAlive = 60

```

#### Install qrencode and create QR code for client config

```text
sudo apt install qrencode

sudo qrencode -t ansiutf8 < /etc/wireguard/peer1.conf
```



#### Configure Wireguard Server to start on boot

```text
sudo systemctl enable wg-quick@wg0

sudo chown -R root:root /etc/wireguard/

sudo chmod -R og-rwx /etc/wireguard/*
```



