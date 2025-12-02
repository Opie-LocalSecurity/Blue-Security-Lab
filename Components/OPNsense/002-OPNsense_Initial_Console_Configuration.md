# OPNsense Initial Console Configuration

> NOTE: All IP address and subnets has been changed for my own network security & privacy
> 

After installing OPNsense and booting up, follow these steps via the console (usually done after the first boot prompt).

## OPNsense Installation

1. At the **login:** prompt, write `installer`. When **password:** appear, write `opnsense`
2. In the installer, select the keyboard. I’ll scroll down to the `French Canadian (accent keys)` keyboard and press the **<SPACEBAR>** to select it
3. Scroll up one time to select `Continue with ca-fr.kbd keymap` and press **<ENTER>** to continue
4. In the list of tasks, choose `Install (ZFS)` and press **<ENTER>** to continue
    1. In the **Select Virtual Device type:**, i’ll choose `stripe` because i have no other disks mapped and press **<ENTER>** to continue
    2. in the **zpool creation**, press the **<SPACEBAR>** to select `QEMU HARDDISK` and press **<ENTER>** to continue
    3. Confirm the **destruction of the disk data** by scrolling to `YES` and pressing **<ENTER>** to continue
5. In the **Final configuration** selection, press **<ENTER>** on **Root Password** to change the password
    1. Enter a new secure password in the text input and press **<ENTER>** to continue
    2. Confirm the same secure password in the text input and press **<ENTER>** to continue
6. Scroll down to **Complete Install** and press **<ENTER>**. Press **<ENTER>** again to select **Reboot now**
7. The system will reboot in 5 seconds

## Interface Assignment

At the console, you will be prompted to assign interfaces. The order of the interfaces may be randomized (e.g., `vtnet0`, `vtnet1`)

1. Choose option **`1) Assign Interface`**
2. Do you want to configure LAGGs now? Say `n` (No)
3. Do you want to configure VLANs now? Say `n` (No)
4. Enter the WAN interface name or ‘a’ for auto-detection: Say `vtnet0`. (It will connect to the interface `vmbr0` → 192.168.1.1/24)
5. Enter the LAN interface name or ‘a’ for auto-detection: Say `vtnet1`. (It will connect to the interface `vmbr1`)
6. Enter the Optional interface 1 name or ‘a’ for auto-detection: Press `<ENTER>`
7. It will display the assignment. Do you want to proceed? Say `y` (Yes)

## Configure LAN IP Address

From the main OPNsense console menu:

1. Choose option **`2) Set interface IP address`**
2. Choose option **`1 - LAN interface`**
3. Configure IPv4 address LAN interface via DHCP? Say `n` (No)
4. Enter the new LAN IPv4 address. Press <ENTER> for none: `172.16.50.1`
5. Enter the new LAN IPv4 subnet bit count (1 to 32): `24` (for 255.255.255.0)
6. For a WAN, enter the new LAN IPv4 upstream gateway address. For a LAN, press <ENTER> for none: Press `<ENTER>`
7. Configure IPv6 address LAN interface via WAN tracking? Say `n` (No)
8. Configure IPv6 address LAN interface via DHCP6? Say `n` (No)
9. Enter the new LAN IPv6 address. Press <ENTER> for none: Press `<ENTER>`
10. Do you want to enable the DHCP server on LAN? say **`y`** (Yes)
11. Enter the start address of the IPv4 client address range: `172.16.50.50`
12. Enter the end address of the IPv4 client address range: `172.16.50.250`
13. Do you want to change the web GUI protocol from HTTPS to HTTP? Say `n` (No)
14. Do you want to generate a new self-signed web GUI certificate? Say `n` (No)
15. Restore web GUI access defaults? Say `n` (No)

At this point, OPNsense is accessible via a VM connected to `vmbr1` at `172.16.50.1`

## Update OPNsense from console

From the main OPNsense console menu:

1. Choose option **`12) Update from console`**
2. Proceed with this action? Say `y` (Yes)
3. Press `q` to quit the displayed note
4. Once the update is completed, it will reboot automatically