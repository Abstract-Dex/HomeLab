## 📅 **Day 1 – Initial Setup**

### 🖥️ **Hardware Setup**

I repurposed my old PC as the **main server**. It had enough resources to run multiple virtual machines comfortably.

#### 💻 Server Configuration:

- **CPU**: Ryzen 5 1400
- **RAM**: 16 GB
- **Storage**: 1TB SSD
- **GPU**: NVIDIA GTX 1050 Ti

---

### 💿 **Software Installation – Proxmox VE**

I installed **Proxmox VE** as the hypervisor to manage virtual machines and containers. Proxmox provides a **web-based interface**, making management easy and efficient.

#### ❓ What is Proxmox VE?

> 🧰 **Proxmox Virtual Environment (VE)** is an open-source, Type 1 hypervisor built on Debian Linux. It uses **KVM** for virtualization and **LXC** for containers. Since it runs directly on the hardware, it's highly efficient and stable.

📥 I downloaded the ISO from the official [Proxmox site](https://www.proxmox.com/) and created a **bootable USB** to install it.

---

### ⚙️ **Installation Configuration**

- 🌐 Set a **static IP** for the server to access it on the local network.
- 🔐 Configured a strong **root password** for secure access.
- 💾 Assigned the **entire SSD** to VM storage by resizing the default LVM volume.
- ✅ Followed [NetworkChuck's Guide](https://youtu.be/_u8qTN3cCnQ?si=YN1loWA8t-FmFq3s) for a smooth installation process.

### First things I did after installation:

#### Updating Proxmox without subscription

```bash
#configure the sources.list file
vim /etc/apt/sources.list

#add the following lines
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
deb http://download.proxmox.com/debian/ceph-quincy bookworm no-subscription

#then we update the package list
apt update

#finally, we can install the latest updates
apt dist-upgrade
```

#### Enabling IOMMU

```bash
#edit the GRUB configuration file
vim /etc/default/grub

#find the line starting with GRUB_CMDLINE_LINUX_DEFAULT and add the following parameters
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on" #for Intel CPUs
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on" #for AMD CPUs

#update GRUB
update-grub

#add the following line to /etc/modules
echo "vfio" >> /etc/modules
echo "vfio_iommu_type1" >> /etc/modules
echo "vfio_pci" >> /etc/modules
echo "vfio_virqfd" >> /etc/modules

#reboot the server
reboot
```
