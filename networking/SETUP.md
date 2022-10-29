# Setup

Since systemd-networkd applies network configuration based on lexical
order of file names any file prefixed with 1 would be applied before a
file prefixed with 2.

## NetworkManager

Many of the installations I have done have come with NetworkManager by
default, systemd-(networkd/resolved) does not work well with
NetworkManager therefore I made the decision to not use NetworkManager
at all.

### Disable NetworkManager

```shell
sudo systemctl stop NetworkManager.service
sudo systemctl disable NetworkManager.service
```

Optionally you could also remove NetworkManager from the system but that
is not required.

## Host

All my hosts run bridged networking

### Bind interface to a network

```ini
# file is /etc/systemd/network/1-br0-bind.network

[Match]
Name=enp3s0

[Network]
Bridge=br0
```

### Assign a ipv4 ip address to the network over DHCP

```ini
# file is /etc/systemd/network/2-br0.dhcp.network

[Match]
Name=br0

[Network]
DHCP=ipv4
MulticastDNS=yes

[DHCPv4]
ClientIdentifier=mac
```

### Create the bridge network device

```ini
# file is /etc/systemd/network/br.netdev

[NetDev]
Name=br0
Kind=bridge
```

## Virtual Machine

All the virtual machines should be running the same networking
configurations

### Select an interface and assign ip address over DHCP

```ini
# file is /etc/systemd/network/eth0.network

[Match]
Name=enp1s0

[Network]
DHCP=ipv4
MulticastDNS=yes

[DHCPv4]
ClientIdentifier=mac
```

## Shared

Now that should be all the specific configuration. The rest should be
shared. At this point we are ready to start systemd-networkd

### Start systemd-networkd

```shell
sudo systemctl start systemd-networkd.service
sudo systemctl enable systemd-networkd.service
```

### Configure systemd-resolved

Configure systemd-resolved to use mDNS, cache and Google 8.8.8.8 DNS
server:

```ini
# file is /etc/systemd/resolved.conf

[Resolve]
DNS=8.8.8.8
MulticastDNS=Yes
LLMNR=no
Cache=yes
```

### Start systemd-resolved

At this point we are required to start systemd-resolved before we
proceed

```shell
sudo systemctl start systemd-resolved.service
sudo systemctl enable systemd-resolved.service
```

### Configure "/etc/nsswitch.conf" and "/etc/resolv.conf"

At this point we have done most of the heavy work. Now we just have to
tell our system to use systemd-resolved when resolving DNS queries. That
is as easy as symlinking a file

```shell
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

Too configure nsswitch to use mDNS we have to make sure that the hosts
option in /etc/nsswitch.conf is set too:

```
hosts: files mdns4_minimal [NOTFOUND=return] dns myhostname
```
