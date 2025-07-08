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
