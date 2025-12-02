# Critical Firewall Rule: Preventing WAN DHCP Discovery

To prevent the OPNsense WAN from offering DHCP to your physical network (192.168.1.1/24), you must create an **outbound** firewall rule on the WAN interface. This rule prevents OPNsense from sending DHCP requests (on port 68) *out* of the WAN interface, which is an extra layer of caution, **but the crucial part is blocking DHCP server advertisements**. 

1. Navigate to **Firewall > Rules > WAN**.
2. Click **(+) Add** to create a new rule.
3. **Action:** **`Block`**
4. **Quick**: **`Check`** Apply the action immediately on match
5. **Interface:** **`WAN`**
6. **Direction:** **`Out`**
7. **TCP/IP Version:** **`IPv4`**
8. **Protocol:** **`UDP`**
9. **Source:**
    - **Type:** **`any`**
    - **Port:** **`68`** (DHCP client port)
10. **Destination:**
    - **Type:** **`any`**
    - **Port:** **`67`** (DHCP server port)
11. **Log**: **`Check`** Log packets that are handled by this rule
12. **Description:** `Block WAN outbound DHCP offers`
13. **Save** and **Apply changes**.

A more direct way to block the *server* function is by blocking traffic originating from port 67 on the WAN interface. Since OPNsense does not run a DHCP server on the WAN interface, no traffic will be *sent* out of WAN destined for port 67 (server port). The rule above strictly prevents any DHCP server traffic from exiting the OPNsense WAN interface.

1. Navigate to **Firewall > Rules > WAN**.
2. Click **(+) Add** to create a new rule.
3. **Action:** **`Block`**
4. **Quick**: **`Check`** Apply the action immediately on match
5. **Interface:** **`WAN`**
6. **Direction:** **`Out`**
7. **TCP/IP Version:** **`IPv4`**
8. **Protocol:** **`UDP`**
9. **Source:** 
    - Type: **`any`**
    - **Port:** **`67`** (DHCP server port)
10. **Destination:** 
    - Type: **`any`**
    - **Port:** **`68`** (DHCP client port)
11. **Log**: **`Check`** Log packets that are handled by this rule
12. **Description:** `Block all WAN outbound DHCP requests`
13. **Save** and **Apply changes**.