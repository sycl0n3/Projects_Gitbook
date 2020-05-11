# WiFi Beacon Sniffer

## Project Aim

Build a Wifi Sniffer for identifying Wifi clients from beacon broadcasts

Youtube video showing scapy principles in Python:[ https://www.youtube.com/watch?v=Z1MbpIkzQjU](https://www.youtube.com/watch?v=Z1MbpIkzQjU)

#### Getting the WLAN interface into monitor mode:

```text
sudo airmon-ng check kill
sudo airmon-ng start wlan1
```

#### Checking WLAN1 is in monitor mode:

```text
sudo ifconfig
```

Should give an output similar to:

```text
wlan1mon: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 150
unspec 00-C0-CA-5A-64-F1-00-00-00-00-00-00-00-00-00-00  txqueuelen 1000  (UNSPEC)
RX packets 7612  bytes 876259 (855.7 KiB)
RX errors 0  dropped 0  overruns 0  frame 0
TX packets 0  bytes 0 (0.0 B)        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

#### Creating the python environment

Installing pipenv \(for python 3\)

```text
sudo pip3 install pipenv
mkdir <project name>
cd <project> name
pipenv install
```

#### Working base code \(Python 3\)

```text
#!/usr/bin/env python

from scapy.all import *
import datetime
conf.iface = "wlan1mon"

def handle_pkt(pkt):
        if Dot11 in pkt and pkt[Dot11].type == 0 and pkt[Dot11].subtype ==4:
                timestmp = datetime.datetime.now()
                hwaddr = pkt[Dot11].addr2
                ssid = pkt[Dot11Elt][0].info
                print (timestmp, hwaddr, ssid.decode())

sniff(prn=handle_pkt)

```

