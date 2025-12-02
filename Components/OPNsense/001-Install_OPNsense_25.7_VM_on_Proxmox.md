# Install a OPNsense 25.7 VM on Proxmox

> NOTE: All IP address and subnets has been changed for my own network security & privacy
> 

## Download OPNsense ISO & upload to Proxmox ISO images local storage

1. Download the `OPNsense-25.7-dvd-amd64.iso.bz2` image from [https://opnsense.org/download/](https://opnsense.org/download/)
2. Decompress the `.bz2` file
3. In Proxmox, cick on the **local** drive and select **ISO Images**
4. Click on the **Upload** button and select the decompressed file `OPNsense-25.7-dvd-amd64.iso`
5. Click on the **Upload** button at the bottom to upload the ISO to the server

## Network Interface in Proxmox Information

We need two Linux bridges on your Proxmox host:

- **`vmbr0` (WAN):** This will be a bridged to your physical network interface (e.g., `enp1s0`) that connects to your personal router (192.168.1.1/24). This allows OPNsense to request a DHCP IP and reach the internet
- **`vmbr1` (LAN):** This will be an **isolated** internal bridge. Machines connected to this bridge can only talk to OPNsense, and not to other machines on your physical 192.168.1.1/24 network. But the internet will be reachable

## Create Network Interface in Proxmox

1. Click on the **Proxmox** node, select **System > Network**
2. By default, `vmbr0` interface should be already present with the **CIDR**(`192.168.1.40/24`) (Proxmox IP address) and **Gateway**(`192.168.1.1`) defined
3. Click on the **Create** button and select **Linux Bridge**, the `vmbr1` interface will be added
4. Double-click on `vmbr1` and make sure **Autostart** and **VLAN aware** are checked. Click the **OK** button to close the window
5. Click the **Apply configuration** button to activate the interface

## Initializing the VM configuration

1. Click on the **Create VM** on the top-right
2. In the **Name**, write the name of the VM: `OPNsense`
3. OS section:
    1. Select the **Storage** where the ISO is stored: `local`
    2. Select the **ISO Image**: `OPNsense-25.7-dvd-amd64.iso` 
4. System section: You can leave by default
5. Disks section:
    1. Select the **Storage** where the disk will be stored : `local-lvm`
    2. Choose a **Disk size (GiB)**: `32`
6. In the CPU section, select the number of **Cores** to assign: `4`
7. Memory section:
    1. Write the required **Memory (MiB)**: `4096`
    2. Write the **Minimum Memory (MiB)**: `4096`
8. Network section: You can leave by default
9. Confirm section: Click on the **Finish** button

## VM Interface Mapping

After creating your OPNsense VM, map the two network interfaces to the Proxmox bridges within the **Hardware** section of the VM. By default there’s only one **Network Device**, add the second one by clicking **Add > Network Device**

1. **Network Device (`net0`):** Connect to **`vmbr0` (WAN)**
    - This interface will receive a DHCP address from your router (192.168.1.1/24)
2. **Network Device (`net1`):** Connect to **`vmbr1` (LAN)**
    - This interface will be the OPNsense LAN (172.16.50.1/24)
