# 🛠️ Day 04 – Resolving a Proxmox Network Disconnection via IP Conflict

## 🔍 Issue

Proxmox server became unreachable after idle periods. Only a physical restart restored connectivity.

---

## 🔎 Key Discovery – IP Conflict via Network Scan

```bash
nmap -sn 192.168.1.0/24
```

**Result:** Proxmox was responding on two IPs:

- `192.168.1.69` (static, expected)
- `192.168.1.126` (DHCP-assigned)

The dual-IP state caused intermittent network loss.

---

## ✅ Fix: Remove DHCP IP and Disable DHCP Client

### 1. Kill DHCP Clients and Remove Extra IP

```bash
sudo killall dhclient
ip addr del 192.168.1.126/24 dev vmbr0
```

### 2. Verify `/etc/network/interfaces` (static only)

```ini
auto lo
iface lo inet loopback

iface enp4s0 inet manual
    post-up ethtool -s enp4s0 wol g

auto vmbr0
iface vmbr0 inet static
    address 192.168.1.69/24
    gateway 192.168.1.1
    bridge-ports enp4s0
    bridge-stp off
    bridge-fd 0
    post-up ethtool -s enp4s0 wol g

source /etc/network/interfaces.d/*
```

### 3. Disable DHCP on vmbr0

```bash
systemctl disable dhclient@vmbr0
systemctl stop dhclient@vmbr0
```

### 4. Restart Networking

```bash
systemctl restart networking
```

---

## ✅ Result

Server now retains a **single static IP** and maintains stable network access. Problem resolved.
