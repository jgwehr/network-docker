# network-docker
This project aims to provide a basic set of networking tools. Docker is used as much as possible. It's intended as a "set and forget" - a pihole which can be left at a non-technical friend or family member network.

### Goals
- Stability
- Remote management
- Run on minimal hardware, such as a RaspberryPi Zero



# Initial Setup (with GUI)
1. set up timezone, wifi
1. Set up the Hostname: `sudo raspi-config` > System Options > Hostname
1. `sudo apt update && sudo apt upgrade`
1. Install dig for troubleshooting: `sudo apt-get install dnsutils`

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
1. `git config --global user.name "your name"`
1. `git config --global user.email "youremail@domain.com"`
1. `cd ~`
1. `git clone https://github.com/jgwehr/network-docker.git Docker` *Stores the local contents in a directory named `Docker`*

## Build directories

1. `mkdir -p /srv/config/{unbound,pihole/{dnsmasq.d,pihole}}`
1. `sudo chown -R $USER:$USER /srv`
1. Copy Unbound Files: `cp staticconfig/unbound/a-records.conf /srv/config/unbound/`
1. Copy Unbound Files: `cp staticconfig/unbound/unbound.conf /srv/config/unbound/`


## Starting Services
### Customize .env

### Create the Docker network
`docker network create -d bridge --subnet 172.20.0.0/16 dns-net`


# General
_credit to https://github.com/willy-wagtail/raspberrypi_
## SD Card Health
To properly shutdown, run `sudo shutdown -h now`.

## Energy Use
Run the command `/usr/bin/tvservice -o` to disable HDMI. Also run `sudo nano /etc/rc.local` and add the command there too in order to disable HDMI on boot.

(To enable again, run `/usr/bin/tvservice -p`, and remove from /etc/rc.local).

## Updating and upgrading rasp pi OS

Run `sudo apt update`, then `sudo apt full-upgrade -y`, and finally `sudo apt clean` to clean up the downloaded package files.

## Restrict ssh accounts
1. Run `sudoedit /etc/ssh/sshd_config`
1. Under the line “# Authentication”, add `AllowUsers <account_name1> <account_name2>`
1. After the change, you will need to restart the sshd service using `sudo systemctl restart ssh` or rebooting.
## Installing Docker
1. Move to a download folder: `cd ~/Downloads`
    1. Copy Docker's installation script to this directory: `curl -fsSL https://get.docker.com -o get-docker.sh`
    1. Run the installation script: `sudo sh get-docker.sh`