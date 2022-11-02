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
1. `git clone https://github.com/jgwehr/network-docker.git network-docker` *Stores the local contents in a directory named `Docker`*

## Build directories

1. `mkdir -p /srv/config/{unbound,fail2ban,pihole/{dnsmasq.d,pihole}}`
1. `sudo chown -R $USER:$USER /srv`
1. Copy Unbound Files: `cp staticconfig/unbound/a-records.conf /srv/config/unbound/`
1. Copy Unbound Files: `cp staticconfig/unbound/unbound.conf /srv/config/unbound/`
1. Copy Fail2Ban Files: `cp staticconfig/fail2ban/jail.local /srv/config/fail2ban/fail2ban/`


## Starting Services
### Customize .env
- You can safely leave all PORT variables as they are. But it's suggested you change `PORT_VPN`
- Provide `DUCKDNS_SUBDOMAINLIST` and `DUCKDNS_TOKEN` via duckdns.org. This is a free DDNS.
- While your at it, the subdomain you provided for `DUCKDNS_SUBDOMAINLIST` is also used for `WG_SERVERURL`. If you're SUBDOMAINTLIST is "foo" then `WG_SERVERURL` should be  "foo.duckdns.org"
- Give a secure password for `PIHOLE_PASSWORD`
- The rest of the pihole variables are unncessary unless you're using DHCP
- **Wireguard**: `WG_PEERS` Provision the *number* of clients (eg. `3`) or specific *named* clients (eg. `myPC,myPhone,myTablet`)
- **Local network accessed via VPN**:  `WG_ALLOWED_IPS` Wireguard allow split tunnelling. Default is to send all traffic through this connection. I recommend changing this to your local network (eg. 192.168.0.0/24) so that only "local" traffic is sent to the vpn.

### Create the Docker network
`docker network create -d bridge --subnet 172.20.0.0/16 dns-net`


## Getting Remote Management
*Done through Wireguard*

### Configure Wireguard
1. Wireguard will automatically create configurations when the container starts.
1. You can get the QR codes using `docker logs --follow wireguard`. Alternatively, these files are located at `/srv/config/wireguard/<peer>` when using the default .env
1. Get the necessary Wireguard client https://www.wireguard.com/install/
1. Follow Wireguard's instructions. Generally speaking, you'll need a peer/client configuration from this server. Easily done on your phone via QR  code. Or, on a computer, by copying a `*.conf` file from the server to the client.

### Harden SSH
*via https://linuxhandbook.com/ssh-hardening-tips/*

1.  Open SSHD Config: `sudo nano /etc/ssh/sshd_config`
1.  Disable empty passwords: `PermitEmptyPasswords no`
2.  Change default SSH ports: `Port 2345`
5.  Configure idle timeout interval: `ClientAliveInterval 300`
5.  Configure idle timeout interval: `ClientAliveCountMax 2`



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