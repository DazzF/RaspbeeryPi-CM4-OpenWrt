# RaspbeeryPi-CM4-OpenWrt
Setup and config files for my Raspberry PI CM4 OpenWrt Router

I've built and I'm currently testing a router made up of a Raspberry Pi Compute Module 4 IO Board, a Ziyituod Gigabit Dual Port Network Ethernet Card PCI Express (Realtek 8111G Chipset) and a Raspberry Pi Compute Module 4 (Wireless / 4GB RAM / Lite), but I suspect a 1GB version will work perfectly (I'll buy and try one in due cource).

At this point my configuration is quite simple. I want a WAN connection for internet, a LAN connection for my wired network at home and a DMZ for a number of servers I run hosting websites etc... (Raspberry PI4's)

The initial set up for me was a little more difficult as I had my virgin router running on 192.168.1.1 (the defult OpenWrt IP address), so I had to set up a laptop on a desktop switch with the CM4 Router and change the address. 

At this point in time the Raspberry Pi 4/CM4 is only supported in development snapshot versions, but it appears stable and should be formally supported soon.

Build Steps

1. Download the OpenWrt snapshot from the site and burn it to an SD card (remember I'm using a lite version with no onboard storage). I used the Raspberry Pi Imager to write the SD card.
2. I booted the CM4 and connected via SSH to 192.168.1.1 (ssh root@192.168.1.1) and changed the root password
3. I change the IP address and DNS server details as follows
      uci set dhcp.lan.ra_management='1'
      uci add_list dhcp.@dnsmasq[-1].server=8.8.8.8
      uci add_list dhcp.@dnsmasq[-1].server=8.8.4.4
      uci commit dhcp
      uci set network.lan.ipaddr='192.168.1.10'
      uci set network.lan.gateway='192.168.1.1'
      uci commit network
      reboot
4. I then updata ther package manager
      opkg update
5. I then install the Web UI 
      opkg install luci
      opkg install luci-ssl
      /etc/init.d/uhttpd restart
6. I then installed the drivers for the Realtek 8111G Chipset, took a quick search to discover which package to install
      opkg install kmod-r8169

I was now in a position to configure the router and my initial set up can be found in /etc/config all the files are there. 
