# Intrusion Detection

> NOTE: The more rules enabled, the more RAM and processing it will need.
> 

## Settings tab

- **General Settings:**
    - **Enabled**: `Check`
    - **IPS mode**: `Check`
    - **Promiscuous mode**: `Check`
    - **Interface**: `WAN`
- **Detection:**
    - **Pattern matcher**: `Default`
- **Logging:**
    - **Enable syslog alerts**: `Check`
    - **Enable eve syslog output**: Unchecked
    - **Rotate log**: `Weekly`
    - **Save logs**: `4`
- Click on the **Apply** button

## Download Tab

- **Adding rules**
    - Select the desired set by adding a `Check` to it.
    - Click on the **Enable selected** button, a ✅ icon will appear in the Enabled column.
    - Click the **Download & Update** button at the bottom to download the rules.
    - They will appear in the **Rules** tab
- **Removing rules**
    - Select the desired set to remove by adding a `Check` to it.
    - Click on the **Disable selected** button, a ❌ icon will appear in the Enabled column.
    - Click the **Download & Update** button at the bottom to remove the rules.
    - They will be removed from the **Rules** tab