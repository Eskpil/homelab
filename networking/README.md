# Homelab networking

My homelab networking is fairly simple, there is no PfSense or
OpenSense. Or no fancy Ubiquty gear, the router is the one shipped with
the apartment and the switches are cheap junk found on Ebay. It is all
for the most part in software.

## Software

The networking on both my hosts and virtual machines is heavily based on
what systemd has to offer. The two biggest dependencies are:

1. systemd-networkd
2. systemd-resolved

# mDNS

To avoid having to remember ip addresses and allow very short DHCP
leases i mDNS which allows the machines to be found at **"hostname".local** on
the network thanks to systemd-resolved.


# IPV4 over IPV6

I decided to use traditional ipv4 over ipv6 since ipv4 provides me with
more than enough ip addresses. 
