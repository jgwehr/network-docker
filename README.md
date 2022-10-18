# network-docker
This project aims to provide a basic set of networking tools. Docker is used as much as possible. It's intended as a "set and forget" - a pihole which can be left at a non-technical friend or family member network.

### Goals
- Stability
- Remote management
- Run on minimal hardware, such as a RaspberryPi Zero



# Initial Setup (with GUI)
1. set up timezone, wifi
1. `sudo apt update && sudo apt upgrade`

## Installing Docker
1. Move to a download folder: `cd ~/Downloads`
    1. Copy Docker's installation script to this directory: `curl -fsSL https://get.docker.com -o get-docker.sh`
    1. Run the installation script: `sudo sh get-docker.sh`

## Rootless Docker
1. Add your Users to the docker group: `sudo usermod -aG docker pi`
2. Restart the sytem
1. Validate: `docker version`
1. Test: `docker run hello-world`


## Lay the foundation with Git
1. `sudo apt install git`
1. `cd ~`
1. `git clone https://github.com/jgwehr/network-docker.git Docker` *Stores the local contents in a directory named `Docker`*

## Build directories
1. `sudo chown -R $USER:$USER /data`
1. `mkdir -p /srv/config/{unbound,pihole/{dnsmasq.d,pihole}}`
## Installing Docker
1. Move to a download folder: `cd ~/Downloads`
    1. Copy Docker's installation script to this directory: `curl -fsSL https://get.docker.com -o get-docker.sh`
    1. Run the installation script: `sudo sh get-docker.sh`