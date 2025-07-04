# HomeLab

Following is the documentation of my personal home lab setup.

## Overview

This home lab is designed to provide a flexible and scalable environment for testing, learning, and development.

## Day 1

The first day of the home lab setup involved the following steps:

1. **Hardware Setup**: My old PC was repurposed as the main server. It was equipped with sufficient RAM and storage to run multiple virtual machines. Following is my configuration:

   - CPU: Ryzen 5 1400
   - RAM: 16 GB
   - Storage: 1TB SSD
   - GPU: NVIDIA GTX 1050 Ti

2. **Software Installation**: I installed Proxmox VE as the hypervisor to manage virtual machines. Proxmox provides a web-based interface for easy management and supports various operating systems.

- **What is Proxmox VE?**: Proxmox Virtual Environment (VE) is an open-source server virtualization management platform that allows us to run virtual machines and containers. It is based on Debian Linux and uses KVM for virtualization. It is considered as a Type 1 hypervisor, meaning it runs directly on the hardware without a host operating system. The installation process was straightforward, their official website provided the ISO file and installation instructions. I created a bootable USB drive using and installed Proxmox on the server.

- **Installation Configuration**: During the installation, I configured the network settings by setting a static IP address for the Proxmox server. This allows me to access the Proxmox web interface from any device on my local network. I also set up a root password for secure access. Additionally I had to configure the storage settings to use the SSD for virtual machine storage. By default, the LVM storage was created but it was not using the entire SSD. I resized the storage to use the full capacity of the SSD. I followed [NetworkChuck's Guide](https://youtu.be/_u8qTN3cCnQ?si=YN1loWA8t-FmFq3s) for the installation and configuration of Proxmox VE.

Finally, I updated the Proxmox system to ensure I had the latest features and security patches. This was done through the web interface by navigating to the "Updates" section and applying all available updates.
