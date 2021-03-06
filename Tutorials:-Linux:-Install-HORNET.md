This is a small tutorial on how to install HORNET using our apt repository.

### Preparations

- Setup an e.g. Ubuntu (>= 16.04) server with at least 1GB RAM
- Optional but recommended: Add a ssh key, activate a firewall,...
- Update your system with:<br>
  ```bash
  sudo apt update
  sudo apt upgrade
  ```
- Allow basic ports in your firewall settings (and your router if you run HORNET behind one).<br>
  The following ports are important for a flawless node operation.<br>
  Please note that these ports can be customized in your `config.json`:<br>
  ```
  14626 UDP - Autopeering port
  15600 TCP - Gossip (neighbors) port
  14265 TCP - API port (optional if you don't want to access your node's API from external)
  ```
  Please also have a look at the [configuration documentation](https://github.com/gohornet/hornet/wiki/Configuration)

### Installation

- Import the key that is used to sign the release
  ```bash
  wget -qO - https://ppa.hornet.zone/pubkey.txt | sudo apt-key add -
  ```
- Add the HORNET repository to your APT sources<br>
  Stable:
  ```bash
  sudo sh -c 'echo "deb http://ppa.hornet.zone stable main" >> /etc/apt/sources.list.d/hornet.list'
  ```
  Testing (incl. pre-releases):<br>
  ```bash
  sudo sh -c 'echo "deb http://ppa.hornet.zone testing main" >> /etc/apt/sources.list.d/hornet.list'
  ```
- Install HORNET:
  ```bash
  sudo apt update
  sudo apt install hornet
  ```
- Adapt the settings to your needs (e.g. [setup for comnet](#comnet-community-network-setup))
- Enable the HORNET service:
  ```bash
  sudo systemctl enable hornet.service
  ```
- Start HORNET:
  ```bash
  sudo service hornet start
  ```
- Watch the logs with:
  ```bash
  sudo journalctl -u hornet.service -f
  ```
- Done

### Operation

- Stop HORNET
  - `sudo service hornet stop`
- Start HORNET
  - `sudo service hornet start`
- Watch the logs
  - `sudo journalctl -u hornet.service -f`
- Remove the mainnetdb (e.g. in case of a failure):
  1. Stop HORNET
  2. `sudo rm -r /var/lib/hornet/mainnetdb`
  3. Start HORNET

### Configuration

You can find HORNET's configuration files in:<br>
`/var/lib/hornet/`<br>

Additional cli arguments can be set in:<br>
`/etc/default/hornet`<br><br>

If you have modified the `config.json`, you have to restart HORNET:<br>
`sudo service hornet stop && sudo service hornet start`<br>
or<br>
`sudo service hornet restart`

### Comnet (community network) setup

1. Edit `/etc/default/hornet`:
   ```bash
   sudo nano /etc/default/hornet
   ```
   `/etc/default/hornet`:
   ```
   # Add cli arguments to hornet, e.g.:
   # (For the full list of cli options run 'hornet -h')
   OPTIONS="--config config_comnet"
   ```
2. Start HORNET
