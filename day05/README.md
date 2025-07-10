# 🛠️ Day 05 – Plex Media Server Installation with TrueNAS NFS Storage

## 🎯 Goal

Set up Plex Media Server on an Ubuntu Server VM inside Proxmox, and attach TrueNAS NFS storage as its media library source.

---

## ✅ Step 1: Install Plex on Ubuntu Server (Proxmox VM)

### Key Commands:

The repositories must be added first and then the Plex Media Server package can be installed.

```bash
apt install curl gnupg -y

curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -

echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list

# Enable and start service
sudo systemctl enable plexmediaserver
sudo systemctl start plexmediaserver
```

Accessed Plex Web UI via:
`http://<vm-ip>:32400/web`

---

## ✅ Step 2: Attach NFS Storage from TrueNAS

Created a new dataset on TrueNAS for media files, and enabled NFS sharing.
Mounted `/mnt/Vault/media` dataset from TrueNAS to `/mnt/media` on the Ubuntu VM.

### A. Install NFS client

```bash
sudo apt install nfs-common
```

### B. Mount NFS share manually

```bash
sudo mkdir -p /mnt/media
sudo mount -t nfs <truenas_ip>:/mnt/Vault/media /mnt/media
```

### C. Make it persistent on boot

Added to `/etc/fstab`:

```bash
<truenas_ip>:/mnt/Vault/media /mnt/media nfs4 defaults,_netdev 0 0
```

---

### D. Verify NFS Mount

```bash
df -h #check for nfs mount
```

```bash
showmount -e <truenas_ip> #check available NFS exports
```

## ✅ Step 3: Set Permissions for Plex

Ensured Plex can read/write to the NFS share by setting appropriate permissions.

```bash
sudo chown -R plex:plex /mnt/media
sudo chmod -R 550 /mnt/media
```

---

## ✅ Step 4: Add Media Library to Plex

- Navigated to Plex Web UI → Add Library
- Selected `/mnt/media/Movies` as source
- Plex successfully scanned media

---

## Additional Notes

Tailscale can also be used to access the Plex server remotely so you can stream your media from anywhere. I added the Tailscale client to the VM and connected it to my Tailscale network.

## 🧪 Test

- Playback successful from local network and Tailscale
- Verified metadata fetch working
- Confirmed proper access without errors

---
