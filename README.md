# Run Pi-Hole + Unbound Using A MacVLAN And Custom Bridge Network On A Synology NAS With Docker-Compose

## Description

Using docker-compose, with the included docker-compose.yaml file, you can run Pi-Hole + Unbound, each in its own container, on a Synology NAS.  The docker-compose.yaml file will also create a MacVLAN and a custom bridge network for the containers.  The MacVLAN network will be a /30 subnet, allowing for two assignable IP addresses from your Local LAN that will be assigned to the individual containers.  The custom bridge network will be a /32 subnet, allowing for 1 assignable IP address that will be assigned to the Pi-Hole container.

Much of the setup for this project is influenced by my [Pi-Hole Docker Synology NAS Setup Guide](https://youtu.be/1yG0p9gU104) and [Unbound Pi-Hole Synology Setup](https://youtu.be/-546g1w_L3w) videos, which I encourage you to view for details on how things are is setup. As an overview, the videos describe how to setup Pi-Hole and Unbound through the command line and from the Docker package from within DSM. This generally works great except, on reboot, Unbound may start up prior to Pi-Hole which causes the MacVLAN IP assignment to switch between the containers resulting in a problem with the setup.  The work around is to manually stop both containers, then restart the containers in the proper order or use a custom startup script to stop the containers and start them up again in the proper order (see the pinned comment from the second video listed above).  The setup configured in this project resolves this issue.

## Directions

You'll need to make sure the prerequisites are fulfilled on your Synology NAS first, then run through the setup steps below. I also run through setting everything up in this video here -> VIDEO NOT DONE YET.

### Prerequistes
On your Synology NAS you'll need to do the following:
1. Install the Docker package.
2. Install the Git Server package.
3. Enable SSH.

### Setup Steps
1. SSH into your Synology NAS.
```
ssh <admin account>@<IP address of Synology NAS>
```
2. Change directory into /volume1/docker. 
```
cd /volume1/docker
```
4. Clone this repository.
```
sudo git clone https://github.com/digtalaloha/synology-docker-pihole-unbound.git
```
5. Change directory into the newly created synology-docker-pihole-unbound directory.
```
cd synology-docker-pihole-unbound
```
6. Create host volume mount points that will be used by the Pi-Hole container.
```
mkdir -p pihole/pihole; mkdir -p pihole/dnsmasq.d
```
7. Edit the .env file with the specifics from your environment (I left the settings I used as placeholders in the actual .env file).  These entries will be used to auto populate the docker-compose.yaml file.
```
PIHOLE_MACVLAN_IP=<Enter the first assignable IP address from the /30 MacVLAN subnet you created>
PIHOLE_BRIDGE_IP=<Enter the one assignable IP address from the /32 custom bridge subnet you created>
UNBOUND_MACVLAN_IP=<Enter the second assignable IP address from the /30 MacVLAN subnet you created>
WEBPASSWORD='<Enter the Pi-Hole GUI password you would like to use (keep the single quotes)>'
NETWORK_INTERFACE=<Enter the network interface your Synology NAS uses to access your LAN>
MACVLAN_SUBNET=<Enter the LAN network>/24
MACVLAN_GATEWAY=<Enter the LAN Gateway>
MACVLAN_IP_RANGE=<Enter the starting IP address of the LAN subnet that the MacVLAN network will use>/30
BRIDGE_SUBNET=<Enter the custom bridge network>/24
BRIDGE_GATEWAY=<Enter the custom bridge network gateway>
BRIDGE_IP_RANGE=<Enter the IP address of the custom bridge network>/32
TIMEZONE=<Enter the timezone you are in>
```
9. Create and start the Pi-Hole and Unbound containers along with setup the MacVLAN and custom bridge network using docker-compose.
```
sudo docker-compose up -d
```
### Start Using Pi-Hole
At this point if everything was created and started up properly you should be able to...
* Access your Pi-Hole container from it's Web GUI using the MacVLAN IP address assigned to it.
* Login to the Pi-Hole admin interface using the password you entered in the .env file.
* Assign the Pi-Hole MacVLAN IP address as the DNS server for your clients and/or on your DHCP server.
* Assign the Pi-Hole custom bridge network on your Synology NAS. 

## References
* https://github.com/chriscrowe/docker-pihole-unbound/
* https://github.com/pi-hole/docker-pi-hole#quick-start
