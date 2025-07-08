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
