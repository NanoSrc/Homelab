# Proxmox Automated Provisioning

Automated installation profiles for Kita and Bocchi.  
> [!CAUTION]
> **Important:** Check `filter.ID_NET_NAME` in the answer files.
> It must match the intended NIC port and be available during installation.
> Verify the interface name and link status before installing.
> Oh and don't forget to fill in the rest of the parameters anyway.


## Hosts

| Setting | Kita | Bocchi |
|---|---|---|
| Hardware | Dell PowerEdge R630 | HPE ProLiant DL180 Gen9 |
| Hostname | `kita.home.arpa` | `bocchi.home.arpa` |
| IP | `10.10.20.10/24` | `10.10.20.15/24` |
| Gateway / DNS | `10.10.20.1` | `10.10.20.1` |
| VLAN | 20 - SERVERS | 20 - SERVERS |

Both hosts use `Europe/Zurich` and Swiss locale settings. `home.arpa` is reserved for private home networks.

## Requirements

- Proxmox VE installation ISO
- Proxmox Auto Install Assistant
- Host-specific `answer.toml` files
- USB drive or other bootable installation media

## Validate answer files

```bash
proxmox-auto-install-assistant validate-answer kita-answer.toml
proxmox-auto-install-assistant validate-answer bocchi-answer.toml
```

Validation checks the file structure, not hardware compatibility.

## Build installation ISOs

```bash
proxmox-auto-install-assistant prepare-iso proxmox-ve_9.2-1.iso \
  --fetch-from iso \
  --answer-file kita-answer.toml \
  --output Proxmox-Kita-Auto.iso
```

```bash
proxmox-auto-install-assistant prepare-iso proxmox-ve_9.2-1.iso \
  --fetch-from iso \
  --answer-file bocchi-answer.toml \
  --output Proxmox-Bocchi-Auto.iso
```

Copy the generated ISOs to the ventoy / installation USB and boot the correct server.

## Network and SFP+ troubleshooting

Run these commands in the Proxmox shell.

### Check the NIC

```bash
lspci | grep -i -E 'ethernet|network'
ip -br link
lspci -nnk | grep -A 4 -i -E 'ethernet|network'
```

### Check SFP+ link and speed

Replace `INTERFACE` with the actual NIC name.

```bash
ip link set INTERFACE up
ethtool INTERFACE
```

Expected output:

```text
Speed: 10000Mb/s
Link detected: yes
```

### Check Proxmox networking

```bash
cat /etc/network/interfaces
ip -br addr
bridge link
ip route
```

Check driver and hardware messages:

```bash
dmesg | grep -iE 'sfp|ixgbe|i40e|ice|mlx|firmware|ethernet' | tail -50
```

### Test connectivity

```bash
ping -c 4 10.10.20.1
```

From a laptop on VLAN 20, test the relevant host:

- Kita: `https://10.10.20.10:8006`
- Bocchi: `https://10.10.20.15:8006`

For throughput testing, use `iperf3` between the host and another machine.

### Bridge configuration

`vmbr0` is the Proxmox virtual bridge. Its `bridge-ports` setting selects the physical connection.

Example:

```text
bridge-ports INTERFACE
```

Use the actual interface name. Keep the management IP on `vmbr0`.

Back up and edit the configuration:

```bash
cp /etc/network/interfaces /root/interfaces.backup
nano /etc/network/interfaces
```

Apply changes with:

```bash
ifreload -a
```

**Use the local console or iDRAC before changing network settings.** Incorrect settings can disconnect Proxmox.

A detected NIC does not guarantee network connectivity. Verify link speed, bridge membership, IP address, VLAN and gateway.
