# Synology NAS - Run Pi-Hole + Unbound Using A MacVLAN and Custom Bridge Network

## Description

Use docker-compose to run Pi-Hole + Unbound, each in its own Docker container, on a Synology NAS.  The docker-compose.yaml file will also create a /30
MacVLAN network for both the Pi-Hole and Unbound containers as well as a /32 custom bridge network for the Pi-Hole container.  

Much of the setup for this project is inspired by my [Unbound Pi-Hole Synology Setup](https://youtu.be/-546g1w_L3w) video which sets everything up through 
the command line and the Docker package from within DSM.  This generally works great except on reboot it seems Unbound always starts up prior to
Pi-Hole which is a problem.  The work around is to manually stop both containers then restart the containers in the proper order or use a custom 
script (see the pinned comment from the video).  The setup configured in this project resolves this issue.

## Directions

### Prerequistes

On your Synology NAS you'll need to.
1. Install the Docker and Git Server Packages
2. Enable SSH

### 



