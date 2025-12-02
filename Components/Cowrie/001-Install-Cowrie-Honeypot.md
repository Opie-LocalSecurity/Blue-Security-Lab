# Install Corwie Honeypot

> NOTE: All IP address and subnets has been changed for my own network security & privacy
> 

## Install a Debian 13 container on Proxmox

### Get the CT Templates

1. Cick on the **local** drive and select **CT Templates**.
2. Click on the **Templates** button and in the **Search** bar, write `debian-13`.
3. Click on the **Download** button at the bottom to download the template.

### Initializing the container

1. Click on the **Create CT** on the top-right.
2. In the **Hostname**, write the name of the LXC Container: `cowrie`.
3. Uncheck the **Unpriviledged container** option.
4. Write a complex password and confirm it in the **Password / Confirm password** entries.
5. Template section:
    1. Select the **Storage** where the template is downloaded: `local`.
    2. In the **Template**, select the `debian-13-standard.xxx.xxx.tar.zst`.
6. Disks section:
    1. Select the **Storage** where the disk will be stored : `local-lvm`.
    2. Choose a **Disk size (GiB)**: `32`.
7. In the CPU section, select the number of **Cores** to assign: `1`.
8. Memory section:
    1. Write the required **Memory (MiB)**: `1024`.
    2. Write the **Swap (MiB)**: `512`.
9. Network section:
    1. **Bridge**: `vmbr1`.
    2. **IPv4 (static)**: `Selected`
    3. **IPv4/CIDR**: `172.16.50.25/24`.
    4. **Gateway (IPv4)**: `172.16.50.1`.
10. In the DNS section, write the **DNS server**: `9.9.9.9, 149.112.112.112`.
11. Confirm the confgihuration to add the container.
12. To manually fix a terminal problem, select the newly created container (`cowrie`).
13. Click on the **Options** category, double click on **Features** and check **Nesting**.
14. Click the **OK** button to confirm the modification and start the container.

## Start the Cowrie installation

### Update the container

1. Login as `root` in the Cowrie container.
2. Run the `apt update` command to update the repository.
3. Run the `apt upgrade` command the upgrade the installed packages.
4. Reboot the container

### Installing Cowrie

1. Installed the required dependencies: `apt install git python3-pip python3.13-venv libssl-dev libffi-dev build-essential libpython3-dev python3-minimal authbind iptables supervisor`
2. Add the `cowrie` user: `adduser --disabled-password cowrie`
3. Switch as the `cowrie` user: `su - cowrie`
4. Clone the Cowrie GitHub repository: `git clone http://github.com/cowrie/cowrie`

### Setup the Virtual Environement and install the Python3 dependencies

1. Create the cowrie-env virtual environement: `python3 -m venv cowrie-env`
2. Activate the cowrie-env virtual environement: `source ~/cowrie-env/bin/activate`
3. Upgrade PIP: `python -m pip install --upgrade pip`
4. Install the Python3 dependencies: `cd cowrie` and then `python -m pip install -e .`

---

On Debian, put the below in /etc/supervisor/conf.d/cowrie.conf:

```jsx
[program:cowrie]
command=/home/cowrie/cowrie/bin/cowrie start -n
directory=/home/cowrie/cowrie/
user=cowrie
autorestart=true
redirect_stderr=true
```

Iptables rules:

- SSH: `iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222`
- TELNET: `iptables -t nat -A PREROUTING -p tcp --dport 23 -j REDIRECT --to-port 2223`

Updating cowrie

```jsx
(cowrie-env) $ cowrie stop
(cowrie-env) $ git pull
(cowrie-env) $ python -m pip install --upgrade -e .
```