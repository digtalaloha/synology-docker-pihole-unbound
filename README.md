# Run Pi-Hole + Unbound Using A MacVLAN And Custom Bridge Network On A Synology NAS With Docker-Compose

## Description

Using docker-compose, with the included docker-compose.yaml file, you can run Pi-Hole + Unbound, each in its own container, on a Synology NAS.  The docker-compose.yaml file will also create a MacVLAN and a custom bridge network for the containers.  The MacVLAN network will be a /30 subnet, allowing for two assignable IP addresses from your Local LAN that will be assigned to the individual containers.  The custom bridge network will be a /32 subnet, allowing for 1 assignable IP address that will be assigned to the Pi-Hole container.

Much of the setup for this project is influenced by my [Pi-Hole Docker Synology NAS Setup Guide](https://youtu.be/1yG0p9gU104) and [Unbound Pi-Hole Synology Setup](https://youtu.be/-546g1w_L3w) videos, which I encourage you to view for details on how things are is setup. As an overview, the videos describe how to setup Pi-Hole and Unbound through the command line and from the Docker package from within DSM. This generally works great except on reboot Unbound may start up prior to Pi-Hole which is a problem.  The work around is to manually stop both containers, then restart the containers in the proper order or use a custom startup script to stop the containers and start them up again in the proper order (see the pinned comment from the second video listed above).  The setup configured in this project resolves this issue.

## Directions

You'll need to make sure the prerequisites are fulfilled on your Synology NAS first, then run through the setup steps below. I also run through setting everything up in this video here -> VIDEO NOT DONE YET.

### Prerequistes
On your Synology NAS you'll need to do the following:
1. Install the Docker package.
2. Install the Git Server package.
3. Enable SSH.

### Setup Steps
1. SSH into your Synology NAS.
2. Change directory to /volume1/docker. 
```
cd /volume1/docker
```
4. Run `sudo git clone https://github.com/digtalaloha/synology-docker-pihole-unbound.git`.
5. Change directory into synology-docker-pihole-unbound.
6. Run `mkdir -p pihole/pihole`.
7. Run `mkdir -p pihole/dnsmasq.d`.
8. Edit the .env file to your specific environment (placeholder content added into the file is what I used in my setup).
9. Run `sudo docker-compose up -d` to run the containers.

## References
* https://github.com/chriscrowe/docker-pihole-unbound/
* https://github.com/pi-hole/docker-pi-hole#quick-start
