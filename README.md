# 🧪 **HomeLab**

Welcome to the documentation of my personal **home lab setup** – a flexible and scalable environment for **testing**, **learning**, and **development**.

---

## 🗂️ **Overview**

This home lab is designed to provide a robust, flexible environment for:

- 🔧 Experimenting with new tools and platforms
- 🎓 Learning system administration, networking, and virtualization
- 🧪 Running isolated test environments for development

---

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

🔄 After installation, I **updated the system** via the Proxmox web interface under the **“Updates”** tab to ensure I had the latest patches and features.

---

## 📅 **Day 2 – Remote Access & VMs**

### 🌐 **TailScale VPN Setup**

To access Proxmox from anywhere 🌍, I set up a **TailScale VPN** server on the Proxmox machine.

#### ❓ What is TailScale?

> 🔐 **TailScale** is a mesh VPN built on **WireGuard**, offering simple and secure connectivity between devices – no port forwarding required!

📥 [Download TailScale](https://tailscale.com/download/linux/)

#### 🛠️ Installation Steps:

```bash
sudo apt-get update
sudo apt-get install tailscale
```

```bash
sudo tailscale up
systemctl enable tailscaled
```

🔑 I logged into my **TailScale account** to authenticate and add the server to my private network.

---

### 💻 **Creating Ubuntu Server VM**

I uploaded the **Ubuntu Server ISO** to Proxmox and created a new VM.

#### 🧾 Ubuntu VM Specs:

- **CPU**: 2 cores
- **RAM**: 2 GB
- **Storage**: 32 GB
- **Network**: Bridged mode (for LAN access)

🧰 The installation was straightforward – selected the ISO, followed the guided setup, and done!

---

### 📦 **Deploying a Debian Container (LXC)**

I also created a **Debian container** from Proxmox's built-in templates.

#### 🧾 Container Specs:

- **CPU**: 1 core
- **RAM**: 512 MB
- **Storage**: 8 GB
- **Network**: Bridged mode

⚡ **LXC containers** are much faster to deploy and use fewer resources than full VMs.

---

### 🔄 **Post-Installation Updates**

Immediately after deploying both the **Ubuntu VM** and **Debian container**, I ran:

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

📦 This ensured both systems were up-to-date and ready for further customization and service deployment.

---

## 📅 **Day 3 – NAS Configuration**

### 🗄️ Setting Up a TrueNAS-Based NAS on Proxmox

With an additional 1TB hard drive available, I set up a **TrueNAS** server as a virtual machine within my **Proxmox** environment to handle centralized data storage and sharing across devices in my network.

---

### 🔍 What is TrueNAS?

**TrueNAS** is a powerful, open-source storage operating system based on FreeBSD. It provides robust data management capabilities and supports a variety of protocols including:

- **SMB (Windows/macOS file sharing)**
- **NFS (Linux sharing)**
- **iSCSI (block storage for virtualization environments)**

This makes it ideal for setting up a **reliable and flexible NAS (Network Attached Storage)** for personal or professional use.

---

### ⚙️ Installation Steps

#### 1. **Download the ISO**

- Retrieved the **TrueNAS ISO** from the [official TrueNAS website](https://www.truenas.com/download-truenas-community-edition/).

#### 2. **Upload ISO to Proxmox**

- Logged into the **Proxmox Web UI** and uploaded the ISO under:

  ```
  Datacenter > Storage > local > ISO Images
  ```

#### 3. **Create the TrueNAS VM**

- Created a new VM with the following specs:

  - **4 vCPUs**
  - **8 GB RAM**
  - **20 GB SSD (nvme) for OS**

- Attached my **1TB HDD** to the VM as secondary storage:

  - Opened Proxmox shell:

    ```bash
    lsblk  # Identify the HDD
    ls /dev/disk/by-id/  # Get persistent disk path
    ```

  - Added the disk using:

    ```bash
    qm set <VMID> --scsi1 /dev/disk/by-id/<disk-id>
    ```

#### 4. **Boot and Install TrueNAS**

- Booted the VM into the ISO and installed TrueNAS on the virtual disk (not the HDD).
- Configured basic network settings to ensure LAN access.

#### 5. **Access the Web Interface**

- Opened the TrueNAS web UI using:

  ```
  https://<TrueNAS_IP>
  ```

- Updated the system to the latest version using the **System > Update** section.

---

### 🧱 Storage Configuration

#### 🧩 6. **Create Storage Pool**

- Used the attached **1TB HDD** to create a new **ZFS pool**.
- TrueNAS makes this straightforward via the web UI's **Storage > Pools** section.

#### 📁 7. **Create Dataset & SMB Share**

- Created a **dataset** within the pool to organize files logically.
- Enabled **SMB (Windows/macOS sharing)** and created a share pointing to the dataset.

#### 👤 8. **Configure Access Permissions**

- Added a new user in **Credentials > Users** with access to the SMB share.
- Set dataset ACLs to allow that user read/write access.

---

### 🍏 Accessing the NAS on macOS

To connect from my Mac:

1. Opened **Finder**, then pressed `⌘ + K` (or **Go > Connect to Server**).
2. Entered the SMB address:

   ```
   smb://<TrueNAS_IP>/<Share_Name>
   ```

3. Entered the **username and password** set in TrueNAS.
4. Clicked **Connect** to mount the NAS share — it now appears in Finder as a mounted drive.
