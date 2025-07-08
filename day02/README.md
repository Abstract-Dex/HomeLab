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
