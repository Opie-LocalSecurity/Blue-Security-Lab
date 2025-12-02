# OPNsense Web GUI Configuration

> NOTE: All IP address and subnets has been changed for my own network security & privacy
> 

Access OPNsense from a client VM connected to **`vmbr1`** by navigating to `https://172.16.50.1`

## Configure WAN Interface

Ensure the WAN is configured for DHCP, which should be the default, but verify:

1. Navigate to **Interfaces > [WAN]**
2. **IPv4 Configuration Type:** **`DHCP`** (This fulfills the requirement of getting an IP from 192.168.1.1/24 network)
3. **Generic Configuration:** Ensure **Block private networks** and **Block bogon networks** are **unchecked** (especially the private networks block, as your WAN is on a private network)
4. **Save** and **Apply changes**

*Verification: OPNsense should now be able to PING 8.8.8.8 and reach the internet. You can test in **Interface > Diagnostics > PING***

## Configure LAN DHCP Service

1. Navigate to **Services > Dnsmasq DNS & DHCP > General**
2. **Enable** is `checked` and **Interface** should be `LAN`
3. In the **DHCP ranges** section, 
    1. Start address be: `172.16.50.50`
    2. End address: `172.16.50.250`
4. **Save** and **Apply changes**.

## VM Client Setup and Verification

Client VM (e.g., Linux, Windows)

1. Create a new VM (e.g., a lightweight Linux or Windows machine).
2. **Network Adapter:** Connect this VM's network interface to the **`vmbr1`** Proxmox bridge.
3. Boot the VM.
4. Ensure its network adapter is set to **DHCP**.

Verification

- **LAN IP Check:** The VM should automatically receive an IP address in the `172.16.50.50` to `172.16.50.250` range from the OPNsense DHCP server.
- **Internet Access Check:** From the client VM, you should be able to:
    - Ping the OPNsense LAN interface: `ping 172.16.50.1` (Success)
    - Ping a public IP: `ping 8.8.8.8` (Success - This confirms WAN access and NAT is working)
    - Ping a public domain: `ping google.com` (Success - This confirms DNS is working)
