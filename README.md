#h1 Mac-Network-Commands-Cheat-Sheet
A handy repository for anyone who wants to fix a network issue :)

I have only orgonised and added my own command to the list but the original document was drafted by [krypted.com](http://krypted.com/mac-security/mac-network-commands-cheat-sheet/)

Feel free to contribute and improve this list :)

#H1 Get an ip address for en0:
```
ipconfig getifaddr en0
```
#H1 Same thing, but setting and echoing a variable:
```
ip=`ipconfig getifaddr en0 ; echo $ip
```
#H1 View the subnet mask of en0:
```
ipconfig getoption en0 subnet_mask
```
#H1 View the dns server for en0:
```
ipconfig getoption en0 domain_name_server  
```
#h1 Get information about how en0 got its dhcp on:
---
ipconfig getpacket en1
---
#h1View some network info:

ifconfig en0
#h1Set en0 to have an ip address of 10.10.10.10 and a subnet mask of 255.255.255.0:

ifconfig en0 inet 10.10.10.10 netmask 255.255.255.0

#h1Show a list of locations on the computer:
networksetup -listlocations

#h1Obtain the active location the system is using:
networksetup -getcurrentlocation

#h1Create a network location called Work and populate it with information from the active network connection:
networksetup -createlocation

#h1Work populate Delete a network location called Work:
networksetup -deletelocation

#h1Work Switch the active location to a location called Work:
networksetup -switchlocation

#h1 Work Switch the active location to a location called Work, but also show the GUID of that location so we can make scripties with it laters:
scselect

#h1 Work List all of the network interfaces on the system:

networksetup -listallnetworkservices

#h1 Rename the network service called Ethernet to the word Wired:
networksetup -renamenetworkservice

#h1 Ethernet Wired Disable a network interface:
networksetup -setnetworkserviceenabled

#h1 off Change the order of your network services:
networksetup -ordernetworkservices “Wi-Fi” “USB Ethernet”

#h1S et the interface called Wi-Fi to obtain it if it isn’t already
networksetup -setdhcp Wi-Fi

#h1 Renew dhcp leases:
ipconfig set en1 BOOTP && ipconfig set en1 DHCP ifconfig en1 down && ifconfig en1 up

#h1 Renew a dhcp lease in a script:

echo "add State:/Network/Interface/en0/RefreshConfiguration temporary" | sudo scutil

#h1 Configure a manual static ip address:

networksetup -setmanual Wi-Fi 10.0.0.2 255.255.255.0 10.0.0.1

#h1 Configure the dns servers for a given network interface:
networksetup -setdnsservers Wi-Fi 10.0.0.2 10.0.0.3

#h1 Obtain the dns servers used on the Wi-Fi interface:
networksetup -getdnsservers Wi-Fi

#h1 Stop the application layer firewall:
```
launchctl unload /System/Library/LaunchAgents/com.apple.alf.useragent.plist launchctl unload /System/Library/LaunchDaemons/com.apple.alf.agent.plist Start the application layer firewall: launchctl load /System/Library/LaunchDaemons/com.apple.alf.agent.plist launchctl load /System/Library/LaunchAgents/com.apple.alf.useragent.plist
```
#h1 Allow an app to communicate outside the system through the application layer firewall:

socketfilterfw -t “/Applications/FileMaker Pro/FileMaker Pro.app/Contents/MacOS/FileMaker Pro” See the routing table of a Mac: netstat -nr Add a route so that traffic for 10.0.0.0/32 communicates over the 10.0.9.2 network interface: route -n add 10.0.0.0/32 10.0.9.2 Log bonjour traffic at the packet level: sudo killall -USR2 mDNSResponder Stop Bonjour: launchctl unload -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist  Start Bojour: launchctl load -w /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist Put a delay in your pings: ping -i 5 192.168.210.1 Ping the hostname 5 times and then stop the ping: ping -c 5 google.com Flood ping the host: ping -f localhost Set the packet size during your ping: ping -s 100 google.com Customize the source IP during your ping: ping -S 10.10.10.11 google.com View disk performance: iostat -d disk0 Get information about the airport connection on your system: /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport -I Scan the available Wireless networks: /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport -s Trace the path packets go through: traceroute google.com Trace the routes without looking up names: traceroute -n google.com Trace a route in debug mode: traceroute -d google.com View information on all sockets: netstat -at View network information for ipv6: netstat -lt View per protocol network statistics: netstat -s View the statistics for a specific network protocol: netstat -p igmp Show statistics for network interfaces: netstat -i View network information as it happens (requires ntop to be installed): ntop Scan port 80 of www.google.com /System/Library/CoreServices/Applications/Network\ Utility.app/Contents/Resources/stroke www.google.com 80 80 Port scan krypted.com stealthily: nmap -sS -O krypted.com/24 Establish a network connection with www.apple.com: nc -v www.apple.com 80 Establish a network connection with gateway.push.apple.com over port 2195 /usr/bin/nc -v -w 15 gateway.push.apple.com 2195 Establish a network connection with feedback.push.apple.com only allowing ipv4 /usr/bin/nc -v -4 feedback.push.apple.com 2196 Setup a network listener on port 2196 for testing: /usr/bin/nc -l 2196 Capture some packets: tcpdump -nS Capture all the packets: tcpdump -nnvvXS Capture the packets for a given port: tcpdump -nnvvXs 548 Capture all the packets for a given port going to a given destination of 10.0.0.48: tcpdump -nnvvXs 548 dst 10.0.0.48 Capture the packets as above but dump to a pcap file: tcpdump -nnvvXs 548 dst 10.0.0.48 -w /tmp/myfile.pcap Read tcpdump (cap) files and try to make them human readable: tcpdump -qns 0 -A -r /var/tmp/capture.pcap What binaries have what ports and in what states are those ports: lsof -n -i4TCP Make an alias for looking at what has a listener open, called ports: alias ports='lsof -n -i4TCP | grep LISTEN' Report back the name of the system: hostname Flush the dns cache: dscacheutil -flushcache Clear your arp cache: arp -ad View how the Server app interprets your network settings: serveradmin settings network Whitelist the ip address 10.10.10.2: /Applications/Server.app/Contents/ServerRoot/usr/libexec/afctl -w 10.10.10.2 Finally, the script network_info.sh shows information about a Macs network configuration. Both active and inactive network interfaces are listed, in the order that they are used by the OS and with a lot of details (MAC-address, interface name, router, subnet mask etc.).
